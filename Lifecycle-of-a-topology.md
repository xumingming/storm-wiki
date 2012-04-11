**Starting a topology**

- "storm jar" command executes your class with the specified arguments. The only special thing that "storm jar" does is set the "storm.jar" environment variable for use by `StormSubmitter` later. [code](https://github.com/nathanmarz/storm/blob/0.7.1/bin/storm#L101)
- When your code uses `StormSubmitter.submitTopology`, `StormSubmitter` takes the following actions:
  - First, `StormSubmitter` uploads the jar if it hasn't been uploaded before. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/jvm/backtype/storm/StormSubmitter.java#L83)
    - Jar uploading is done via Nimbus's Thrift interface [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/storm.thrift#L200)
    - `beginFileUpload` returns a path in Nimbus's inbox
    - 15 kilobytes are uploaded at a time through `uploadChunk`
    - `finishFileUpload` is called when it's finished uploading
    - Here is Nimbus's implementation of those Thrift methods: [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L694)
  - Second, `StormSubmitter` calls `submitTopology` on the Nimbus thrift interface [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/jvm/backtype/storm/StormSubmitter.java#L60)
    - The topology config is serialized using JSON (JSON is used so that writing DSL's in any language is as easy as possible)
    - Notice that the Thrift `submitTopology` call takes in the Nimbus inbox path where the jar was uploaded

- Nimbus receives the topology submission. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L639)
- Nimbus normalizes the topology configuration. The main purpose of normalization is to ensure that every single task will have the same serialization registrations, which is critical for getting serialization working correctly. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L557) 
- Nimbus sets up the static state for the topology [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L661)
    - Jars and configs are kept on local filesystem because they're too big for Zookeeper. The jar and configs are copied into the path <nimbus local dir>/stormdist/<topology id>
    - `setup-storm-static` writes task -> component mapping into ZK
    - `setup-heartbeats` creates a ZK "directory" in which tasks can heartbeat
- Nimbus calls `mk-assignment` to assign tasks to machines [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L458) 
    - Assignment record definition is here: [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/common.clj#L25)
    - Assignment contains:
      - `master-code-dir`: used by supervisors to download the correct jars/configs for the topology from Nimbus
      - `task->node+port`: Map from a task id to the worker that task should be running on. (A worker is identified by a node/port pair)
      - `node->host`: A map from node id to hostname. This is used so workers know which machines to connect to to communicate with other workers. Node ids are used to identify supervisors so that multiple supervisors can be run on one machine. One place this is done is with Mesos integration.
      - `task->start-time-secs`: Contains a map from task id to the timestamp at which Nimbus launched that task. This is used by Nimbus when monitoring topologies, as tasks are given a longer timeout to heartbeat when they're first launched (the launch timeout is configured by "nimbus.task.launch.secs" config)
- Once topologies are assigned, they're initially in a deactivated mode. `start-storm` writes data into Zookeeper so that the cluster knows the topology is active and can start emitting tuples from spouts. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L504)

- TODO cluster state diagram (show all nodes and what's kept everywhere)

- Supervisor runs two functions in the background:
    - `synchronize-supervisor`: This is called whenever assignments in Zookeeper change and also every 10 seconds. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/supervisor.clj#L241)
      - Downloads code from Nimbus for topologies assigned to this machine for which it doesn't have the code yet. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/supervisor.clj#L258)
      - Writes into local filesystem what this node is supposed to be running. It writes a map from port -> LocalAssignment. LocalAssignment contains a topology id as well as the list of task ids for that worker. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/supervisor.clj#L13)
    - `sync-processes`: Reads from the LFS what `synchronize-supervisor` wrote and compares that to what's actually running on the machine. It then starts/stops worker processes as necessary to synchronize. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/supervisor.clj#L177)
    
- Worker processes start up through the `mk-worker` function [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/worker.clj#L67)
  - Worker connects to other workers and starts a thread to monitor for changes. So if a worker gets reassigned, the worker will automatically reconnect to the other worker's new location. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/worker.clj#L123)
  - Monitors whether a topology is active or not and stores that state in the `storm-active-atom` variable. This variable is used by tasks to determine whether or not to call `nextTuple` on the spouts. [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/worker.clj#L155)
  - The worker launches the actual tasks as threads within it [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/worker.clj#L178)
- Tasks are set up through the `mk-task` function [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/task.clj#L160)
  - Tasks set up routing function which takes in a stream and an output tuple and returns a list of task ids to send the tuple to [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/task.clj#L207) (there's also a 3-arity version used for direct streams)
  - Tasks set up the spout-specific or bolt-specific code with [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/task.clj#L241)
   
**Topology Monitoring**

- Nimbus monitors the topology during its lifetime
   - Schedules recurring task on the timer thread to check the topologies [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L623)
   - Nimbus's behavior is represented as a finite state machine [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L98)
   - The "monitor" event is called on a topology every "nimbus.monitor.freq.secs", which calls `reassign-topology` through `reassign-transition` [code](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L497)
   - `reassign-topology` calls `mk-assignments`, the same function used to assign the topology the first time. `mk-assignments` is also capable of incrementally updating a topology
      - `mk-assignments` checks heartbeats and reassigns workers as necessary
      - Any reassignments change the state in ZK, which will trigger supervisors to synchronize and start/stop workers
      
**Killing a topology** 

- killing a topology:
   - deactivate, *then* shutdown
   - managed with the event transition system
   - https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L63
   - creates an event in the future to remove the topology
   - changes the status of the topology to "killed"
   - if nimbus crashes while the topology is killed but not shutdown, it knows that the topology was killed because the status was changed so on startup will schedule that remove event again https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/nimbus.clj#L111
   - 
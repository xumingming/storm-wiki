Running topologies on a production cluster is similar to running in [[Local mode]]. Here are the steps:

1) Define the topology (Use [TopologyBuilder](http://nathanmarz.github.com/storm/doc/backtype/storm/topology/TopologyBuilder.html) if defining using Java)

2) Use [StormSubmitter](http://nathanmarz.github.com/storm/doc/backtype/storm/StormSubmitter.html) to submit the topology to the cluster. `StormSubmitter` takes as input the name of the topology, a configuration for the topology, and the topology itself. For example:

```java
Map conf = new HashMap();
conf.put(Config.TOPOLOGY_WORKERS, 20);
conf.put(Config.TOPOLOGY_MAX_SPOUT_PENDING, 5000);
StormSubmitter.submitTopology("mytopology", conf, topology);
```

3) Create a jar containing your code and all the dependencies of your code (except for Storm -- the Storm jars will be added to the classpath on the worker nodes). Submit the topology to the cluster using the `storm` client like so:

`storm jar allmycode.jar org.me.MyTopology arg1 arg2 arg3`

`storm jar` will submit the jar to the cluster and configure the `StormSubmitter` class to talk to the right cluster. In this example, after uploading the jar `storm jar` calls the main function on `org.me.MyTopology` with the arguments "arg1", "arg2", and "arg3".

You can find out how to configure your `storm` client to talk to a Storm cluster on [[Setting up development environment]].

### Common configurations

There are a variety of configurations you can set per topology. A list of all the configurations you can set can be found [here](http://nathanmarz.github.com/storm/doc/backtype/storm/Config.html). The ones prefixed with "TOPOLOGY" can be overridden on a topology-specific basis (the other ones are cluster configurations and cannot be overridden). Here are some common ones that are set for a topology:

1. **Config.TOPOLOGY_WORKERS**: This sets the number of worker processes to use to execute the topology. For example, if you set this to 25, there will be 25 Java processes across the cluster executing all the tasks. If you had a combined 150 parallelism across all components in the topology, each worker process will have 6 tasks running within it as threads.
2. **Config.TOPOLOGY_ACKERS**: This sets the number of tasks that will track tuple trees and detect when a spout tuple has been fully processed. Ackers are an integral part of Storm's reliability model and you can read more about them on [[Guaranteeing message processing]].
3. **Config.TOPOLOGY_MAX_SPOUT_PENDING**: This sets the maximum number of spout tuples that can be pending on a single spout task at once (pending means the tuple has not been acked or failed yet). It is highly recommended you set this config to prevent queue explosion.
4. **Config.TOPOLOGY_MESSAGE_TIMEOUT_SECS**: This is the maximum amount of time a spout tuple has to be fully completed before it is considered failed. This value defaults to 30 seconds, which is sufficient for most topologies. See [[Guaranteeing message processing]] for more information on how Storm's reliability model works.
5. **Config.TOPOLOGY_SERIALIZATIONS**: You can register more serializers to Storm using this config so that you can use custom types within tuples.


### Killing a topology

To kill a topology, simply run:

`storm kill {stormname}`

Give the same name to `storm kill` as you used when submitting the topology.

Storm won't kill the topology immediately. Instead, it deactivates all the spouts so that they don't emit any more tuples, and then Storm waits Config.TOPOLOGY_MESSAGE_TIMEOUT_SECS seconds before destroying all the workers. This gives the topology enough time to complete any tuples it was processing when it got killed.

### Updating a running topology

To update a running topology, the only option currently is to kill the current topology and resubmit a new one. A planned feature is to implement a `storm swap` command that swaps a running topology with a new one, ensuring minimal downtime and no chance of both topologies processing tuples at the same time. 

### Monitoring topologies

The best place to monitor a topology is using the Storm UI. The Storm UI provides information about errors happening in tasks and fine-grained stats on the throughput and latency performance of each component of each running topology.

You can also look at the worker logs on the cluster machines.
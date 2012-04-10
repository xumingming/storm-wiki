This section of the wiki is dedicated to explaining how Storm is implemented. You should have a good grasp of how to use Storm before reading these sections. 

[[Structure of the codebase]]
[[Lifecycle of a topology]]

- Cluster state
  - Metadata in Zookeeper
  - Code and configs managed separately
- Passing messages around
  - one "receiver port" (virtual port)
  - Kryo serialization
  - worker send thread
  - ZeroMQ
- Acking framework implementation
- How transactional topologies work
   - subtopology for TransactionalSpout
   - how state is stored in ZK
   - subtleties around what to do when emitting batches out of order
- Unit testing
  - time simulation
  - complete-topology
  - tracker clusters

This section of the wiki is dedicated to explaining how Storm is implemented. You should have a good grasp of how to use Storm before reading these sections. 

[[Structure of the codebase]]
- Cluster state
  - Metadata in Zookeeper
  - Code and configs managed separately
- Passing messages around
  - one "receiver port" (virtual port)
  - Kryo serialization
  - worker send thread
  - ZeroMQ
- Acking framework implementation

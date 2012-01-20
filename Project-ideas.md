
 * DSLs for non-JVM languages: These DSL's should be all-inclusive and not require any Java for the creation of topologies, spouts, or bolts. Since topologies are [Thrift](http://thrift.apache.org/) structs, Nimbus is a Thrift service, and bolts can be written in any language, this is possible.
 * Higher level abstraction: There's a huge opportunity to build something like [Cascading](http://www.cascading.org/) on top of Storm's primitives. A good higher level abstraction should include:
   * Batching capabilities: auto-batch updates to a database and delay acking of tuples until the database operation has been completed
   * Streaming joins
   * Windowed aggregations
   * Rolling aggregations
   * More composable operations
 * Online machine learning algorithms: Something like [Mahout](http://mahout.apache.org/) but for online algorithms
 * Suite of performance benchmarks: These benchmarks should test Storm's performance on CPU and IO intensive workloads. There should be benchmarks for different classes of applications, such as stream processing (where throughput is the priority) and distributed RPC (where latency is the priority). 
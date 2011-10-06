Storm is a distributed realtime computation system. Similar to how Hadoop provides a set of general primitives for doing batch processing, Storm provides a set of general primitives for doing realtime computation. Storm is simple, can be used with any programming language, and is a lot of fun to use!

### Read these first

* [[Rationale]] 
* [[Setting up development environment]]
* [[Creating a new Storm project]]
* [[Tutorial]]

### Getting help

Feel free to ask questions on Storm's mailing list: http://groups.google.com/group/storm-user

You can also come to the #storm-user room on [freenode](http://freenode.net/). You can usually find a Storm developer there to help you out.

### Related projects

* [storm-deploy](http://github.com/nathanmarz/storm-deploy): One click deploys for Storm clusters on AWS\
* [storm-kestrel](https://github.com/nathanmarz/storm-kestrel): Adapter to use Kestrel as a spout within Storm topologies
* [storm-amqp-spout](https://github.com/rapportive-oss/storm-amqp-spout): Adapter to use AMQP source as a spout within Storm topologies
* [io-storm](https://github.com/gphat/io-storm): Perl support for Storm
* [storm-json](https://github.com/rapportive-oss/storm-json): Simple JSON serializer for Storm

### Documentation

#### Basics

* [Javadoc](http://nathanmarz.github.com/storm/doc/index.html)
* [[Concepts]]
* [[Guaranteeing message processing]]

#### Setup and deploying

* [[Setting up a Storm cluster]]
* [[Running topologies on a production cluster]]
* [[Local mode]]
* [[Maven]]

#### Intermediate

* [[Serialization]]
* [[Common patterns]]
* [[Clojure DSL]]
* [[Using non-JVM languages with Storm]]
* [[Distributed RPC]]
* [[Direct groupings]]
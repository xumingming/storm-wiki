There are three distinct layers to Storm's codebase.

First, Storm was designed from the very beginning to be compatible with multiple languages. Nimbus is a Thrift service and topologies are defined as Thrift structures. The usage of Thrift allows Storm to be used from any language.

Second, all of Storm's interfaces are specified as Java interfaces. So even though there's a lot of Clojure in Storm's implementation, all usage must go through the Java API. This means that every feature of Storm is always available via Java.

Third, Storm's implementation is largely in Clojure. Line-wise, Storm is about half Java code, half Clojure code. But Clojure is much more expressive, so in reality the great majority of the implementation logic is in Clojure. 

The following sections explain each of these layers in more detail.

**storm.thrift**

The first place to look to understand the structure of Storm's codebase is the [storm.thrift](https://github.com/nathanmarz/storm/blob/0.7.1/src/storm.thrift) file.

Every spout or bolt in a topology is given a user-specified identifier called the "component id". The component id is used to specify subscriptions from a bolt to the output streams of other spouts or bolts. A [StormTopology]9https://github.com/nathanmarz/storm/blob/0.7.1/src/storm.thrift#L91) structure contains a map from component id to component for each type of component (spouts and bolts).

Spouts and bolts have the same Thrift definition, so let's just take a look at the [Thrift definition for bolts](https://github.com/nathanmarz/storm/blob/0.7.1/src/storm.thrift#L79). It contains a `ComponentObject` struct and a `ComponentCommon` struct.

The `ComponentObject` defines the implementation for the bolt. It can be one of three types:

1. A serialized java object (that implements [IBolt](https://github.com/nathanmarz/storm/blob/0.7.1/src/jvm/backtype/storm/task/IBolt.java))
2. A `ShellComponent` object that indicates the implementation is in another language. Specifying a bolt this way will cause Storm to instantiate a [ShellBolt](https://github.com/nathanmarz/storm/blob/0.7.1/src/jvm/backtype/storm/task/ShellBolt.java) object to handle the communication between the JVM-based worker process and the non-JVM-based implementation of the component.
3. A `JavaObject` structure which tells Storm the classname and constructor arguments to use to instantiate that bolt. This is useful if you want to define a topology in a non-JVM language. This way, you can make use of JVM-based spouts and bolts without having to create and serialize a Java object yourself.

`ComponentCommon` defines everything else for this component. This includes:

1. What streams this component emits and the metadata for each stream (whether it's a direct stream, the fields declaration)
2. What streams this component consumes (specified as a map from component_id:stream_id to the stream grouping to use)
3. The parallelism for this component
4. The component-specific [configuration](https://github.com/nathanmarz/storm/wiki/Configuration) for this component

Note that the structure spouts also have a `ComponentCommon` field, and so spouts can also have declarations to consume other input streams. Yet the Storm Java API does not provide a way for spouts to consume other streams, and if you put any input declarations there for a spout you would get an error when you tried to submit the topology. The reason that spouts have an input declarations field is not for users to use, but for Storm itself to use. Storm adds implicit streams and bolts to the topology to set up the [acking framework](https://github.com/nathanmarz/storm/wiki/Guaranteeing-message-processing), and two of these implicit streams are from the acker bolt to each spout in the topology. The acker sends "ack" or "fail" messages along these streams whenever a tuple tree is detected to be completed or failed. The code that transforms the user's topology into the runtime topology is located [here](https://github.com/nathanmarz/storm/blob/0.7.1/src/clj/backtype/storm/daemon/common.clj#L188).

**Java interfaces**

All the interfaces for Storm are specified as Java interfaces. 

- always an interface first, then possibly a "base class" to provide default implementations
- TopologyBuilder to make it easy to build those thrift structs
- IBolt, ISpout vs. IRichBolt, IRichSpout

**Clojure implementation**

- 
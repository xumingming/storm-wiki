Storm comes with a Clojure DSL for defining spouts, bolts, and topologies. The Clojure DSL has access to everything the Java API exposes, so if you're a Clojure user you can code Storm topologies without touching Java at all. The Clojure DSL is defined in the source in the [backtype.storm.clojure](https://github.com/nathanmarz/storm/blob/master/src/clj/backtype/storm/clojure.clj) namespace.

This page outlines all the pieces of the Clojure DSL, including:

1. Defining topologies
2. defbolt
3. defspout

### Defining topologies

To define a topology, use the `topology` function. `topology` takes in two arguments: a map of "spout specs" and a map of "bolt specs". Each spout and bolt spec wires the code for the component into the topology by specifying things like inputs and parallelism.

Let's take a look at an example topology definition [from the storm-starter project](https://github.com/nathanmarz/storm-starter/blob/master/src/clj/storm/starter/clj/word_count.clj):

```clojure
(topology
 {1 (spout-spec sentence-spout)
  2 (spout-spec (sentence-spout-parameterized
                 ["the cat jumped over the door"
                  "greetings from a faraway land"])
                :p 2)}
 {3 (bolt-spec {1 :shuffle 2 :shuffle}
               split-sentence
               :p 5)
  4 (bolt-spec {3 ["word"]}
               word-count
               :p 6)})
```

The maps of spout and bolt specs are maps from the component id to the corresponding spec. The component ids must be unique across the maps. Just like defining topologies in Java, component ids are used to declare inputs for bolts in the topology.

#### spout-spec

`spout-spec` takes as arguments the spout implementation (an object that implements [IRichSpout](http://nathanmarz.github.com/storm/doc/backtype/storm/topology/IRichSpout.html)) and optional keyword arguments. The only existing option currently is the `:p` option, which specifies the parallelism for the spout. If you omit `:p`, the spout will execute as a single task.

#### bolt-spec

`bolt-spec` takes as arguments the input declaration for the bolt, the bolt implementation (an object that implements [IRichBolt](http://nathanmarz.github.com/storm/doc/backtype/storm/topology/IRichBolt.html)), and optional keyword arguments.

The input declaration is a map from stream ids to stream groupings. A stream id can have one of two forms:

1. `[==component id== ==stream id==]`: Subscribes to a specific stream on a component
2. `==component id==`: Subscribes to the default stream on a component

The value indicates a stream grouping, and can be one of the following:

1. `:shuffle`: subscribes with a shuffle grouping
2. Vector of field names, like `["id" "name"]`: subscribes with a fields grouping with the specified fields
3. `:global`: subscribes with a global grouping
4. `:all`: subscribes with an all grouping
5. `:direct`: subscribes with a direct grouping

See [[Concepts]] for more info on stream groupings. Here's an example input declaration showcasing the various ways to declare inputs:

```clojure
{[2 1] :shuffle
 3 ["field1" "field2"]
 [4 2] :global}
```

This input declaration subscribes to three streams total. It subscribes to stream 1 on component 2 with a shuffle grouping, subscribes to the default stream on component 3 with a fields grouping on the fields "field1" and "field2", and subscribes to stream 2 on component 4 with a global grouping.

Like `spout-spec`, the only current supported keyword argument for `bolt-spec` is `:p` which specifies the parallelism for the bolt.

#### shell-bolt-spec


### defbolt


### defspout

- can create storm topologies completely from clojure (has the full power of the java API)
- defbolt and defspout for creating components
- how to declare outputs, map or vector, direct-stream for declaring direct streams
- parameterized components
- prepared components


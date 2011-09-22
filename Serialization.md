Tuples can be comprised of objects of any types. Since Storm is a distributed system, it needs to know how to serialize and deserialize objects when they're passed between tasks. By default Storm can serialize ints, shorts, longs, floats, doubles, bools, bytes, strings, and byte arrays, but if you want to use another type in your tuples, you'll need to implement a custom serializer.

### Dynamic typing

There are no type declarations for fields in a Tuple. You put objects in and Storm figures out the serialization dynamically. Before we get to the interface for serialization, let's spend a moment understanding why Storm's tuples are dynamically typed.

Adding static typing to tuple fields would add large amount of complexity to Storm's API. Hadoop, for example, statically types its keys and values but requires a huge amount of annotations on the part of the user. Hadoop's API is a burden to use and the "type safety" isn't worth it. Dynamic typing is simply easier to use.

Further than that, it's not possible to statically type Storm's tuples in any reasonable way. Suppose a Bolt subscribes to multiple streams. The tuples from all those streams may have different types across the fields. When a Bolt receives a `Tuple` in `execute`, that tuple could have come from any stream and so could have any types. There might be some reflection magic you can do to declare a different method for every tuple stream a bolt subscribes to, but Storm opts for the simpler, straightforward approach of dynamic typing.

Finally, another reason for using dynamic typing is so Storm can be used in a straightforward manner from dynamically typed languages like Clojure and JRuby.

### Custom serialization

Let's dive into Storm's API for defining custom serializations. There are two steps you need to take as a user to create a custom serialization: implement the serializer, and register the serializer to Storm.

#### Creating a serializer

Custom serializers implement the [ISerialization](http://nathanmarz.github.com/storm/doc/backtype/storm/serialization/ISerialization.html) interface. Implementations specify how to serialize and deserialize types into a binary format.

The interface looks like this:

```java
public interface ISerialization<T> {
    public boolean accept(Class c);
    public void serialize(T object, DataOutputStream stream) throws IOException;
    public T deserialize(DataInputStream stream) throws IOException;
}
```

Storm uses the `accept` method to determine if a type can be serialized by this serializer. Remember, Storm's tuples are dynamically typed so Storm determines what serializer to use dynamically.

`serialize` writes the object out to the output stream in binary format. The field must be written in a way such that it can be deserialized later. For example, if you're writing out a list of objects, you'll need to write out the size of the list first so that you know how many elements to deserialize.

`deserialize` reads the serialized object off of the stream and returns it.

You can see the serialization implementations for the primitive types, strings, and byte arrays in the source for [SerializationFactory](https://github.com/nathanmarz/storm/blob/master/src/jvm/backtype/storm/serialization/SerializationFactory.java)

### Registering a serializer

Once you create a serializer, you need to tell Storm it exists. This is done through the Storm configuration (See [[Concepts]] for information about how configuration works in Storm). You can register serializations either through the config given when submitting a topology or in the storm.yaml files across your cluster.

Serializer registrations are done through the Config.TOPOLOGY_SERIALIZATIONS config and are a map from a unique token id and class name. Let's start with the token id, as it's a common point of confusion.

To identify fields in a tuple so that they can be deserialized, Storm needs to annotate each field with its type information. The simplest thing Storm could do is write out the classname, but this would cause a huge amount of overhead at runtime. High-performance serialization is critical to Storm's performance, so writing the classname for every field is unacceptable.

Instead, Storm requires you as a user to provide an identifier (the "token") that Storm can use to identify tuples on the wire. The token must be an integer greater than 32 (token values 32 and smaller are reserved by Storm). This small burden that Storm places on you has major performance benefits. And you only have to do it once per serializer if you register the serializers in your `storm.yaml` files.

Storm provides helpers for registering serializers in a topology config. The [Config](http://nathanmarz.github.com/storm/doc/backtype/storm/Config.html) class has a method called `addSerialization` that takes in a token and a serializer class.

There's an advanced config called Config.TOPOLOGY_SKIP_MISSING_SERIALIZATIONS. If you set this to true, Storm will ignore any serializations that are registered but do not have their code available on the classpath. Otherwise, Storm will throw errors when it can't find a serialization. This is useful if you run many topologies on a cluster that each have different serializations, but you want to declare all the serializations in the `storm.yaml` files.
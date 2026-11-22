---
tags:
  - Java
---
Serialization is the conversion of the state of an object into a byte stream; deserialization does the opposite. Stated differently, serialization is the conversion of a Java object into a static stream (sequence) of bytes, which we can then save to a database or transfer over a network.

# Serialization and Deserialization[](https://www.baeldung.com/java-serialization#serialization-and-deserialization)

The serialization process is instance-independent; for example, we can serialize objects on one platform and deserialize them on another. **Classes that are eligible for serialization need to implement a special marker interface,** *Serializable.*

*Both *ObjectInputStream* and *ObjectOutputStream* are high level classes that extend *java.io.InputStream* and *java.io.OutputStream,* respectively. *ObjectOutputStream* can write primitive types and graphs of objects to an *OutputStream* as a stream of bytes. We can then read these streams using *ObjectInputStream*.*

The most important method in *ObjectOutputStream* is:

```java
public final void writeObject(Object o) throws IOException;
```

This method takes a serializable object and converts it into a sequence (stream) of bytes. Similarly, the most important method in *ObjectInputStream* is:

```java
public final Object readObject() 
  throws IOException, ClassNotFoundException;
```

This method can read a stream of bytes and convert it back into a Java object. It can then be cast back to the original object.

# Java Serialization Caveats

## Inheritance and Composition[](https://www.baeldung.com/java-serialization#1-inheritance-and-composition)

When a class implements the *java.io.Serializable* interface, all its sub-classes are serializable as well. Conversely, when an object has a reference to another object, these objects must implement the *Serializable* interface separately, or else a *NotSerializableException* will be thrown:

```java
public class Person implements Serializable {
    private int age;
    private String name;
    private Address country; // must be serializable too
}

```

If one of the fields in a serializable object consists of an array of objects, then all of these objects must be serializable as well, or else a *NotSerializableException* will be thrown.

## Serial Version UID[](https://www.baeldung.com/java-serialization#2-serial-version-uid)

**The JVM associates a version (*long*) number with each serializable class.** We use it to verify that the saved and loaded objects have the same attributes, and thus are compatible on serialization.

Most IDEs can generate this number automatically, and it’s based on the class name, attributes, and associated access modifiers. Any changes result in a different number, and can cause an *InvalidClassException*.

If a serializable class doesn’t declare a *serialVersionUID*, the JVM will generate one automatically at run-time. However, it’s highly recommended that each class declares its *serialVersionUID,* as the generated one is compiler dependent and thus may result in unexpected *InvalidClassExceptions*.

## Custom Serialization in Java[](https://www.baeldung.com/java-serialization#3-custom-serialization-in-java)

Java specifies a default way to serialize objects, but Java classes can override this default behavior. Custom serialization can be particularly useful when trying to serialize an object that has some unserializable attributes. We can do this by providing two methods inside the class that we want to serialize:

```java
private void writeObject(ObjectOutputStream out) throws IOException;
```

and

```java
private void readObject(ObjectInputStream in) 
  throws IOException, ClassNotFoundException;
```

With these methods, we can serialize the unserializable attributes into other forms that we can serialize:

```java
public class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    private transient Address address;
    private Person person;

    // setters and getters

    private void writeObject(ObjectOutputStream oos) 
      throws IOException {
        oos.defaultWriteObject();
        oos.writeObject(address.getHouseNumber());
    }

    private void readObject(ObjectInputStream ois) 
      throws ClassNotFoundException, IOException {
        ois.defaultReadObject();
        Integer houseNumber = (Integer) ois.readObject();
        Address a = new Address();
        a.setHouseNumber(houseNumber);
        this.setAddress(a);
    }
}

public class Address {
    private int houseNumber;

    // setters and getters
}
```

We can run the following unit test to test this custom serialization:

```java
@Test
public void whenCustomSerializingAndDeserializing_ThenObjectIsTheSame() 
  throws IOException, ClassNotFoundException {
    Person p = new Person();
    p.setAge(20);
    p.setName("Joe");

    Address a = new Address();
    a.setHouseNumber(1);

    Employee e = new Employee();
    e.setPerson(p);
    e.setAddress(a);

    FileOutputStream fileOutputStream
      = new FileOutputStream("yourfile2.txt");
    ObjectOutputStream objectOutputStream 
      = new ObjectOutputStream(fileOutputStream);
    objectOutputStream.writeObject(e);
    objectOutputStream.flush();
    objectOutputStream.close();

    FileInputStream fileInputStream 
      = new FileInputStream("yourfile2.txt");
    ObjectInputStream objectInputStream 
      = new ObjectInputStream(fileInputStream);
    Employee e2 = (Employee) objectInputStream.readObject();
    objectInputStream.close();

    assertTrue(
      e2.getPerson().getAge() == e.getPerson().getAge());
    assertTrue(
      e2.getAddress().getHouseNumber() == e.getAddress().getHouseNumber());
}
```

In this code, we can see how to save some unserializable attributes by serializing *Address* with custom serialization. Note that we must mark the unserializable attributes as *transient* to avoid the *NotSerializableException.*
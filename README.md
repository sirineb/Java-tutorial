# Java-Core Cheat sheet
## Java Compilation / Runtime Notions
### Java compiler
Checks syntax rules, then writes bytecode in .class files. It differs from other language compilers, which write instructions for the CPU.
### The JVM
At runtime, the JVM interprets .class files and executes its instructions on the native hardware platform for which the JVM was written (Linux, Windows and mobile). The JVM interprets the bytecode just as a CPU would interpret assembly. The difference is that the JVM is a software.
### The garbage collector
Java keeps up with memory allocation for you. When a Java application creates an object instance at runtime, the JVM automatically allocates memory space for that object. The Java garbage collector runs in the background, keeping track of which objects the application no longer needs and reclaiming memory from them. 
### The Java Development Kit
The JDK includes the compiler and a complete class library of prebuilt utilities.
### The Java Runtime Environment
The JRE includes the JVM, code libraries, and components that are necessary for running programs that are written in the Java language.
## Java Language / Structure Notions
### What is an object?
An object is an entity that contains attributes and behavior. 
### Encapsulation
Using modifiers, it’s possible to vary the nature of object relationships from public to private.
### Inheritance
In structured programming, it's common to copy a structure and add or modify the attributes which generates duplicated code. OOP introduces inheritance, to "copy" the attributes and behavior of the source classes. If some of those attributes or behaviors need to change, you override them.
### Polymorphism
Objects belonging to the same branch of a hierarchy, when told to do the same thing, can manifest that behavior differently.
## Java Syntax Notions

### Access Specifier
* public: Any object in any package can see the variable. 	
* protected: Any object defined in the same package, or a subclass (in any package), can see the variable.
* No specifier: Only objects whose classes are defined in the same package can see the variable.
* private: 	Only the class containing the variable can see it. 

### Static and instance methods
There are two types of methods: instance and static. Static methods or class methods’ don't dependent on object's state. Object instance are not needed to invoke them. Just the name of the class.
```
public class Main {
    public static void main(String []args){
        System.out.println("Static method call: " + TestClass.staticMethod());
        //Creating an instance
        TestClass instance = new TestClass();
        System.out.println("Instance method call: " + instance.instanceMethod());
    }
}

class TestClass {
    private static int staticValue = 1;
    private int nonStaticValue = 0;
    
    public static int staticMethod(){
        return staticValue;
    }
    
    public int instanceMethod(){
        return nonStaticValue;
    }
}

```
### final classes, methods and fields
A final class is simply a class that can't be extended. This does not mean that all references to objects of the class would act as if they were declared as final.

Final method can't be overridden (for object scope) or hidden (for static). This allows the developer to create functionality that cannot be changed by subclasses. 

### abstract class
It cannot be instantiated and may have abstract methods (only abstract classes can have abstract methods). While an interface is used to make a class have a complete set of methods, an abstract class is used to group classes in a family. A class can only extend one abstract class but can implements multiple interfaces.

An abstract method cannot be implemented `public abstract void doAbstractStuff();`

The following combinations are illegal: abstract static, abstract final, abstract native, private abstract, synchronized abstract, abstract strictfp.
### interface
It is a blueprint that defines a set of behaviors to be implemented. An interface can only have constants, method signatures, default and static methods with implementations and nested types. 

An class can implement one interface or more. An interface can extend multiple interfaces. Constructors are not allowed in interfaces, and an interface cannot be instantiated.

Only public constants are allowed as fields in an interface. Public attributes or attributes with no modifier will be implicitly considered static final. 

Any method (except static or default starting java 8) are abstract and public. No private methods allowed (but starting from java 9 we can have concrete private methods). All abstract methods in an interface are public abstract implicitly. 

Java 8 enables implementing (default and static) methods inside of an interface. Then the difference between a java 8 interface and an abstract class is that interfaces are not a part of the class hierarchy and still does not allow constructors nor non final or private members.

### super and this
`super` is used to access methods of the base class while this is used to access methods of the current class. `super()` refers to constructor of the base class, and this() refers to the constructor of the very class where you are writing this code.

### equals and ==
If a class does not override the equals method, then it defaults to the equals(Object o) method of the closest parent class that has overridden this method. If no parent class provided an override, then it defaults to the method from the ultimate parent class, Object. Per the Object API this is the same as ==.
### Overloading and Overriding
1. Polymorphism applies to overriding, not to overloading.
2. Overriding is a runtime concept while overloading is a compile-time concept.

```
public class Main {
    public static void main(String []args){
        Child child = new Child();
        child.eatsMeat(); //prints "No, Vegan"
    }
}

class Parent{
    public void eatsMeat(){
        System.out.println("yes");
    }

    //overloading method
    public void eatsMeat(int timesInDay){
        if ( timesInDay > 1)
            System.out.println("No, too much");
        else
            System.out.println("Yes");
    }
}

class Child extends Parent{
    //overriding method
    public void eatsMeat(){
        System.out.println("No, Vegan");
    }
}
```
### Wildcards
`Collection<?>` (collection of unknown) is a collection whose element type matches anything called a wildcard type. The add method is forbidden: add takes arguments of type E, the element type of the collection. On the other hand, `get()` could be called. `List<? extends SomeClass>` is a bounded wildcard. The ? means this unknown type is a subtype of SomeClass. 

### Exceptions
A problem that arises during the execution of a program that disrupts the program flow and terminates it abnormally. 
* Checked exceptions − occur at the compile time, also called as compile time exceptions. 
* Unchecked exceptions − occur at the time of execution, also called Runtime Exceptions. These include programming bugs, such as logic errors or improper use of an API. Runtime exceptions are ignored at the time of compilation.

All exception classes are subtypes of the java.lang.Exception class. Errors are abnormal conditions that happen in case of severe failures, these are not handled by the Java programs. Example: JVM is out of memory.
#### The Throws/Throw Keywords
If a method does not handle a checked exception, the method must declare it using the throws keyword. You can throw an exception, either a newly instantiated one or an exception that you just caught, by using the throw keyword.
#### The Finally Block
The finally block follows a try block or a catch block. A finally block of code always executes.
### Serialization, externalization and the "transient" keyword
The serialization of objects allows making a byte sequence from any object that has implemented the Serializable interface; an turning that byte sequence back into an object. The mechanism does not depend on the operating system, allowing transferring objects via a network and restoring them.

To serialize an object, the OutputStream is put into the serialization stream called ObjectOutputStream. writeObject() is called to serialize the object for the output stream. To deserialize an object, the InputStream goes into ObjectInputStream and then call the readObject() method. 

In case of special requirements for the serialization, such as security-sensitive parts of the object, like passwords, the process of serialization could be controlled by implementing the Externalizable interface instead of Serializable. This interface extends the original Serializable interface and adds writeExternal() and readExternal(). 
Serialized information can be read in a file or in a captured network packet. In case of external information, nothing is written automatically. However, in a serializable object, everything is serialized there automatically. The serialization of any member could be forbidden with the transient modifier. 
### serialVersionUID
The serialization runtime associates with each serializable class a version number, called a serialVersionUID, which is used during deserialization to verify that the sender and receiver of a serialized object have loaded classes for that object that are compatible with respect to serialization. If the receiver has loaded a class for the object that has a different serialVersionUID than that of the corresponding sender's class, then deserialization will result in an  InvalidClassException. A serializable class can declare its own serialVersionUID explicitly by declaring a field named serialVersionUID that must be static, final, and of type long:
```
ANY-ACCESS-MODIFIER static final long serialVersionUID = 42L;
```
If a serializable class does not explicitly declare a serialVersionUID, then the serialization runtime will calculate a default serialVersionUID value for that class based on various aspects of the class, as described in the Java Object Serialization Specification. However, it is strongly recommended that all serializable classes explicitly declare serialVersionUID values, since the default serialVersionUID computation is highly sensitive to class details that may vary depending on compiler implementations, and can thus result in unexpected InvalidClassExceptions during deserialization. Therefore, to guarantee a consistent serialVersionUID value across different java compiler implementations, a serializable class must declare an explicit serialVersionUID value. It is also strongly advised that explicit serialVersionUID declarations use the private modifier where possible, since such declarations apply only to the immediately declaring class serialVersionUID fields are not useful as inherited members.

### java I/O, Files
The java.io package contains nearly every class to perform input and output (I/O) in Java. Stream is a sequence of data.
* Byte Streams are input and output of 8-bit bytes. The most used related classes are FileInputStream and FileOutputStream.
* Character Streams are input and output for 16-bit unicode. The most used related classes are FileReader and FileWriter. Internally FileReader uses FileInputStream and FileWriter uses FileOutputStream but the difference is that FileReader reads two bytes at a time and FileWriter writes two bytes at a time.
* Standard Streams: Standard Input (System.in), Standard Output (System.out) and Standard Error (System.err). 
* Directories in Java are a File which can contain a list of other files and directories. 	
### String, StringBuilder and StringBuffer
* String is immutable  (once created can not be changed) object. Every immutable object in Java is thread safe: cannot be used by two threads simultaneously. Once assigned, they cannot be changed.
* StringBuffer is mutable. It has the same methods as the StringBuilder, but each method in StringBuffer is synchronized that is StringBuffer is thread safe.
* StringBuilder is same as the StringBuffer. It stores the object in heap and it can also be modified. StringBuilder is not thread safe. StringBuilder is fast as it is not thread safe. 


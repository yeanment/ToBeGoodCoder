# Singleton Pattern

## Motivation
Sometimes it's important to have only one instance for a class. For example, in a system there should be only one window manager (only a file system or only a print spooler). Usually singletons are used for centralized management of internal or external resources and they provide a global point of access to themselves.

The singleton pattern involves only one class which is responsible to make sure there is no more than one instance. It does it by instantiating itself and in the same time provides a global point of access to that instance.

## Intent
- Ensure that only one instance of a class is created.
- Provide a global point of access to the object to ensure the same instance can be used from everywhere.
- Prevente direct invocation of the singleton constructor

## Implementation
The implementation involves a static member in the Singleton class which keeps the reference to the instance, a private constructor and a static public method `getInstance` that returns the static member reference to the unique instance which is accessed by the clients. Meanwhile, `getInstance()` is is responsible for creating its class unique instance in case it is not created yet and to return that instance, and the only way of instantiating the instance.

```C++
class Singleton {
private:
    static Singleton instance;

    Singleton() {
        ...
    }
    
public:
    static Singleton getInstance(){

        if (instance == null)
            instance = new Singleton();

        return instance;
    }
    
    ...
    
    void doSomething()
    {
        ...
    }
}
```

## Applicability & Examples
### Example 1 - Logger Classes
The Singleton pattern is used in the design of logger classes. This classes are ussualy implemented as a singletons, and provides a global logging access point in all the application components without being necessary to create an object each time a logging operations is performed.
### Example 2 - Configuration Classes
The Singleton pattern is used to design the classes which provides the configuration settings for an application. By implementing configuration classes as Singleton not only that we provide a global access point, but we also keep the instance we use as a cache object. When the class is instantiated( or when a value is read) the singleton will keep the values in its internal structure. If the values are read from the database or from files this avoids the reloading the values each time the configuration parameters are used.
### Example 3 - Factories implemented as Singletons
Let's assume that we design an application with a factory to generate new objects(Acount, Customer, Site, Address objects) with their ids, in an multithreading environment. If the factory is instantiated twice in 2 different threads then is possible to have 2 overlapping ids for 2 different objects. If we implement the Factory as a singleton we avoid this problem. Combining Abstract Factory or Factory Method and Singleton design patterns is a common practice.

## Specific problems and implementation
### Thread-safe implementation for multi-threading use.
A robust singleton implementation should work in any conditions. This is why we need to ensure it works when multiple threads uses it. As seen in the previous examples singletons can be used specifically in multi-threaded application to make sure the reads/writes are synchronized.

In the early implementattion the singleton object with static field is instantiated when the class is loaded and not when it is first used, due to the fact that the instance member is declared static. This is why in we don't need to synchronize any portion of the code in this case. The class is loaded once this guarantee the uniquity of the object.

### Protected constructor
It is possible to use a protected constructor to in order to permit the subclassing of the singeton. This techique has 2 drawbacks that makes singleton inheritance impractical:
- First of all, if the constructor is protected, it means that the class can be instantiated by calling the constructor from another class in the same package. A possible solution to avoid it is to create a separate package for the singleton.
- Second of all, in order to use the derived class all the getInstance calls should be changed in the existing code from Singleton.getInstance() to NewSingleton.getInstance().

### Multiple singleton instances if classes loaded by different classloaders access a singleton.

If a class(same name, same package) is loaded by 2 diferent classloaders they represent 2 different clasess in memory.

### Abstract Factory and Factory Methods implemented as singletons.

There are certain situations when the a factory should be unique. Having 2 factories might have undesired effects when objects are created. To ensure that a factory is unique it should be implemented as a singleton. By doing so we also avoid to instantiate the class before using it.

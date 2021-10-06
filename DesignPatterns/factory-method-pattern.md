# Factory Method Pattern

## Motivation
Also known as Virtual Constructor, the Factory Method is related to the idea on which libraries work: a library uses abstract classes for defining and maintaining relations between objects. One type of responsibility is creating such objects. The library knows when an object needs to be created, but not what kind of object it should create, this being specific to the application using the library. The Factory method works just the same way: it defines an interface for creating an object, but leaves the choice of its type to the subclasses, creation being deferred at run-time.

## Intent
- Defines an interface for creating objects, but let subclasses to decide which class to instantiate
- Refers to the newly created object through a common interface

## Implementation
- Product defines the interface for objects the factory method creates.
- ConcreteProduct implements the Product interface.
- Creator(also refered as Factory because it creates the Product objects) declares the method FactoryMethod, which returns a Product object. May call the generating method for creating Product objects
- ConcreteCreator overrides the generating method for creating ConcreteProduct objects

All concrete products are subclasses of the Product class, so all of them have the same basic implementation, at some extent. The Creator class specifies all standard and generic behavior of the products and when a new product is needed, it sends the creation details that are supplied by the client to the ConcreteCreator. Having this diagram in mind, it is easy for us now to produce the code related to it. Here is how the implementation of the classic 

```C++
virtual Product {}

class Creator 
{
public: 
    void anOperation() 
    {
        Product product = factoryMethod();
    }    
protected:
    Product factoryMethod();
}

class ConcreteProduct::Product {  }

class ConcreteCreator::Creator 
{
protected:
    Product factoryMethod() 
    {
        return new ConcreteProduct();
    }
}

class Client 
{
public:
    static void main( string arg[] ) 
    {
        Creator creator = new ConcreteCreator();
        creator.anOperation();
    }
}
```

## Applicability & Examples
The need for implementing the Factory Method is very frequent, when a class can't anticipate the type of the objects it is supposed to create, or when a class wants its subclasses to be the ones to specific the type of a newly created object.

### Example 1: Documents Application
Take into consideration a framework for desktop applications, working with documents. A framework for desktop applications contains definitions for operations such as opening, creating and saving a document. The basic classes are abstract ones, named Application and Document, their clients having to create subclasses from them in order to define their own applications. For generating a drawing application, for example, they need to define the DrawingApplication and DrawingDocument classes. The Application class has the task of managing the documents, taking action at the request of the client (for example, when the user selects the open or save command form the menu).

Because the Document class that needs to be instantiated is specific to the application, the Application class does not know it in advance, so it doesn't know what to instantiate, but it does know when to instantiate it. The framework needs to instantiate a certain class, but it only knows abstract classes that can't be instantiated.

The Factory Method design pattern solves the problem by putting all the information related to the class that needs to be instantiated into an object and using them outside the framework, as you can see below
The factory method is pattern which provides us a way to achieve the DIP principle. 

## Specific problems and implementation
### Definition of Creator class
If we apply the pattern to an already written code there may be problems with the way we have the Creator class already defined. There are two cases:
- Creator class is abstract and generating method does not have any implementation. In this case the ConcreteCreator classes must define their own generation method and this situation usually appears in the cases where the Creator class can't foresee what ConcreteProduct it will instantiate.
- Creator class is a concrete class, the generating method having a default implementation. If this happens, the ConcreteCreator classes will use the generating method for flexibility rather than for generation. The programmer will always be able to modify the class of the objects that the Creator class implicitly creates, redefining the generation method.

### Drawbacks and Benefits
- \+ It introduces a separation between the application and a family of classes (it introduces weak coupling instead of tight coupling hiding concrete classes from the application). It provides a simple way of extending the family of products with minor changes in application code.
- \+ It provides customization hooks. If a factory is used instead of creating the objects directly inside the class,  the customized objects can easily replace the original objects, configuring the factory to create them.
- \- The factory has to be used for a family of objects. If the classes doesn't extend common base class or interface they can not be used in a factory design template.

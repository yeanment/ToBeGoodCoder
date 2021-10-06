# Factory Pattern

## Motivation
The Factory Design Pattern is probably the most used design pattern in modern programming languages like Java and C#. If you are searching for it, most likely, you'll find references about the GoF patterns: Factory Method and Abstract Factory.

In this article we'll describe a flavor of factory pattern commonly used nowdays. You can also check the original Factory Method pattern which is very similar.

## Intent
- Creates objects without exposing the instantiation logic to the client.
- Refers to the newly created object through a common interface

## Implementation
The client needs a product, but instead of creating it directly using the new operator, it asks the factory object for a new product, providing the information about the type of object it needs. The factory instantiates a new concrete product and then returns to the client the newly created product(casted to abstract product class). The client uses the products as abstract products without being aware about their concrete implementation.

## Applicability & Examples
### Example 1: Graphical application works with shapes
The drawing framework is the client and the shapes are the products. All the shapes are derived from an abstract shape class (or interface). The `Shape` class defines the draw and move operations which must be implemented by the concrete shapes. The framework receives the shape type as a string parameter, it asks the factory to create a new shape sending the parameter received from menu. The factory creates the new shape and returns it to the framework, casted to an abstract shape. Then the framework uses the object as casted to the abstract class without being aware of the concrete object type.

The advantage is obvious: New shapes can be added without changing a single line of code in the framework (the client code that uses the shapes from the factory). 

## Specific problems and implementation
### Procedural Solution - switch/case noob instantiation.
Those are also known as parameterized Factories. The generating method can be written so that it can generate more types of Product objects, using a condition (entered as a method parameter or read from some global configuration parameters - see abstract factory pattern) to identify the type of the object that should be created as below:
```C++
public class ProductFactory{
    public Product createProduct(string ProductID){
        if (id==ID1)
            return new OneProduct();
        if (id==ID2) return
            return new AnotherProduct();
        ... // so on for the other Ids
        
        return null; //if the id doesn't have any of the expected values
    }
    ...
}
```
The problem here is that once we add a new concrete product call we should modify the Factory class. It is not very flexible and it violates open close principle. Of course we can subclass the factory class, but let's not forget that the factory class is usually used as a singleton. Subclassing it means replacing all the factory class references everywhere through the code.

### Class Registration
You can register new product classes to the factory without even changing the factory itself. For creating objects inside the factory class without knowing the object type we keep a map between the productID and the class type of the product. In this case when a new product is added to the application it has to be registered to the factory. This operation doesn't require any change in the factory class code, and can be put anywhere in our code.

Since the factory should be unaware of products we have to move the creation of objects outside of the factory to an object aware of the concrete products classes. That would be the concrete class itself. We add a new abstract method in the product abstract class. Each concrete class will implement this method to create a new object of the same type as itself. We also have to change the registration method such that we'll register concrete product objects instead of Class objects.

```C++
virtual class Product
{
public:
    Product createProduct();
    ...
}

class OneProduct::Product
{
    ...
    static
    {
        ProductFactory.instance().registerProduct("ID1", new OneProduct());
    }
public:
    OneProduct createProduct()
    {
        return new OneProduct();
    }
    ...
}

class ProductFactory
{
public:
    void registerProduct(string productID, Product p)    {
        m_RegisteredProducts.put(productID, p);
    }

    Product createProduct(string productID){
        ((Product)m_RegisteredProducts.get(productID)).createProduct();
    }
}
```
### A more advanced solution - Factory design pattern with abstractions (Factory Method)
Let's assume we need to add a new product to the application. For the procedural switch-case implementation we need to change the Factory class, while in the class registration implementation all we need is to register the class to the factory without actually modifying the factory class. For sure this is a flexible solution. The most intuitive solution to avoid modifying the Factory class is to extend it, which is the classic implementation of the factory method pattern. There are some drawbacks over the class registration implementation and not so many advantages:
- \+ The derived factory method can be changed to perform additional operations when the objects are created (maybe some initialization based on some global parameters ...).
- \- The factory can not be used as a singleton.
- \- Each factory has to be initialized before using it.
- \- More difficult to implement.
- \- If a new object has to be added a new factory has to be created.
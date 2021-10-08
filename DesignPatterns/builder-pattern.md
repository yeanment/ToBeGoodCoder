# Builder Pattern

## Motivation
Builder (or Adaptive Builder) design pattern allows a client object to construct a complex object by specifying only its type and content, being shielded from the details related to the object's representation. This way the construction process can be used to create different representations. The logic of this process is isolated form the actual steps used in creating the complex object, so the process can be used again to create a different object form the same set of simple objects as the first one.

## Intent
- Defines an instance for creating an object but letting subclasses decide which class to instantiate
- Refers to the newly created object through a common interface
Creates objects without exposing the instantiation logic to the client.

## Implementation
- The Builder class specifies an abstract interface for creating parts of a Product object.
- The ConcreteBuilder constructs and puts together parts of the product by implementing the Builder interface. It defines and keeps track of the representation it creates and provides an interface for saving the product.
- The Director class constructs the complex object using the Builder interface.
- The Product represents the complex object that is being built.

The Builder represents the complex object that needs to be built in terms of simpler objects and types. The constructor in the Director class receives a Builder object as a parameter from the Client and is responsible for calling the appropriate methods of the Builder class. In order to provide the Client with an interface for all concrete Builders, the Builder class should be an abstract one. This way you can add new types of complex objects by only defining the structure and reusing the logic for the actual construction process. The Client is the only one that needs to know about the new types, the Director needing to know which methods of the Builder to call.

## Applicability & Examples
Builder Pattern is used when:
- the creation algorithm of a complex object is independent from the parts that actually compose the object
- the system needs to allow different representations for the objects that are being built

### Example 1 - Vehicle Manufacturer.
Let us take the case of a vehicle manufacturer that, from a set of parts, can build a car, a bicycle, a motorcycle or a scooter. In this case the Builder will become the VehicleBuilder. It specifies the interface for building any of the vehicles in the list above, using the same set of parts and a different set of rules for every type of type of vehicle. The ConcreteBuilders will be the builders attached to each of the objects that are being under construction. The Product is of course the vehicle that is being constructed and the Director is the manufacturer and its shop.
### Example 2 - Students Exams.
If we have an application that can be used by the students of a University to provide them with the list of their grades for their exams, this application needs to run in different ways depending on the user that is using it, user that has to log in. This means that, for example, the admin needs to have some buttons enabled, buttons that needs to be disabled for the student, the common user. The Builder provides the interface for building form depending on the login information. The ConcreteBuilders are the specific forms for each type of user. The Product is the final form that the application will use in the given case and the Director is the application that, based on the login information, needs a specific form. 

## Specific problems and implementation
### Builder and Abstract Factory
The Builder design pattern is very similar, at some extent, to the Abstract Factory pattern. In the case of the Abstract Factory, the client uses the factory's methods to create its own objects. In the Builder's case, the Builder class is instructed on how to create the object and then it is asked for it, but the way that the class is put together is up to the Builder class, this detail making the difference between the two patterns.

In practice the products created by the concrete builders have a structure significantly different, so if there is not a reason to derive different products a common parent class. This also distinguishes the Builder pattern from the Abstract Factory pattern which creates objects derived from a common type.

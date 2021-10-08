# Prototype Pattern

## Motivation
The Prototype design pattern allows an object to create customized objects without knowing their class or any details of how to create them. Rather than creation it uses cloning, if the cost of creating a new object is large and creation is resource intensive. Up to this point it sounds a lot like the Factory Method pattern, the difference being the fact that for the Factory the palette of prototypical objects never contains more than one object.

## Intent
- specifying the kind of objects to create using a prototypical instance
- creating new objects by copying this prototype

## Implementation
- Client - creates a new object by asking a prototype to clone itself.
- Prototype - declares an interface for cloning itself.
- ConcretePrototype - implements the operation for cloning itself.

The process of cloning starts with an initialized and instantiated class. The Client asks for a new object of that type and sends the request to the Prototype class. A ConcretePrototype, depending of the type of object is needed, will handle the cloning through the Clone() method, making a new instance of itself.

## Applicability & Examples
Use Prototype Pattern when a system should be independent of how its products are created, composed, and represented, and:
- Classes to be instantiated are specified at run-time
- Avoiding the creation of a factory hierarchy is needed
- It is more convenient to copy an existing instance than to create a new one.

### Example 1
In building stages for a game that uses a maze and different visual objects that the character encounters it is needed a quick method of generating the haze map using the same objects: wall, door, passage, room... The Prototype pattern is useful in this case because instead of hard coding (using new operation) the room, door, passage and wall objects that get instantiated, CreateMaze method will be parameterized by various prototypical room, door, wall and passage objects, so the composition of the map can be easily changed by replacing the prototypical objects with different ones. The Client is the CreateMaze method and the ConcretePrototype classes will be the ones creating copies for different objects.

## Specific problems and implementation
### Using a prototype manager
When the application uses a lot of prototypes that can be created and destroyed dynamically, a registry of available prototypes should be kept. This registry is called the prototype manager and it should implement operations for managing registered prototypes like registering a prototype under a certain key, searching for a prototype with a given key, removing one from the register, etc. The clients will use the interface of the prototype manager to handle prototypes at run-time and will ask for permission before using the Clone() method. Prototype Manager is implemented usually as a hashtable keeping the object to clone. When use it, prototype become a factory method which uses cloning instead of instantiation.

### Implementing the Clone operation
A small discussion appears when talking about how deep or shallow a clone should be: a deep clone clones the instance variables in the cloning object while a shallow clone shares the instance variables between the clone and the original. Usually, a shallow clone is enough and very simple, but cloning complex prototypes should use deep clones so the clone and the original are independent, a deep clone needing its components to be the clones of the complex object’s components.

### Initializing clones
There are cases when the internal states of a clone should be initialized after it is created. This happens because these values cannot be passed to the Clone() method, that uses an interface which would be destroyed if such parameters were used. In this case the initialization should be done by using setting and resetting operations of the prototype class or by using an initializing method that takes as parameters the values at which the clone’s internal states should be set.

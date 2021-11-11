# OOP Principles

## SOLID Design Principles 

An acronym referring to a collection of design principles evolved out of object-orientated community. These principles are often discussed within the context of classical, statically typed, object-oriented languages. However, the benefits from the application of these princples can still be applied to a prototype-based, dynamically typed language (JavaScript).

- Single Responsibility Principle
- Open/Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

## Single Responsibility Principle

```
A class should have one and only one reason to change, meaning a class should only have one job/responsibility.
```

- "Do one thing and do it well"
- An objects definition should only have to be modified due to changes to a single responsibility within the system
- Otherwise, if an object is trying to have multiple responsibilities, changing one aspect might affect another (negatively). By decoupling such responsibilities, we can create code that is more resilient to change

### Loosely Coupled Objects

- Tightly coupled objects are objects that rely so heavily on each other that removing or changing one will mean that you have to completelty change another one

### Object role stereotypes

- An approach which can aid in the organization of behaviour
- A set of general, pre-established roles which commonly occur across object-oriented architectures:

- **Information holder** - object designed to know certain information and provide that information to other objects
- **Structurer** - object that mantains the relationships between objects and information about those relationships
- **Service providor** - object that performs specific work and offers services to others on demand
- **Controller** - object designed to make decisions and control a complex task
- **Coordinator** - object that doesn't make many decisions but delegates work to other objects (in a mechanical way)
- **Interfacer** - object that transforms information or requests between distinct parts of a system

## Open-Closed Principle

```
Objects (or entities) should be open to extension, but closed for modification.
```

- Open to extension means we should be able to add new features or components **without breaking** existing code
- Closed for modification means **we should not introduce breaking changes toe existing functionality**, because that would force you to refactor alot of existing code
- e.g. in the case of a class or factory function, it should be easily extendable without modifying the class or function itself

## Liskov Substitution Principle

```
Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.
```

- This states every subclass/derived class should be substitutable for their base/parent class
- So a subclass should override the parent class methods in a way that does not break functionality from a client's point of view

## Interface Segretion Principle

```
A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.
```

- Bloated inferfaces shouldn't be created. So whenever a module is exposed to outside use, make sure only the bare essentials are required and the rest are optional

## Dependency Inversion Principle

```
Entities must depend on abstractions not on concretions. It states that the high level module must not depend on the low level module, but they should depend on abstractions.
```

- This principle allows for decoupling


---
layout: post
---
# Design Patterns for Software Design

When God created this world, first he created us - you and me. Then he established relationship between us so that some became our teacher, some our siblings, some our friends etc. Finally, HE put us to work according to our relationships i.e. teacher passed knowledge to us, we processed that knowledge and passed it out friends and siblings etc.

Design patterns can be divided into three categories:

1. Creational
   - Solves problem of instantiating an object or group of related objects by providing object creation mechanisms depending on the situation
2. Structural
   - Solves problem of connections/relationships between objects
3. Behavioral
   - Solves problem of communication/message passing between objects.
     

[TOC]



## Creational Design Patterns

1. Simple Factory
2. Factory Method
3. Abstract Factory
4. Builder
5. Prototype
6. Singleton

### Simple Factory Pattern

Simple factory just hides the instantiation logic (read as usage of "new" keyword) from client. It takes in few input parameters and returns an object.

```c#
public interface Vehicle
{
    int GetWheels();
}

public class Car : Vehicle
{
    int _wheels;

    public Car(int wheels)
    {
        _wheels = wheels;
    }

    public int GetWheels() { return _wheels; }
}

public class CarFactory
{
    public static Car MakeCar(int wheels)
    {
        return new Car(wheels);
    }
}
```
### Abstract Factory Pattern

Abstract factory is just factory of factories. A factory creates objects of related types eg. Car factory creates car engines, car tyres, car body. A truck factory creates truck wheels, truck body, truck engines etc. An abstract factory lets client choose whether to create "car factory" or "truck factory".

The key is there are interrelated dependencies eg. car wheels, car engine, car body are all interrelated. 

### Factory method Pattern

This pattern delegates instantiation to child classes. You have multiple workers - each worker capable of performing different type of role. In factory pattern, a single static function creates a type of worker, so clients know which type of worker to create. In factory method, workers are encapsulated inside Manager classes. Manager class is an abstract class. Any one can extend a Manager class and choose whcih type of worker to create. 

Clients work with child classes (Derived manager class) and get the work done without knowing which child class was involved.

This is useful when client's choice of worker is dynamically decided at runtime. A wrapper class (manager) will do some generic work and also helps in creation of right worker class.

### Builder Pattern

In factory pattern, object creation is a one step process. You ask for a car and car is given to you. What if you want to customize car like adding color, choosing no. of airbags, choosing engine displacement, etc. All of these can be the parameters to constructor that creates the object but soon these parameters increase to too many. There's a term for it called "telescoping constructor anti-pattern". Telescoping constructor is just a chain of constructors each with one more parameter than previous one and previous constructor calls constructor with one extra parameter with default value for extra parameter. Builder pattern is a better alternative to telescoping constructors. Builder pattern provides a builder object which provides flag/value to set for each of the parameter and finally a build function which will create object by using default values for variables that are not set.

### Prototype Pattern

Prototype pattern helps in creating a clone of an object. It saves from the trouble of creating object from scratch by reusing all properties and new properties can be set. Usually, an object need to provide ShallowCopy and DeepCopy functions which can be used by clients to create prototype of an existing object using these functions.

### Singleton Pattern

This is used when a class can have only one object ever created. This single object is used across whole system.

## Structural Design Patterns

Structural design patterns are mostly concerned with object composition by establishing relationships between different object in order to create a component. In Object oriented system, an object is the smallest unit but an object alone does not achieve much unless it is wired with multiple objects to create a component. A component is the smallest task unit which can fulfill a task. A component could just be a button on the screen where button component is made up of clickable box object and text object and a drawing manager which is responsible for changing button color when clicked/hovered etc.

- Adapter
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Proxy

### Adapter Pattern

A class 1 implements an interface and class implementation is frozen. Now, another class 2 wants to use this class but it expects different interface, different interface because it might be working with N other classes and all those classes might be implementing a different interface, so our class 1 is odd-one-out. To fix that we write a Class1Adapter which provides the interface that class 2 looks for and internally, it uses the interface implemented by class 1 to do the work.

### Bridge Pattern

Bridge pattern prefers composition over inheritance. Instead of deep hierarchies, it prefers to keep separate hierarchies. Consider a wheel class. To create wheel for bicycle, car, truck, we can have three derived classes or we can create a different Vehicle class which knows how to create wheel based on vehicle type. Wheel class "has-a" vehicle class object passed to it and invoke buildWheel inside it. Now wheel class does not know how to build a wheel, so in future if new vehicle comes, it will bring its implementation of wheel and for the rest of the world, it can work with wheel class to create wheels. 

This composition of object is called establishing bridge between the two objects and hence, the name Bridge pattern.

### Composite Pattern

Composite pattern helps treat individual objects as well as composite objects (i.e. objects made up of multiple individual objects) in the same uniform way.

### Decorator Pattern

Decorator pattern allows to add functionality to an instance of class without impacting other instances. For example, coffee class has 10 instances. In one instance, customer also added extra milk or chocolate powder. Decorator patterns let us define "milk" & "chocolate powder" as separate classes which accepts instance of coffee class and override the method in it. Eg. GetCost function returns cost for normal coffee. Milk class will override GetCost function and add 2 to it. So, the original way to obtain cost for normal coffee remains same while Milk only adds functionality i.e. extra 2 to the price. Similarly, ChocolatePowder class will derive from Coffee class and override GetCost and add extra 5 to it. 

Clients can just call MilkCoffee class to get price of it. 

### Facade Pattern

It provides simple interface to a complex subsystem. If there are large no. of steps to be performed to complete an operation, we can put those steps inside another class and let the class provide a simple interface like DoOperation(). Clients do not need to know the details of complex operations happening beneath the facade layer.

### Flyweight Pattern

From wikipedia,

> A flyweight is an object that minimizes memory use by sharing as much data as possible with other similar objects; it is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory. 

### Proxy Pattern

In proxy pattern, a class represents the functionality of another class. From wikipedia,

>  A proxy, in its most general form, is a class functioning as an interface to something else. A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked. 



## Behavioral Design Patterns

These patterns define common communication/message-passing/data-passing patterns between objects.

- Chain of Responsibility
- Command
- Iterator
- Mediator
- Memento
- Observer
- Visitor
- Strategy
- State
- Template Method

### Chain of Responsibility

You have a list of command objects and a list of processing objects. Each processing object contains logic to handle a specific type of command object. In this pattern, each command object enters a *chain* of processing object and traverses the chain until a processing object is found that can handle the command object. One example of chain of responsbility is Outlook Rules where multiple rules are specified and are internally kept in a chain of responsibility. Any incoming message is passed through the chain until the message is handled and rule will either pass it down the chain or *stop processing more rules* if specified.

Chain is set by setting a next pointer inside each processing object from outside. 

### Command Pattern

There are two real world examples. You go to a restaurant to order food. Customer object issues command to Waiter object to bring food and waiter object "encapsulates all the information needed to perform an action or trigger an event at a later time". The waiter object encapsulates all the information including the method name (CookFood), the object (Chef) that owns the method and values for the method parameters (Dishes). Similarly, cashier prepares the bill but waiter brings us the bill and change. Here, we have decoupled Customer from Chef and cashier by making Waiter encapsulate all the information required to achieve the prepare-dish and prepare-bill tasks.

Second example is that of transaction based system where multiple commands are issued one after another and a transaction is said to succeed only when all commands succeed. If some command fails in between, then all commands before that in the transaction are rolled back. Each command implements Do() and Undo() method. A transaction object maintains history of commands executed and executes undo if any command fails in the lifetime of transaction object.

### Iterator Pattern

This pattern allows to iterate/traverse/enumerate objects inside a container without knowing the data structure used by container in actually storing those objects internally. 

### Mediator Pattern

It provides a "middle-layer" object that is used by objects on either side of middle-layer to communicate with each other i.e. each object send message to the mediator object and mediator object forwards the message either originally or after some modification to the object on other side.

Mediators are common in marriage talks.

### Memento Pattern

A memento is an object that captures the current state of an object. It can be used to restore the object to same state. It does not break the encapsulation of the object because only the object can create a memento object which is snapshot of its current state and if the object receive that memento object in future, it can use it to restore its state. 

### Observer Pattern

An object maintains a list of subscribers, also called as dependents or observers, and notified them automatically of any state change by calling one of their methods.

The class that maintains list implements "Observable" interface and class that wants to be notified implements "Observer" interface.

### Visitor Pattern

Visitor pattern provides the ability to add new operations to existing objects without modifying the objects. It is one way of achieving open/closed principle.

 In essence, the visitor allows adding new *virtual* functions to a family of classes, without modifying the classes. Instead, a visitor class is created that implements all of the appropriate specializations of the virtual function. The visitor takes the instance reference as input, client calls the method in existing object passing it instance of visitor object which is dynamic decided at runtime. Existing object invokes the virtual method of visitor. So, multiple polymorphism takes place - first an existing object is dynamically chosen and then a dynamic operation is applied on it.

### Strategy pattern

Strategy pattern allows you to dynamically switch strategy i.e. logic/algorithm based upon the runtime situation. For eg. for sorting operation, if input is small, use bubble sort else use quick sort.

### State pattern

It lets you change the behavior of an object when its state changes. State is kept inside the instance and current state is used to accomplish the task an object is trying to achieve.

This can be used to implement state machine.

### Template method

Define an abstract class with abtsract methods for each step of multi-step process and a *final* template method that invokes all methods in an order. Each new class can extend the abstract class and define custom implementation for each step of the process. The template method can then be invoked which remains consistent. Wikipedia says,

> In software engineering, the template method pattern is a behavioral design pattern that defines the program skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets one redefine certain steps of an algorithm without changing the algorithm's structure. 




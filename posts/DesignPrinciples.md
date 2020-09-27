Whenever you move to design stage of software project, you need to design high level and low level diagrams of the system. In order to determine the components in that system, it's almost necessary that you have knowledge of these principles so as to create a system that is "maintainable", "scalable" and "extensible".  You should write code in a way that follows these principles. 

[TOC]

## What are SOLID Principles of Software Design

1. S - SRP - Single Responsibility Principle
2. O - OCP - Open/Closed Principle
3. L - Liskov Substitution Principle
4. I - Interface Segregation Principle
5. D - Depedency Inversion Principle

### SRP - Single Responsibility Principle

 It states that each class should be designed for single responsibility. Single responsibility means that there should not be more than one reason to chnage the class.

### OCP - Open Closed Principle

It states that each class should provide functionality in such a way that to support a new use-case/scenario, the existing code need not be changed but the class can be extended by means of new functionality. For extending an additional classes, usually we try to avoid putting concrete code inside the class and instead pass interface. Interface can be extended anytime in future to add new functionality. Also, we do not want a complex business logic inside the class so that if anyone does not want to use that business logic, then he need not make change in the class. This way class is closed for modification by providing only limited fixed functionality and abstracting out the parts that can be changed in future. "Virtual" methods can be overriden to extend the functionality.

### LSP - Liskov Substitution Principle

Simply, it states that derived class should have "is-a" relationship with base class i.e. derived class should be able to completely replace base class. If derived class does not support any of the functionalities of base class, then it violates LSP and the supported and non-supported functionalities should be separated through different interface. Each class should implement the interface that it supports. At runtime, we can enumerate all particular types implementing an interface via reflection.

### ISP - Interface Segregation Principle

Have you even seen an interface with lot of methods? Are all those methods very closely inter-related? If not, then someday someone implementing that interface may not want some of the methods. THis principle suggests to segregate unrelated methods in an interface into multiple small and specific interfaces.

### IOC - Inversion of Control or Dependency Inversion Principle

UI depends on database. When any button is pressed in UI, data is fetched from database. All high level modules will depend on low level modules and this can make it hard to change low level modules. This can be avoided by making database implement IDatabase interface. Now database implementation can change over time without any change in interface and UI only works with interface. So, it does not depend on database. In fact, interface methods are what UI needs and database by implementing that interface now fulfills needs of UI. Earlier, UI was using what database provides and now database provides what UI needs, so dependency inversion has been achieved. That's what Dependency Inversion principle says "Both high level and low component should depend on abstractions and not on implementation". Also, we should "Program to an interface, not to an implementation". The moment we do a "obj = new Class()", we are working with an implementation. If we have lot of such "new" across multiple classes, we have a very tightly coupled system . We should try to keep the initialization and usage separate and try to work with abstractions. Interfaces provide very good abstraction of functionality without worrying about the provider of functionality.

## BONUS 1: Other Popular Design Principles

### Hollywood Principle

This is similar to dependency inversion principle and goal is to avoid complex dependency graph between different components. The aim is to have single path (think single linked list) dependency between components. The curious name for this principle is inspired from regular statement of hollywood producers (high level component) to struggling artists (low level components) - "Don't call us, we will call you".

### DRY - Don't Repeat Yourself

This principle states that there should be single representation of any logic or data in a system. Duplication of data or logic or process leads to poor maintainability and extensibility.

### YAGNI - You Ain't Gonna Need It

This principle states that you should implement something that is needed today and not spend much time on things that do not have any short term impact but might be needed in future. Implement what you need when you need it.

### KISS - Keep It Simple, Stupid OR Keep It Short and Simple OR Keep It Simple and Straightforward

This principle states that simplicity should be a goal of system design and complexity should be avoided. This is also a life principle and applicable in every area of our life. This is important principle in aspect of maintainability of software. If code is complex to understand for an average competent programmer, it's going to be hard to maintain it. Similarly, if a product UI is complex to understand for a first-time, new user, it violates KISS principle and should be modified. Please carefully note that the suggestion here is to not reduce the features of product. A product must do all the things that it is expected to do but the complexity which does not help user do the tasks should be reworked. There's a popular quote related to this, 

> "Perfection is achieved not when there's nothing left to add but when there's nothing left to throw away"

### Composite Reuse principle (Favor composition over inheritance)

I do not really see composition and inheritance as competitors but see them as mutually exclusive. For inheritance, we need to ensure there's a "is-a" relationship between base class and derived class. For composition, we need to ensure there's a "has-a" relationship between two real world objects we're trying to model. Eg. Car "is a" type of vehicle. Car "has a" engine or wheel or seat or steering. This principle says that favoring composition has an advantage because each component can provide specific functionality and can change independent of rest of the components whereas in case of inheritance, we need to find common functionalities and create a tree like structure which is more rigid and impact extensibility.

## BONUS 2 : GRASP Design Principles

There's a group of software design principles which are called General Responsibility Assignment Software Patterns (or Prinicples), abbreviated as GRASP. These are

1. High Cohesion
2. Low coupling
3. Polymorphism
4. Information Expert
5. Creator
6. Controller
7. Indirection
8. Pure Fabrication

### High Cohesion & Low Coupling

Both these principles are related to SRP. 

Low cohesion means class is fulfilling many different responsibilities, eg. it might be fetching data, then processing that data according to some logic and then saving data. Each of these three responsibilities can be split into three different classes. **High Cohesion** means a class is fulfilling only a single responsbility. **Low coupling** means dependency between classes should be minimum. One good way to estimmate coupling in your project is to search for *new* keyword and see how many different files turn up.

### Polymorphism

Polymorphism is basic OOP principle which allows class hierarchy, overriding functionalities of base class in derived class, function polymotphism which allows different parameters with same function name and ability to define interface which can be implemented by many unrelated classes. This design principle nudges designers to keep those in mind while designing system.

### Information Expert

This principle suggest to put logic where data is i.e. responsibility should be assigned to the class which is an "expert" for that functionality i.e. has the best information to fulfill that responsibility. This reduces coupling between classes.

### Creator

Creator principle suggest that a class B should create instance of class A only if at least one or preferably more of the following conditions are met:

1. B contains A 
2. B "has-a" relationship (aggregation) with A
3. B records A i.e. B monitors all activities done by instances of A 
4. B closely uses instances of A i.e. A's usage is mostly inside B class and A need not be used outside of class B
5. B has initialization data for A

### Controller

Controller is an object that acts as a layer between two components. It does not do much work on its own but uses different objects to get the work done. It represents a use-case. A use-case can have many different tasks and each of the tasks can be done by different object. Controller helps in routing the input and getting output back.

### Indirection

The goal of indirection is to keep coupling low between objects by introducing a new object that acts as "mediating layer" between two complex components. . A new object known as "indirection" is used which Controller in MVC pattern is an example of this indirection.

### Pure Fabrication

In a system design, there are some classes which model entities in the domain. Eg. in a library system design, there are books, users, authors, shelfs etc. A pure fabrication class does not represent any concept. Eg. a class named UserNotification will notify user when a book is available or InformationProvider class which will provide information to user about library timings but it does not represent any domain concept. It's a pure fabrication class.
---
layout: post
title: Dependency Injection
date: 2020-01-02 00:58:49 +0000
---

# Dependency Injection

Dependency injection is a design pattern that helps achieve Dependency inversion SOLID design principle. To recap, Dependency inversion principle states that all high level and low level components should depend on abstractions and not on implementation. Also, Dependency inversion principle states that we should program to an interface and not to an implementation. As a result of this advice, our code depends on lot of different interface.

Dependency injection is the mechanism through which interfaces are connected to implementation. Dependency injection provides mechanism to create implementation outside the classes that depend on those implementations and provide a way to pass those implementations to dependent high-level classes. 

### Container

A fundamental concept to DI is container. Container is a blackbox that holds all the implementations. Whenever you need an implementation/instance for an interface, you get it from the container.

It is upto container to give you a new instance everytime or give the same instance everytime. In case of latter, we get a singleton object without actually making the class implement singleton design pattern. That's the beauty of container. It manages the lifetime of objects as configured. In singleton design pattern, a class is responsible for managing lifetime as well functionality which breaks Single Responsibility Principle. That's why it is sometimes recommended to go with DI to control lifetime instead of singleton pattern because DI gives you flexibility to share single instance in whole application or create new objects per-thread, per-webrequest etc and this decision can be easily changed in future if needed unlike singleton class which need change in class behavior to manage lifetime.


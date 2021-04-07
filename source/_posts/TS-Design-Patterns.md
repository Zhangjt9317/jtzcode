---
title: TS Design Patterns
date: 2021-03-29 15:36:57
tags:
---

Several design patterns and TS implementaion will be shown here:

[reference](https://refactoring.guru/design-patterns/typescript)

more here:
* https://github.com/torokmark/design_patterns_in_typescript

Based on the refactoring Guru, lets do the following first:

* Creational Patterns
* Structural Patterns
* Behavioral Patterns

## Creational Patterns

* Abstract Factory
* Builder
* Factory Method
* Prototype
* Singleton


## Structural Patterns

* Adaptor
* Bridge
* Composite
* Decorator
* Facade
* Flyweight
* Proxy


## Behavioral Patterns

* Chain of Responsibility
* Command
* Iterator
* Mediator
* Memento
* Observer
* State
* Strategy
* Template Method
* Visitor


### Creational Patterns

Abstract Factory is a creational pattern, which solves the problem of creating entire product families without sepecifying their concrete classes. 

```Typescript

/**
 * The Abstract Factory interface declares a set of methods that return
 * different abstract products. These products are called a family and are
 * related by a high-level theme or concept. Products of one family are usually
 * able to collaborate among themselves. A family of products may have several
 * variants, but the products of one variant are incompatible with products of
 * another.
 */
interface AbstractFactory {
    createProductA(): AbstractProductA;

    createProductB(): AbstractProductB;
}


class ConcreteFactory1 implements AbstractFactory {
    public createProductA(): AbstractProductA {
        return new ConcreteProductA1();
    }

    public createProductB(): AbstractProductB {
        return new ConcreteProductB1();
    }
}

```

### Structural Pattern


### Builder


### Bridge



### Factory Method 



### Composite


### Prototype


### Decorator


### Singleton


### Facade



### Flyweight

### Proxy



### Adapter Pattern

Convert the interface of class into another interface clients expect. 
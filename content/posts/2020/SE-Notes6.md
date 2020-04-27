---
title: "软件工程复习笔记[6] - Design And Implementation"
date: 2020-04-23T19:09:46+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

Software **design and implementation** activities are invariably **inter-leaved**.  

# An Object-oriented Design Process
+ Object-oriented design processes involve **developing a number of different system models**.
+ They require a lot of effort for development and maintenance of these models and, for **small systems**, this may not be cost-effective.
+ However, for **large systems** developed by different groups **design models** are an **important communication mechanism**.

### Common activities
+ Define the context and modes of use of the system;
+ Design the system architecture;
+ Identify the principal system objects;
+ Develop design models;
+ Specify object interfaces.

## Context and interaction models
 + A **system context model** is a structural model that demonstrates the other systems in the environment of the system being developed.
 + An **interaction model** is a dynamic model that shows how the system interacts with its environment as it is used.

## Architecture Design
Once **interactions** between the system and its environment have been understood, you use this information for **designing the system architecture**.      
You identify the **major components** that make up the system and their interactions, and then may organize the components using an **architectural pattern** such as a layered or client-server model.     

## Object Class Identification
It relies on the **skill, experience and domain knowledge of system designers**.        
Object identification is an **iterative process**. You are unlikely to get it right first time.

### Approaches to identification
+ Use a **grammatical approach** based on a natural language description of the system (used in Hood OOD method).
+ Base the **identification on tangible things** in the application domain.
+ Use a **behavioural approach** and identify objects based on what participates in what behaviour.
+ Use a **scenario-based analysis**. The objects, attributes and methods in each scenario are identified.

## Design Models
Design models show **the objects and object classes and relationships between these entities**.     
**Static models** describe the static structure of the system in terms of object classes and relationships.     
**Dynamic models** describe the dynamic interactions between objects.       

### Examples of design models
+ **Subsystem models** (**packages** in the UML) that show logical groupings of objects into coherent subsystems.
+ **Sequence models** that show the sequence of object interactions.
+ **State machine models** that show how individual objects change their state in response to events. (You **don’t usually need a state diagram for all of the objects** in the system. Many of the objects in a system are relatively simple and a state model adds unnecessary detail to the design.)
+ Other models include use-case models, aggregation models, generalisation models, etc.

## Interface Specification
+ The UML uses **class diagrams** for interface specification but **Java** may also be used.

# Design Patterns
### What is a Pattern?
+ “Each pattern is a three-part rule, which expresses a **relation** between a certain context, a **problem** and a **solution**.”       
+ The common definition of a pattern: **“A solution to a problem in a context.”**     

A design pattern is a way of **reusing abstract knowledge about a problem and its solution**.       
A pattern is **a description of the problem and the essence of its solution**.      
Pattern descriptions usually make use of object-oriented characteristics such as **inheritance and polymorphism**.      

### GoF Essential Elements of Design Patterns
+ Pattern Name
+ Problem
+ Solution
+ Consequences

# Implementation Issues
+ **Reuse.** Most modern software is constructed by reusing existing components or systems. When you are developing software, you should make as much use as possible of existing code.
+ **Configuration management.** During the development process, you have to keep track of the many different versions of each software component in a configuration management system.
+ **Host-target development.** Production software does not usually execute on the same computer as the software development environment. Rather, you develop it on one computer (the host system) and execute it on a separate computer (the target system). 

## Reuse Levels
### The abstraction level
+ At this level, you don’t reuse software directly but use knowledge of successful abstractions in the design of your software. 

### The object level 
+ At this level, you directly reuse objects from a library rather than writing the code yourself. 

### The component level
+ Components are collections of objects and object classes that you reuse in application systems. 

### The system level 
+ At this level, you reuse entire application systems. 

## Configuration Management
+ The general process of **managing a changing software system**.         
+ **Version management**, where support is provided to keep track of the different versions of software components.
+ **System integration**, where support is provided to help developers define what versions of components are used to create each version of a system. 
+ **Problem tracking**, where support is provided to allow users to report bugs and other problems, and to allow all developers to see who is working on these problems and when they are fixed. 

## Host-target Development
Most software is developed on one computer (the host), but runs on a separate machine (the target).         
**An IDE** is a set of software tools that supports different aspects of software development, within some common framework and user interface.     

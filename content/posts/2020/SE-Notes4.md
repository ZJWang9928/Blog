---
title: "软件工程复习笔记[4] - System Modeling"
date: 2020-04-23T09:53:53+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---
System modeling is the process of **developing abstract models of a system**, with each model **presenting a different view or perspective** of that system.    
System modeling helps the analyst to **understand the functionality of the system** and models are used to **communicate with customers**.     

# Unified Modeling Language (UML)
The UML is the standard language for **visualizing, specifying, constructing, and documenting** the artifacts of a software-intensive system.   
It can be used with **all processes**, throughout the development life cycle, and across different implementation technologies.     

### The UML may be used to:
+ Display the boundary of a system & its major functions using **use cases and actors**
+ Illustrate use case realizations with **interaction diagrams**
+ Represent a static structure of a system using **class diagrams**
+ Model the behavior of objects with **state transition diagrams**
+ Reveal the physical implementation architecture with **component & deployment diagrams**
+ Extend your functionality with **stereotypes**

### The elements of UML
+ Things
+ Relationships
+ Diagrams
+ Rules

## Things
### Structural thing: Object classes
+ Classes in different abstraction

![Pic](/images/2020/04/se-notes4/Picture1.png)

### Structural thing: Interfaces
+ The services set of a class or a component
+ Description of a external behavior of a class or a component 

![Pic](/images/2020/04/se-notes4/Picture2.png)
![Pic](/images/2020/04/se-notes4/Picture3.png)

### Structural thing: Components
+ Physical parts in system

![Pic](/images/2020/04/se-notes4/Picture4.png)

### Behavioral thing: Interaction
+ Communication between objects

![Pic](/images/2020/04/se-notes4/Picture5.png)

### Grouping thing: Package
+ Organize some things into a group

![Pic](/images/2020/04/se-notes4/Picture6.png)

### Comment thing: Comment

![Pic](/images/2020/04/se-notes4/Picture7.png)

## UML Relationships
### Types:
+ Association
+ Generalization
+ Dependency
+ Realization

### Association

![Pic](/images/2020/04/se-notes4/Picture8.png)

### Aggregation (not must) and Composition (must)
+ Special association

![Pic](/images/2020/04/se-notes4/Picture9.png)

### Generalization
+ A relationship to show **general and special** (**a kind of**)

![Pic](/images/2020/04/se-notes4/Picture10.png)

### Dependency
+ Reflect the relationship that of **one thing will be affected by another**.

![Pic](/images/2020/04/se-notes4/Picture11.png)

### Realization
+ A part is **realized by another**

![Pic](/images/2020/04/se-notes4/Picture12.png)

## UML Diagrams
#### Functional model
+ Use case diagram 
#### Structural model
+ Class diagram
+ Object diagram
+ Component diagram
+ Deployment diagram 
+ Sequence diagram
#### Behavioral model
+ Collaboration diagram
+ State diagram
+ Activity diagram 

### Existing and planned system models
+ **Models of the existing system** are used during requirements engineering. They help clarify what the existing system does and can be used as a basis for discussing its strengths and weaknesses. These then lead to requirements for the new system.
+ **Models of the new system** are used during requirements engineering to help explain the proposed requirements to other system stakeholders. Engineers use these models to discuss design proposals and to document the system for implementation. 
+ In a **model-driven engineering process**, it is possible to generate a complete or partial system implementation from the system model. 

### Use of graphical models
+ As **a means of facilitating discussion** about an existing or proposed system
+ As a way of **documenting** an existing system
+ As a **detailed system description** that can be used to generate a system implementation

### Use of UML diagrams
+ **Activity diagrams** (Show the activities involved in a process or in data processing)
+ **Use case diagrams** (Show the interactions between a system and its environments)
+ **Sequence diagrams** (Show interactions between system components)
+ **Class diagrams** (Show static relationships between classes)
+ **State diagrams** (Show how the system reacts to internal & external events)

### Context models
+ Context models are used to illustrate the **operational context** of a system - they show what lies **outside the system boundaries**.
+ **Architectural models** show the system and its relationship with other systems.
+ **System boundaries** are established to define **what is inside and what is outside the system**.

![Pic](/images/2020/04/se-notes4/Picture13.png)

### Process models
+ Context models **simply show the other systems** in the environment, not how the system being developed is used in that environment.
+ Process models reveal **how the system being developed** is used in broader business processes.
+ **UML activity diagrams** may be used to define business process models.

![Pic](/images/2020/04/se-notes4/Picture14.png)

### Interaction models
+ **Modeling system-to-system interaction** highlights the communication problems that may arise.
+ **Modeling component interaction** helps us understand if a proposed system structure is likely to deliver the required system performance and dependability.
+ **Use case diagrams and sequence diagrams** may be used for interaction modeling.

#### Use case modeling
+ **Use cases** were developed originally to support **requirements elicitation** and now incorporated into the UML.
+ **Each use case represents a discrete task** that involves external interaction with a system.
+ Actors in a use case may be **people or other systems**.
+ Represented diagramatically to provide an overview of the use case and in a **more detailed textual form** (**use case description**).

![Pic](/images/2020/04/se-notes4/Picture15.png)

#### Sequence diagrams
+ Sequence diagrams are part of the UML and are used to **model the interactions between the actors and the objects within a system**.
+ A sequence diagram shows the **sequence of interactions** that take place during a particular use case or use case instance.
+ The objects and actors involved are listed along the **top of the diagram**, with a **dotted line** drawn vertically from these.
+ Interactions between objects are indicated by **annotated arrows**.  
+ **Vertical triangle** denotes **life span**.

![Pic](/images/2020/04/se-notes4/Picture16.png)
![Pic](/images/2020/04/se-notes4/Picture17.png)

### Structural models
+ Structural models of software display the **organization of a system** in terms of the components that make up that system and their relationships.
+ Structural models may be **static models**, which show the structure of the system design, or **dynamic models**, which show the organization of the system when it is executing. 

#### Class diagrams
+ Class diagrams are used when developing an object-oriented system model to show the **classes in a system and the associations between these classes**. 
+ An **association** is a **link between classes** that indicates that there is some relationship between these classes. 
+ When you are developing models **during the early stages** of the software engineering process, **objects represent something in the real world**, such as a patient, a prescription, doctor, etc. 

### Behavioral models
Behavioral models are models of the **dynamic behavior** of a system as it is executing. They show **what happens or what is supposed to happen** when a system responds to a **stimulus** from its environment.          
You can think of these **stimuli** as being of two types:       
+ **Data**. Some data arrives that has to be processed by the system.
+ **Events**. Some event happens that triggers system processing. Events may have associated data, although this is not always the case.

#### Data-driven modeling
+ **Many business systems** are data-processing systems that are primarily driven by data.
+ Data-driven models show the sequence of actions involved in **processing input data and generating an associated output**. 
+ They are **particularly useful during the analysis of requirements** as they can be used to show end-to-end processing in a system.

![Pic](/images/2020/04/se-notes4/Picture20.png)

#### Event-driven modeling
+ **Real-time systems** are often event-driven, with minimal data processing. For example, a landline phone switching system responds to events such as ‘receiver off hook’ by generating a dial tone. 
+ Event-driven modeling shows how a system responds to **external and internal events**. 
+ It is based on the assumption that **a system has a finite number of states and that events (stimuli) may cause a transition from one state to another**. 
+ **Statecharts** are an integral part of the UML and are used to represent **state machine models**.

# Model Driven Architecture (MDA)
MDA is a model-focused approach to software design and implementation that uses **a subset of UML models** to describe a system.        
**Models at different levels of abstraction are created.** From a high-level, platform independent model, it is possible, in principle, to generate a working program without manual intervention.      

## Types of Model
### A computation independent model (CIM)
+ These model the important domain abstractions used in a system. CIMs are sometimes called **domain models**.

### A platform independent model (PIM)
+ These model the operation of the system **without reference to its implementation**. The PIM is usually described using UML models that show the static system structure and how it responds to external and internal events.

### Platform specific models (PSM)
+ These are transformations of the platform-independent model with **a separate PSM for each application platform**. In principle, there may be layers of PSM, with each layer adding some platform-specific detail.  

#### MDA transformations
![Pic](/images/2020/04/se-notes4/Picture21.png)

## Agile Methods And MDA
If **transformations can be completely automated and a complete program generated from a PIM**, then, in principle, MDA could be used in an agile development process as no separate coding would be required.      

## Executable UML
+ The **fundamental notion** behind model-driven engineering is that **completely automated transformation of models to code** should be possible.
+ This is possible using **a subset of UML 2**, called **Executable UML** or **xUML**.
+ **To create an executable subset of UML**, the number of model types has therefore been dramatically reduced to these 3 key types: **Domain models**, **Class models** and **State models**.
+ The dynamic behavior of the system may be specified declaratively using the **object constraint language (OCL)**, or may be expressed using **UML’s action language**.

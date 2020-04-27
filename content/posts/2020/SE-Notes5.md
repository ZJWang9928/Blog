---
title: "软件工程复习笔记[5] - Architectural Design"
date: 2020-04-23T13:47:30+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

The design process for **identifying the sub-systems making up a system and the framework for sub-system control and communication** is **architectural design**, which is an **early stage** of the system design process.         
The output of this design process is **a description of the software architecture**.        
It represents the **link between specification and design** processes and is often carried out **in parallel with some specification activities**.        
Each architectural model only shows **one view or perspective** of the system. For both **design and documentation**, you usually need to present **multiple views** of the software architecture.      
Architectural design decisions include decisions on the **type of application**, the **distribution of the system**, the **architectural styles** to be used.       

# 4 + 1 View Model of Software Architecture
+ **A logical view**, which shows the key abstractions in the system as objects or object classes. 
+ **A process view**, which shows how, at run-time, the system is composed of interacting processes. 
+ **A development view**, which shows how the software is decomposed for development.
+ **A physical view**, which shows the system hardware and how software components are distributed across the processors in the system.
+ **Related using use cases or scenarios (+1)**

![Pic](/images/2020/04/se-notes5/Picture1.png)

### Logic View
![Pic](/images/2020/04/se-notes5/Picture2.png)
### Development View
![Pic](/images/2020/04/se-notes5/Picture3.png)
### Process View
![Pic](/images/2020/04/se-notes5/Picture4.png)
### Physical View
![Pic](/images/2020/04/se-notes5/Picture5.png)

# Architectural Patterns
Patterns are a means of representing, sharing and reusing knowledge.    
An architectural pattern is a **stylized description of good design practice**, which has been tried and tested in different environments.      

## The Model-View-Controller (MVC) Pattern 

| Name          | MVC                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description   | Separates presentation and interaction from the system data. The system is structured into three logical components that interact with each other. The Model component manages the system data and associated operations on that data. The View component defines and manages how the data is presented to the user. The Controller component manages user interaction (e.g., key presses, mouse clicks, etc.) and passes these interactions to the View and the Model. |
| When used     | Used when there are multiple ways to view and interact with data. Also used when the future requirements for interaction and presentation of data are unknown.                                                                                                                                                                                                                                                                                                          |
| Advantages    | Allows the data to change independently of its representation and vice versa. Supports presentation of the same data in different ways with changes made in one representation shown in all of them.                                                                                                                                                                                                                                                                    |
| Disadvantages | Can involve additional code and code complexity when the data model and interactions are simple.                                                                                                                                                                                                                                                                                                                                                                        |
### The organization of the Model-View-Controller
![Pic](/images/2020/04/se-notes5/Picture6.png)

### Web application architecture using the MVC pattern
![Pic](/images/2020/04/se-notes5/Picture7.png)

## Layered Architecture
+ Used to model the **interfacing of sub-systems**.
+ **Organises the system into a set of layers** (or abstract machines) each of which provide a set of services.
+ Supports the **incremental development of sub-systems** in different layers. When a layer interface changes, only the adjacent layer is affected.

| Name          | Layered Architecture                                                                                                                                                                                                                                                                                                                          |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description   | Organizes the system into layers with related functionality associated with each layer. A layer provides services to the layer above it so the lowest-level layers represent core services that are likely to be used throughout the system.                                                                                                  |
| When used     | Used when building new facilities on top of existing systems; when the development is spread across several teams with each team responsibility for a layer of functionality; when there is a requirement for multi-level security.                                                                                                           |
| Advantages    | Allows replacement of entire layers so long as the interface is maintained. Redundant facilities (e.g., authentication) can be provided in each layer to increase the dependability of the system.                                                                                                                                            |
| Disadvantages | In practice, providing a clean separation between layers is often difficult and a high-level layer may have to interact directly with lower-level layers rather than through the layer immediately below it. Performance can be a problem because of multiple levels of interpretation of a service request as it is processed at each layer. |

![Pic](/images/2020/04/se-notes5/Picture8.png)

## Repository Architecture
When **large amounts of data** are to be shared, the repository model of sharing is most commonly used a this is an efficient data sharing mechanism.       

| Name          | Repository                                                                                                                                                                                                                                                                    |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description   | All data in a system is managed in a **central repository** that is accessible to all system components. Components do not interact directly, only through the repository.                                                                                                    |
| When used     | You should use this pattern when you have a system in which **large volumes of information** are generated that has to be stored for a long time. You may also use it in **data-driven systems** where the inclusion of data in the repository triggers an action or tool.    |
| Advantages    | **Components can be independent**—they do not need to know of the existence of other components. Changes made by one component can be propagated to all components. All data can be managed **consistently** (e.g., backups done at the same time) as it is all in one place. |
| Disadvantages | The repository is a single point of failure so **problems in the repository affect the whole system**. May be inefficiencies in organizing all communication through the repository. Distributing the repository across several computers may be difficult.                   |

### A repository architecture for an IDE
![Pic](/images/2020/04/se-notes5/Picture9.png)

## Client-server Architecture
| Name          | Client-server                                                                                                                                                                                                                                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description   | In a client–server architecture, the functionality of the system is organized into services, with each service delivered from a separate server. Clients are users of these services and access servers to make use of them.                                                   |
| When used     | Used when data in a shared database has to be **accessed from a range of locations**. Because servers can be replicated, may also be used when **the load on a system is variable**.                                                                                           |
| Advantages    | The principal advantage of this model is that **servers can be distributed across a network**. General functionality (e.g., a printing service) can be available to all clients and does not need to be implemented by all services.                                           |
| Disadvantages | Each service is a single point of failure so susceptible to denial of service attacks or server failure. Performance may be unpredictable because it depends on the network as well as the system. May be management problems if servers are owned by different organizations. |

## Pipe And Filter Architecture
+ **Functional transformations** process their inputs to produce outputs.
+ May be referred to as a **pipe and filter model** (as in **UNIX shell**).
+ Commonly used in **data processing applications** (both batch- and transaction-based) where inpputs are processed in separate stages to generate related outputs.

## Application Architecture
### Examples of application types
+ Data processing applications
+ Transaction processing applications
+ Event processing systems
+ Language processing systems

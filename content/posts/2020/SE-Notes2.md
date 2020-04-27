---
title: "软件工程复习笔记[2] - Agile Software Development"
date: 2020-04-22T17:18:47+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---


# Agile Methods
## Features
+ **Focus on the code** rather than the design.
+ Are **based on an iterative approach** to software development.
+ Are intended to **deliver working software quickly and evolve this quickly** to meet changing requirements.

The **aim of agile methods** is to **reduce overheads in the software process** (e.g. by limiting documentation) and to be able to **respond quickly to changing requirements without excessive rework**.       

## The principles of agile methods
| Principle            | Description                                                                                                                                                                         |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Customer involvement | Customers should be closely involved throughout the development process. Their role is provide and prioritize new system requirements and to evaluate the iterations of the system. |
| Incremental delivery | The software is developed in increments with the customer specifying the requirements to be included in each increment.                                                             |
| People not process   | The skills of the development team should be recognized and exploited. Team members should be left to develop their own ways of working without prescriptive processes.             |
| Embrace change       | Expect the system requirements to change and so design the system to accommodate these changes.                                                                                     |
| Maintain simplicity  | Focus on simplicity in both the software being developed and in the development process. Wherever possible, actively work to eliminate complexity from the system.                  |

## Agile method applicability
+ **small or medium-sized product**
+ Custom system development within an organization, where there is a clear commitment from the **customer to become involved in the development process** and where there are not a lot of external rules and regulations that affect the software.
+ Because of their focus on small, tightly-integrated teams, there are **problems in scaling agile methods to large systems**. 

## Two key issues
+ Are systems that are developed using an agile approach **maintainable**, given the emphasis in the development process of **minimizing formal documentation**?
+ Can agile methods be used effectively for **evolving a system in response to customer change requests**?

Problems may arise if **original development team cannot be maintained**.       

## Plan-driven and agile development
### Plan-driven development
+ A plan-driven approach to software engineering is **based around separate development stages** with the outputs to be produced at each of these stages planned in advance.      
+ Not necessarily waterfall model – plan-driven, **incremental development is possible (delivered as a whole).**   
+ Iteration occurs within activities.   

### Agile development
+ **Specification, design, implementation and testing are inter-leaved and the outputs** from the development process are decided through a process of negotiation during the software development process.       
+ Agile methods rely on **good tools** to keep track of an evolving design.       

# Extreme Programming (XP)
Perhaps the best-known and most widely used agile method.       

Extreme Programming (XP) takes an **‘extreme’ approach** to iterative development:      

+ New versions may be built several times per day;
+ Increments are delivered to customers every 2 weeks;
+ All tests must be run for every build and the build is only accepted if tests run successfull.

User requirements are expressed as **scenarios** or **user stories**, which are later broke into **implementation tasks**.       

A particular strength of extreme programming is the development of **automated tests before a program feature is created**. All tests must successfully execute when an increment is integrated into a system.      

## XP practices

| Principle or practive  | Description                                                                                                                                                                                                                                                                                                                                                                                                                           |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Incremental planning   | Requirements are recorded on story cards and the stories to be included in a release are determined by the time available and their relative priority. The developers break these stories into development ‘Tasks’.                                                                                                                                                                                                                   |
| Small releases         | The minimal useful set of functionality that provides business value is developed first. Releases of the system are frequent and incrementally add functionality to the first release.                                                                                                                                                                                                                                                |
| Simple design          | Enough design is carried out to meet the current requirements and no more.                                                                                                                                                                                                                                                                                                                                                            |
| Test-first development | An **automated unit test framework** is used to write tests for a new piece of functionality before that functionality itself is implemented.                                                                                                                                                                                                                                                                                           |
| Refactoring            | All developers are expected to refactor the code continuously as soon as possible code improvements are found. This keeps the code simple and maintainable. This **improves the understandability** of the software and so reduces the need for documentation. **Changes are easier to make** because the code is well-structured and clear. However, some changes requires architecture refactoring and this is much more expensive. |
| Pair programming       | Developers work in pairs, checking each other’s work and providing the support to always do a good job.                                                                                                                                                                                                                                                                                                                               |
| Collective ownership   | The pairs of developers work on all areas of the system, so that no islands of expertise develop and all the developers take responsibility for all of the code. Anyone can change anything.                                                                                                                                                                                                                                          |
| Continuous integration | As soon as the work on a task is complete, it is integrated into the whole system. After any such integration, all the unit tests in the system must pass.                                                                                                                                                                                                                                                                            |
| Sustainable pace       | Large amounts of overtime are not considered acceptable as the net effect is often to reduce code quality and medium term productivity.                                                                                                                                                                                                                                                                                               |
| On-site customer       | A representative of the end-user of the system (the customer) should be available full time for the use of the XP team. In an extreme programming process, the customer is a member of the development team and is responsible for bringing system requirements to the team for implementation.                                                                                                                                       |

# Scrum
The Scrum approach is a **general agile method** but its focus is on **managing iterative development** rather than specific agile practices.       

## Three phases in Scrum:
+ The **initial phase** is an outline planning phase where you establish the general objectives for the project and design the software architecture.
+ This is followed by a series of **sprint cycles** (**Assess, Select, Review, Develop**), where each cycle develops **an increment** of the system. 
+ The **project closure phase** wraps up the project, completes required documentation such as system help frames and user manuals and assesses the lessons learned from the project.

## The Sprint cycle
**Sprints are fixed length, normally 2–4 weeks.** They correspond to **the development of a release** of the system in XP.      

The starting point for planning is the product backlog, which is the list of work to be done on the project.        

The **selection phase** involves all of the project team who work with the customer to select the features and functionality to be developed during the sprint.         

Once these are agreed, the team organize themselves to **develop** the software. **During this stage the team is isolated from the customer and the organization, with all communications channelled through the so-called ‘Scrum master’.**        

The role of the Scrum master is to **protect the development team from external distractions**.         

At the end of the sprint, the work done is **reviewed and presented to stakeholders**. The next sprint cycle then begins.

## Scrum benefits
+ The product is **broken down into a set of manageable and understandable chunks**.
+ Unstable requirements do not hold up progress.
+ The whole team have visibility of everything and consequently team communication is improved.
+ Customers see on-time delivery of increments and gain feedback on how the product works.
+ Trust between customers and developers is established and a positive culture is created in which everyone expects the project to succeed.

# Scaling Agile Methods
Agile methods have proved to be successful for small and medium sized projects that can be developed by a small co-located team. It is sometimes argued that the success of these methods comes because of improved communications which is possible when everyone is working together.     

**Scaling up agile methods** involves changing these to cope with larger, longer projects where there are multiple development teams, perhaps working in different locations.       

## Scaling out and scaling up
+ **‘Scaling up’** is concerned with using agile methods for developing large software systems that cannot be developed by a small team.
+ **‘Scaling out’** is concerned with how agile methods can be introduced across a large organization with many years of software development experience.

When scaling agile methods, it is essential to maintain **agile fundamentals**:     
+ Flexible planning
+ frequent system releases
+ continuous integration
+ test-driven development
+ good team communications.          

For large systems development, it is not possible to focus only on the code of the system. You need to do more **up-front design and system documentation**. **Cross-team communication mechanisms have to be designed and used.**      

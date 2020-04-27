---
title: "软件工程复习笔记[3] - Requirements Engineering"
date: 2020-04-22T17:24:10+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

# Types Of Requirement
+ **User requirements**: Statements in **natural language plus diagrams** of the services the system provides and its operational constraints. Written for customers.
+ **System requirements**: **A structured document setting out detailed descriptions** of the system’s functions, services and operational constraints. Defines what should be implemented so may be part of a contract between client and contractor.

# Functional And Non-functional Requirements
### Functional requirements
+ Statements of services the system should provide, how the system should **react to particular inputs** and how the system should **behave in particular situations**.
+ May state what the system should not do.

### Non-functional requirements
+ Constraints on the services or functions offered by the system such as timing constraints, constraints on the development process, standards, etc.
+ Often apply to the system **as a whole** rather than individual features or services.
+ Non-functional requirements may be **more critical than functional requirements**. If these are not met, the system may be useless.
+ Non-functional requirements may **affect the overall architecture** of a system rather than the individual components.
+ A single non-functional requirement, such as a security requirement, may **generate a number of related functional requirements** that define system services that are required.
+ Non-functional requirements may be very **difficult to state precisely** and imprecise requirements may be **difficult to verify**. 

### Domain requirements
+ Constraints on the system from the domain of operation.

## Requirements completeness and consistency
### Complete
+ They should include descriptions of all facilities required.

### Consistent
+ There should be no conflicts or contradictions in the descriptions of the system facilities.

In principle, requirements should be both complete and consistent.      
In practice, it is impossible to produce a complete and consistent requirements document.   

# Requirements Engineering Processes
### Generic activities
+ Requirements elicitation;
+ Requirements analysis;
+ Requirements validation;
+ Requirements management.

In practice, RE is an **iterative activity** in which these processes are interleaved.      

### Stages of Requirements elicitation and analysis
+ Requirements discovery (Interaction is with system stakeholders from managers to external regulators.);
+ Requirements classification and organization;
+ Requirements prioritization and negotiation;
+ Requirements specification.

## Interviewing
Formal or informal interviews with stakeholders are part of most RE processes.      

### Types of interview
+ **Closed interviews** based on pre-determined list of questions;
+ **Open interviews** where various issues are explored with stakeholders.

Normally **a mix of closed and open-ended interviewing**.   
Interviews are **good for getting an overall understanding** of what stakeholders do and how they might interact with the system.       
Interviews are **not good for understanding domain requirements**.

## Ethnography
**To spends a considerable time observing and analysing how people actually work**.     
Ethnography is effective for **understanding existing processes** but **cannot identify new features** that should be added to a system.        
Ethnography can be combined with prototyping. **Prototype development** results in unanswered questions which focus the ethnographic analysis.        
The **problem with ethnography** is that it studies existing practices which may have some historical basis which is no longer relevant.

## Stories and Scenarios
Scenarios are real-life examples of how a system can be used. They should include:    
+ A description of the starting situation;
+ A description of the normal flow of events;
+ A description of what can go wrong;
+ Information about other concurrent activities;
+ A description of the state when the scenario finishes.

# Requirements Specification
The process of **writing down the user and system requirements in a requirements document**.        

### Ways of writing a system requirements specification

| Notation                     | Dexcription                                                                                                                                                                                                                                                                                                                                               |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Natural language             | The requirements are written using numbered sentences in natural language. Each sentence should express one requirement.                                                                                                                                                                                                                                  |
| Structured natural language  | The requirements are written in natural language on a standard form or template. Each field provides information about an aspect of the requirement. (Form-based specifications; Tabular specification, particularly useful when you have to **define a number of possible alternative courses of action**.)                                                                                                                                                                                                     |
| Design description languages | This approach uses a language like a programming language, but with more abstract features to specify the requirements by defining an operational model of the system. This approach is now **rarely used** although it can be useful for interface specifications.                                                                                       |
| Graphical notations          | Graphical models, supplemented by text annotations, are used to define the functional requirements for the system; **UML use case** and **sequence diagrams** are commonly used.                                                                                                                                                                            |
| Mathematical specifications  | These notations are based on mathematical concepts such as finite-state machines or sets. Although these unambiguous specifications can reduce the ambiguity in a requirements document, most customers don’t understand a formal specification. They cannot check that it represents what they want and are reluctant to accept it as a system contract. |

In practice, requirements and design are inseparable.       

## Use cases
+ **High-level graphical model** supplemented by more detailed tabular description.       
+ Use-cases are **a scenario based technique** in the UML which identify the actors in an interaction and which describe the interaction itself.
+ A set of use cases should **describe all possible interactions** with the system.
+ **Sequence diagrams may be used to add detail** to use-cases by showing the sequence of event processing in the system.

## The software requirements document
It is **NOT** a design document. As far as possible, it should set of **WHAT** the system should do rather than **HOW** it should do it.    

# Requirement Validation
Concerned with **demonstrating that the requirements define the system that the customer really wants**.            
Requirements error costs are high so validation is very important.      

### Requirements checking
+ **Validity**
+ **Consistency**
+ **Completeness**
+ **Realism**
+ **Verifiability**

### Requirements validation techniques
+ **Requirements reviews**. Systematic manual analysis of the requirements.
+ **Prototyping**. Using an executable model of the system to check requirements.
+ **Test-case generation**. Developing tests for requirements to check testability.

# Requirements Management
Requirements management is the process of **managing changing requirements** during the requirements engineering process and system development.       

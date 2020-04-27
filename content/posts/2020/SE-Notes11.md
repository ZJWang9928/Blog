---
title: "软件工程复习笔记[11] - Quality Management"
date: 2020-04-26T16:19:25+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

Concerned with ensuring that the required level of quality is achieved in a software product.   
Three **principal concerns**:       
+ At the **organizational level**, quality management is concerned with establishing a framework of organizational processes and standards that will lead to high-quality software. 
+ At the **project level**, quality management involves the application of specific quality processes and checking that these planned processes have been followed. 
+ At the **project level**, quality management is also concerned with establishing a **quality plan** for a project. The quality plan should set out the quality goals for the project and define what processes and standards are to be used. 

## Quality Management Activities
Quality management provides an **independent check** on the software development process.       
The quality management process checks the project deliverables to ensure that they are **consistent with organizational standards and goals**.      
The quality team should be **independent from the development team** so that they can take an objective view of the software. This allows them to report on software quality without being influenced by software development issues.       

### Quality management and software development
![Pic](/images/2020/04/se-notes11/Picture1.png)

## Quality Plans
### Quality plan structure
+ Product introduction;
+ Product plans;
+ Process descriptions;
+ Quality goals;
+ Risks and risk management.

Quality plans should be **short, succinct documents**.  

## Software Standards
Standards define the **required attributes** of a product or process.       
Standards may be **international, national, organizational or project standards**.          
Product standards define characteristics that all software components should exhibit e.g. a common programming style.       
Process standards define how the software process should be enacted.        

## ISO 9001 Standards Framework
An international set of standards that can be used as a basis for developing quality management systems.        
The ISO 9001 standard is a framework for developing software standards.     
 It sets out general quality principles, describes quality processes in general and lays out the organizational standards and procedures that should be defined. These should be documented in an organizational quality manual.        

## Quality Reviews
A group of people carefully examine part or all of a software system and its associated documentation.      
Code, designs, specifications, test plans, standards, etc. can all be reviewed.     
Software or documents may be 'signed off' at a review which signifies that progress to the next development stage has been approved by management.      

### The software review process
![Pic](/images/2020/04/se-notes11/Picture2.png)

## Program Inspections
These are peer reviews where engineers **examine the source of a system** with the aim of discovering anomalies and defects.    
Inspections **do not require execution** of a system so may be used before implementation.  

## Software Measurement And Metrics
**Software measurement** is concerned with deriving a **numeric value** for an attribute of a software product or process.      
This allows for **objective comparisons** between techniques and processes.     

### Relationships between internal and external software
![Pic](/images/2020/04/se-notes11/Picture3.png)

### Product metrics
A quality metric should be a **predictor of product quality**.      
Classes of product metric       
+ **Dynamic metrics** which are collected by measurements made of a program in execution;
+ **Dynamic metrics** help assess **efficiency and reliability**;
+ **Dynamic metrics** are **closely related** to software quality attributes;
+ **Static metrics** which are collected by measurements made of the system representations;
+ **Static metrics** help assess **complexity, understandability and maintainability**;
+ **Static metrics** have an **indirect relationship** with quality attributes.

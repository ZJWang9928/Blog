---
title: "软件工程复习笔记[7] - Software Testing"
date: 2020-04-24T13:43:03+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

# Program Testing Goals
+ To **demonstrate** to the developer and the customer that the software meets its requirements. (leads to **validation testing**)
+ To **discover** situations in which the behavior of the software is incorrect, undesirable or does not conform to its specification. (leads to **defect testing**)
+ Testing can only show the presence of errors in a program. It **cannot demonstrate that there are no remaining faults**.

# Verification vs Validation
## Verification
+ **"Are we building the product right”.**
+ The software should conform to its specification.

## Validation
+ **"Are we building the right product”.**
+ The software should do what the user really requires.

# Inspections And Testing
## Software inspections
+ Concerned with analysis of the static system representation to discover problems  (**static verification**)     
+ May be supplement by tool-based document and code analysis.     
+ These involve **people examining the source representation** with the aim of discovering anomalies and defects.
+ Inspections **not require execution** of a system so may be **used before implementation**.
+ They have been shown to be an effective technique for discovering program errors.

## Software testing 
+ Concerned with exercising and observing product behaviour (**dynamic verification**)        
+ The system is executed with **test data** and its operational behaviour is observed.        

![Pic](/images/2020/04/se-notes7/Picture1.png)

+ Inspections and testing are **complementary** and not opposing verification techniques.
+ **Both** should be used during the V & V process.
+ Inspections can check conformance with a specification but not conformance with the **customer’s real requirements**.
+ Inspections cannot check **non-functional characteristics** such as performance, usability, etc.

### A model of the software testing process 
![Pic](/images/2020/04/se-notes7/Picture2.png)

# Test Data And Test Cases
## Test data
Inputs which have been devised to test the system.      

## Test case
Inputs to test the system and the **predicted outputs** from these inputs if the system operates according to its specification.            

# Choosing Test Case
## Black-box testing
+ An approach to testing where the program is considered as a ‘black-box’.
+ The program test cases are based on the **system specification**.

## White-box testing
+ Derivation of test cases according to **program structure**. 
+ **Knowledge of the program** is used to identify additional test cases.

# Stages of Testing
## Development testing
Development testing includes all testing activities that are carried out by the team developing the system.         
+ **Unit testing**, where individual program units or object classes are tested. Unit testing should focus on **testing the functionality of objects or methods**. (Whenever possible, unit testing should **be automated** so that tests are run and checked without manual intervention.) (Automated test components: a **setup part**, a **call part**, an **assertion part**)
+ **Component testing**, where several individual units are integrated to create composite components. Component testing should focus on **testing component interfaces**.
+ **System testing**, where some or all of the components in a system are integrated and the system is tested as a whole. System testing should focus on **testing component interactions**.

## Release testing
A separate testing team test a complete version of the system before it is released to users.         

## User testing
Users or potential users of a system test the system in their own environment.      

# Testing Strategies (Unit Testing)
## Partition testing (Black-box)
+ Where you identify groups of inputs that have **common characteristics** and should be processed in the same way.     
+ Input data and output results often fall into different classes where all members of a class are **related**.
+ Each of these classes is an **equivalence partition** or domain where the program behaves in an equivalent way for each class member.
+ Test cases should be **chosen from each partition**.

## Guideline-based testing
Where you use testing guidelines to choose test cases.      

### General testing guidelines
+ Choose inputs that force the system to generate all error messages.
+ Design inputs that cause input buffers to overflow.
+ Repeat the same input or series of inputs numerous times.
+ Force invalid outputs to be generated.
+ Force computation results to be too large or too small.

## Structural testing (White-box)
+ Derivation of test cases **according to program structure**. Knowledge of the program is used to identify additional test cases.
+ Objective is to **exercise all program statements** (not all path combinations)

![Pic](/images/2020/04/se-notes7/Picture3.png)

## Path testing
The objective of path testing is to ensure that the set of test cases is such that **each path through the program is executed at least once**.     
The starting point for path testing is a **program flow graph** that shows nodes representing program decisions and arcs representing the flow of control.      
Statements with conditions are therefore nodes in the flow graph.       

### Program flow graphs
+ **Describes the program control flow.** Each branch is shown as a separate path and loops are shown by arrows looping back to the loop condition node.
+ Used as a basis for computing the **cyclomatic complexity**.
+ **Cyclomatic complexity = Number of edges - Number of nodes +2**.

### Cyclomatic complexity
+ **The number of tests** to test all control statements equals the cyclomatic complexity.
+ Cyclomatic complexity equals **number of conditions in a program**.
+ Useful if used with care. **Does not imply adequacy of testing**.
+ Although all paths are executed, **all combinations of paths** are not executed.

# Component Testing
Software components are often composite components that are **made up of several interacting objects**.         
You access the functionality of these objects through the defined component interface. Testing composite components should therefore focus on showing that the **component interface** behaves according to its specification.      
You can assume that unit tests on the individual objects within the component have been completed.          

## Interface testing
![Pic](/images/2020/04/se-notes7/Picture4.png)
Objectives are to **detect faults due to interface errors or invalid assumptions about interfaces**.        

### Interface types
+ **Parameter interfaces.** Data passed from one method or procedure to another.
+ **Shared memory interfaces.** Block of memory is shared between procedures or functions.
+ **Procedural interfaces.** Sub-system encapsulates a set of procedures to be called by other sub-systems.
+ **Message passing interfaces.** Sub-systems request services from other sub-systems.

### Interface errors
+ **Interface misuse**
+ **Interface misunderstanding**
+ **Timing errors** (The called and the calling component operate at different speeds and out-of-date information is accessed.)

### Interface testing guidelines
+ Design tests so that parameters to a called procedure are at the **extreme ends of their ranges**.
+ Always test pointer parameters with **null pointers**.
+ Design tests which **cause the component to fail**.
+ Use **stress testing** in message passing systems.
+ + In shared memory systems,**vary the order** in which components are activated.

# System Testing
System testing during development involves **integrating components to create a version of the system** and then **testing the integrated system**.     
The focus in system testing is **testing the interactions between components**.         
System testing checks that components are **compatible**, **interact correctly** and **transfer the right data at the right time** across their interfaces.     
System testing tests the **emergent behaviour** of a system.        

## Use-case testing
+ The use-cases developed to **identify system interactions** can be used as **a basis for system testing**.      
+ Each use case usually involves several system components so **testing the use case forces these interactions to occur**.
+ **The sequence diagrams associated with the use case** documents the components and interactions that are being tested.

# Test-driven Development
Test-driven development (TDD) is an approach to program development in which you **inter-leave testing and code development**.      
**Tests are written before code** and ‘passing’ the tests is the critical driver of development.    
TDD was introduced as part of **agile methods** such as Extreme Programming. However, it can also be used in **plan-driven development** processes.         

![Pic](/images/2020/04/se-notes7/Picture5.png)

## Benefits of test-driven development
### Code coverage 
+ Every code segment that you write has at least one associated test so **all code written has at least one test**.
### Regression testing 
+ A regression test suite is developed incrementally as a program is developed. 
+ Regression testing is testing the system to **check that changes have not ‘broken’ previously working code**.
+ In a manual testing process, regression testing is expensive but, with **automated testing**, it is simple and straightforward. **All tests are rerun every time a change is made to the program**.
### Simplified debugging 
+ When a test fails, it should be obvious where the problem lies. The newly written code needs to be checked and modified. 
### System documentation 
+ The tests themselves are a form of documentation that describe what the code should be doing. 

# Release Testing
The primary goal of the release testing process is to **convince the supplier of the system that it is good enough for use**.       
Release testing is usually a **black-box testing process** where tests are only derived from the **system specification**.      

## Release testing and system testing
Release testing is **a form of system testing**.        

### Important differences:
+ A separate team that has not been involved in the system development, should be responsible for release testing.
+ **System testing** by the development team should focus on **discovering bugs in the system** (**defect testing**). The objective of **release testing** is to **check that the system meets its requirements and is good enough for external use** (**validation testing**).

## Requirements based testing
+ Requirements-based testing involves examining each requirement and developing a test or tests for it.

## Scenario testing
+ An approach to release testing whereby you devise **typical scenarios of use** and use these scenarios to develop test cases.
+ A scenario is a story that describes one way in which the system might be used.

## Performance testing
+ Performance tests usually involve planning a series of tests where **the load is steadily increased** until the system performance becomes unacceptable.
+ **Stress testing is a form of performance testing** where the system is deliberately overloaded to test its failure behaviour.

# User Testing
User or customer testing is a stage in the testing process in which **users or customers** provide input and advice on system testing.      

## Types of User Testing
### Alpha testing
+ Users of the software **work with the development team** to test the software at the developer’s site.
### Beta testing
+ **A release of the software** is made available to users to allow them to experiment and to raise problems that they discover with the system developers.
### Acceptance testing
+ Customers test a system to decide whether or not it is ready to be accepted from the system developers and deployed in the customer environment. Primarily for custom systems.

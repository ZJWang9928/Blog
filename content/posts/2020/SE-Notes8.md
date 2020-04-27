---
title: "软件工程复习笔记[8] - Software Evolution"
date: 2020-04-24T19:30:54+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

A key problem for all organizations is **implementing and managing change to their existing software systems**.     

### The software evolution process
![Pic](/images/2020/04/se-notes8/Picture1.png)

### Change implementation
![Pic](/images/2020/04/se-notes8/Picture2.png)

# Program Evolution Dynamics
+ Program evolution dynamics is the study of the **processes of system change**.
+ After several major empirical studies, **Lehman and Belady proposed that there were a number of ‘laws’** which applied to all systems as they evolved.
+ There are sensible observations rather than laws. They are applicable to large systems developed by large organisations. 

## Lehman’s Laws 

| Law                         | Description                                                                                                                                                                                  |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Continuing change           | A program that is used in a real-world environment must necessarily change, or else become progressively less useful in that environment.                                                    |
| Increasing complexity       | As an evolving program changes, its structure tends to become more complex. Extra resources must be devoted to preserving and simplifying the structure.                                     |
| Large program evolution     | Program evolution is a self-regulating process. System attributes such as size, time between releases, and the number of reported errors is approximately invariant for each system release. |
| Organizational stability    | Over a program’s lifetime, its rate of development is approximately constant and independent of the resources devoted to system development.                                                 |
| Conservation of familiarity | Over the lifetime of a system, the incremental change in each release is approximately constant.                                                                                             |
| Continuing growth           | The functionality offered by systems has to continually increase to maintain user satisfaction.                                                                                              |
| Declining quality           | The quality of systems will decline unless they are modified to reflect changes in their operational environment.                                                                            |
| Feedback system             | Evolution processes incorporate multiagent, multiloop feedback systems and you have to treat them as feedback systems to achieve significant product improvement.                            |

### Applicability of Lehman’s laws
Lehman’s laws seem to be generally applicable to large, tailored systems developed by **large organisations**.      
It is **not clear** how they should be modified for     
+ Shrink-wrapped software products;
+ Systems that incorporate a significant number of COTS components;
+ Small organisations;
+ Medium sized systems.

# Software Maintenance
**Modifying a program after it has been put into use**.     
The term is mostly used for changing custom software. **Generic software products are said to evolve to create new versions**.      
Maintenance **does not normally involve major changes to the system’s architecture**.        

## Types of Maintenance
+ Maintenance to repair software faults
+ Maintenance to adapt software to a different operating environment
+ Maintenance to add to or modify the system’s functionality

## Maintenance Costs
+ Usually greater than development costs (**2x to 100x depending on the application**).
+ Affected by both technical and non-technical factors.
+ Increases as software is maintained. Maintenance corrupts the software structure so makes further maintenance more difficult.
+ Ageing software can have high support costs.

## Maintenance Cost Factors
+ Team stability
+ Contractual responsibility
+ Staff skills
+ Program age and structure

## Complexity Metrics
Predictions of maintainability can be made by assessing the **complexity of system components**.        
Studies have shown that most maintenance effort is spent on **a relatively small number of system components**.       
### Complexity depends on       
+ Complexity of control structures;
+ Complexity of data structures;
+ Object, method (procedure) and module size.

# System Re-engineering
**Re-structuring or re-writing part or all** of a legacy system without changing its functionality.     

### The reengineering process
![Pic](/images/2020/04/se-notes8/Picture3.png)

# Preventative Maintenance by Refactoring
+ Refactoring is the process of making improvements to a program to **slow down degradation** through change.
+ You can think of refactoring as **‘preventative maintenance’** that reduces the problems of future change.
+ Refactoring involves modifying a program to improve its structure, reduce its complexity or make it easier to understand.
+ When you refactor a program, you should not add functionality but rather **concentrate on program improvement**.

## ‘Bad Smells’ In Program Code
+ Duplicate code
+ Long methods
+ Switch (case) statements
+ Data clumping
+ Speculative generality (This occurs when developers include generality in a program in case it is required in the future. This can often simply be removed.)

# Legacy System Management
Organisations that rely on legacy systems must choose a strategy for evolving these systems.       
The strategy chosen should depend on the **system quality** and its **business value**.         

## Legacy System Categories
### Low quality, low business value
+ These systems should be scrapped. 

### Low-quality, high-business value
+ These make an important business contribution but are expensive to maintain. Should be re-engineered or replaced if a suitable system is available.

### High-quality, low-business value
+ Replace with COTS, scrap completely or maintain.

### High-quality, high business value
+ Continue in operation using normal system maintenance.

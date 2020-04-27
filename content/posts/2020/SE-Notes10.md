---
title: "软件工程复习笔记[10] - Project Planning"
date: 2020-04-26T13:08:51+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

Project planning involves **breaking down the work into parts and assign these to project team members**, anticipate problems that might arise and prepare tentative solutions to those problems.      
The aim of planning at this stage is to provide information that will be used in **setting a price** for the system to customers.       

### Plan sections
+ Introduction	
+ Project organization
+ Risk analysis
+ Hardware and software resource requirements
+ Work breakdown 
+ Project schedule
+ Monitoring and reporting mechanisms 

### The project planning process
![Pic](/images/2020/04/se-notes10/Picture1.png)

# Project Scheduling
Project scheduling is the process of **deciding how the work in a project will be organized** as separate tasks, and when and how these tasks will be executed.     
You **estimate the calendar time** needed to complete each task, the effort required and who will work on the tasks that have been identified.      
You also have to **estimate the resources** needed to complete each task, such as the disk space required on a server, the time required on specialized hardware, such as a simulator, and what the travel budget will be.       

## Milestones And Deliverables
+ **Milestones** are points in the schedule against which you can assess progress, for example, the handover of the system for testing.
+ **Deliverables** are work products that are delivered to the customer, e.g. a requirements document for the system.

## Schedule Representation
+ **Graphical notations** are normally used to illustrate the project schedule.
+ **Bar charts** are the most commonly used representation for project schedules. They show the schedule as activities or resources against time.

### Activity bar chart
![Pic](/images/2020/04/se-notes10/Picture2.png)

# Software Pricing
Estimates are made to discover the cost, to the developer, of producing a software system.      
There is not a simple relationship between the development cost and the price charged to the customer.      

## Estimation Techniques
Organizations need to make **software effort and cost estimates**. There are two types of technique that can be used to do this:        
+ **Experience-based techniques.** The estimate of future effort requirements is based on the manager’s experience of past projects and the application domain. Essentially, the manager makes an informed judgment of what the effort requirements are likely to be.
+ **Algorithmic cost modeling.** In this approach, a formulaic approach is used to compute the project effort based on estimates of product attributes, such as size, and process characteristics, such as experience of staff involved.

## Algorithmic Cost Modelling
Cost is estimated as a mathematical function of product, project and process attributes whose values are estimated by project managers:     
$$Effort = A \times \ Size^{B} \times M.$$      
**A** is an **organisation-dependent constant**, **B** reflects the **disproportionate effort** for large projects and **M** is a **multiplier reflecting product, process and people attributes**.         
The most commonly used product attribute for cost estimation is **code size**.      
**Most models are similar but they use different values for A, B and M**.       

### Estimation accuracy
The size of a software system can only be known accurately **when it is finished**.     
Several factors influence the final size        
+ Use of COTS and components;
+ Programming language;
+ Distribution of system.
As **the development process progresses** then the size estimate becomes more accurate.     
The estimates of the factors contributing to B and M are subjective and vary according to the **judgment of the estimator**.        

## The COCOMO 2 Model
+ An empirical model based on project experience.
+ Well-documented, ‘independent’ model which is not tied to a specific software vendor.
+ Long history from initial version published in 1981 (COCOMO-81) through various instantiations to COCOMO 2.
+ COCOMO 2 takes into account different approaches to software development, reuse, etc.
+ COCOMO 2 incorporates **a range of sub-models** that produce increasingly detailed software estimates.

### The sub-models in COCOMO 2
+ **Application composition model**. Used when software is composed from existing parts.
+ **Early design model**. Used when requirements are available but design has not yet started.
+ **Reuse model**. Used to compute the effort of integrating reusable components.
+ **Post-architecture model**. Used once the system architecture has been designed and more information about the system is available.

### COCOMO estimation models
![Pic](/images/2020/04/se-notes10/Picture3.png)

### Application composition model
Supports prototyping projects and projects where there is extensive reuse.      
Based on standard estimates of **developer productivity in application (object) points/month**.         
**Formula** is $$PM = (NAP \times (1 - reuse/100)) / PORD.$$       
**PM** is the **effort in person-months**, **NAP** is the **number of application points** and **PROD** is the **productivity**.    

### Early design model
Estimates can be made after the **requirements have been agreed**.  
Based on **a standard formula** for algorithmic models      
$$PM = A \times Size^{B} \times M,$$
where $$M = PERS \times RCPX \times RUSE \times PDIF \times PREX \times FCIL \times SCED.$$     
A = 2.94 in initial calibration, Size in KLOC, B varies from 1.1 to 1.24 depending on novelty of the project, development flexibility, risk management approaches and the process maturity.     

#### Multipliers
Multipliers reflect the capability of the developers, the non-functional requirements, the familiarity with the development platform, etc.      
+ RCPX - product reliability and complexity;
+ RUSE - the reuse required;
+ PDIF - platform difficulty;
+ PREX - personnel experience;
+ PERS - personnel capability;
+ SCED - required schedule;
+ FCIL - the team support facilities.

### The reuse model
Takes into account **black-box code** that is reused without change and code that has to be adapted to integrate it with new code.      
There are two versions:     
+ **Black-box reuse.** where code is not modified. An effort estimate (PM) is computed.
+ **White-box reuse.** where code is modified. A size estimate equivalent to the number of lines of new source code is computed. This then adjusts the size estimate for new code.
The formula used to calculate the **source code equivalence**   
$$ESLOC = ASLOC \times (1 - AT / 100) \times AAM.$$ 
**ESLOC** is the **equivalence number of lines of new code**. **ASLOC** is the **number of lines of generated code**. **AT** is the **percentage of code automatically generated**. **AAM** is the **adaptation adjustment multiplier** computed from the costs of changing the reused code, the costs of understanding how to integrate the code and the costs of reuse decision making.       
Apply the standard formula to calculate **total effort**    
$$Effort = A \times ESLOC^{B} \times M.$$

### Post-architecture model
Uses the same formula as the early design model but with **17 rather than 7 associated multipliers**.   
The **code size** is estimated as:      
+ Number of lines of new code to be developed;
+ Estimate of equivalent number of lines of new code computed using the reuse model;
+ An estimate of the number of lines of code that have to be modified according to requirements changes.

The **exponent term** depends on **5 scale factors**. Their **sum/100** is **added to 1.01**.       

| Scale factor                 | Explanation                                                                                                                                                                                                        |
|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Precedentedness              | Reflects the previous experience of the organization with this type of project. Very low means no previous experience; extra-high means that the organization is completely familiar with this application domain. |
| Development flexibility      | Reflects the degree of flexibility in the development process. Very low means a prescribed process is used; extra-high means that the client sets only general goals.                                              |
| Architecture/risk resolution | Reflects the extent of risk analysis carried out. Very low means little analysis; extra-high means a complete and thorough risk analysis.                                                                          |
| Team cohesion                | Reflects how well the development team knows each other and work together. Very low means very difficult interactions; extra-high means an integrated and effective team with no communication problems.           |
| Process maturity             | Reflects the process maturity of the organization. The computation of this value depends on the CMM Maturity Questionnaire, but an estimate can be achieved by subtracting the CMM process maturity level from 5.  |

## Project Duration And Staffing
As well as effort estimation, managers must estimate the **calendar time** required to complete a project and when staff will be required.      
Calendar time can be estimated using a **COCOMO 2 formula**     
$$TDEV = 3 \times (PM)^{0.33 + 0.2 \times (B - 1.01)}.$$
PM is the effort computation and B is the exponent computed as discussed above (B is 1 for the early prototyping model). This computation predicts the nominal schedule for the project.        
**The time required is independent of the number of people working on the project.**        

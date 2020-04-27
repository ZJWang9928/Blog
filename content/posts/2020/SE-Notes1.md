---
title: "软件工程复习笔记[1] - Software Process"
date: 2020-04-22T14:31:05+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

# Software Process
+ **Software processes** are the activities involved in producing a software system.
+ **General Software process models**: The waterfall model, Incremental development, Reuse-oriented software engineering.
+ **Waterfall model phases**: Requirements analysis and definition, System and software design, Implementation and unit testing, Integration and system testing, Operation and maintenance.
+ The **main drawback of the waterfall model**: the difficulty of accommodating change after the process is underway. (In principle, a phase has to be complete before moving onto the next phase.)
+ The **waterfall model** is mostly used for **large systems**. (stable requirements, only appropriate when the requirements are well-understood and changes will be fairly limited during the design process)
+ **Incremental development benefits**: 1. The **cost of accommodating changing customer requirements** is reduced; 2. It is **easier to get customer feedback** on the development work that has been done; 3. **More rapid delivery and deployment** of useful software to the customer is possible.
+ **Incremental development problems**: 1. The **process is not visible** (lack of documents); 2. **System structure tends to degrade** as new increments are added.
+ Process stages of **reuse-oriented software engineering**: Requirements specification; Component analysis; Requirements modification; System design with reuse; Development and integration; System validation.
+ **Software specification** is the process of establishing what **services are required** and **the constraints** on the system’s operation and development.
+ **Requirements engineering process**: Feasibility study (Feasibility report); Requirements elicitation and analysis (System model); Requirements specification (User and system requirements); Requirements validation (Requirements document).
+ Requirement specification (**what to do**) / Software design (**how to do**)
+ **Design activities**: Architectural design; Interface design; Component design; Database design.
+ **Software validation**: **Verification and validation (V & V)** is intended to show that a system conforms to its specification and meets the requirements of the system customer, which involves **checking and review** processes and **system testing** (most commonly used).
+ **Testing stages**: Development or component testing (**Individual components** are tested independently); System testing (Testing of the system **as a whole**); Acceptance testing (Testing with customer data to check that the system **meets the customer’s needs**).
+ **Software evolution**: Define system requirements; Assess existing systems; Propose system changes; Modify systems.
+ **Reducing the costs of rework**: change **avoidance** (where the software process includes activities that can **anticipate possible changes** before significant rework is required; **prototype system**) and change **tolerance** (where the process is designed so that changes can be **accommodated at relatively low cost**; **incremental development**).
+ **A prototype** is an initial version of a system used to demonstrate concepts and try out design options.
+ A prototype can be used in: **The requirements engineering process** to help with requirements elicitation and validation; **In design processes** to explore options and develop a UI design; **In the testing process** to run back-to-back tests.
+ Prototype development **may involve leaving out functionality**.
+ **Incremental development and delivery \\(\downarrow\\)**

| Incremental development                                                                                                  | Incremental delivery                                                                                                 |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Develop the system in increments and evaluate each increment before proceeding to the development of the next increment; | Deploy an increment for use by end-users;                                                                            |
| Normal approach used in agile methods;                                                                                   | More realistic evaluation about practical use of software;                                                           |
| Evaluation done by user/customer proxy.                                                                                  | Difficult to implement for replacement systems as increments have less functionality than the system being replaced. |
| Deliver after finishing.                                                                                                 | Deliver every increment.                                                                                             |

+ **Incremental delivery pros and cons \\(\downarrow\\)**

| Pros                                                                                                  | Cons                                                                                                          |
|-------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Customer value can be delivered with each increment so **system functionality is available earlier**. | Most systems **require a set of basic facilities that are used by different parts of the system.**            |
| **Early increments act as a prototype** to help elicit requirements for later increments.             | The essence of **iterative processes** is that the specification is developed in conjunction with the software. |
| Lower risk of **overall project failure**.                                                            | ---                                                                                                           |
| **The highest priority system services** tend to receive the most testing.                            | ---                                                                                                           |

+ **Incremental delivery** combines advantages of **waterfall model** and **incremental development**, which can deal with software changes well.
+ **Boehm’s spiral model**: 1. Process is represented as a **spiral** rather than as a sequence of activities with backtracking; 2. **Each loop** in the spiral represents **a phase in the process**; 3. No fixed phases such as specification or design - **loops in the spiral are chosen depending on what is required**; 4. **Risks are explicitly assessed and resolved** throughout the process.
+ **Spiral model sectors**: Objective setting; Risk assessment and reduction; Development and validation; Planning (for the next phase).
+ **The Rational Unified Process (RUP)** is a modern generic process derived from the work on the UML and associated process, bringing together aspects of the 3 generic process models discussed previously.
+ RUP is organized into phases (inception, elaboration, construction and transition) but separates activities (requirements, analysis and design, etc.) from these phases.
+ **RUP phases\\(\downarrow\\)**

| Phase        | Activities                                                                  |
|--------------|-----------------------------------------------------------------------------|
| Inception    | Establish the business case for the system.                                 |
| Elaboration  | Develop an understanding of the problem domain and the system architecture. |
| Construction | System design, programming and testing.                                     |
| Transition   | Deploy the system in its operating environment.                             |

+ **RUP good practice\\(\downarrow\\)**

| Item                              | Desc                                                                                         |
|-----------------------------------|----------------------------------------------------------------------------------------------|
| Develop software **iteratively**  | Plan increments based on customer priorities and deliver highest priority increments first.  |
| Manage requirements               | Explicitly document customer requirements and keep track of changes to these requirements.   |
| Use component-based architectures | Organize the system architecture as a set of reusable components.                            |
| Visually model software           | Use **graphical UML models** to present static and dynamic views of the software.            |
| Verify software quality           | Ensure that the software meet’s organizational quality standards.                            |
| Control changes to software       | Manage software changes using a change management system and configuration management tools. |


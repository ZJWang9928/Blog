---
title: "软件工程复习笔记[12] - Configuration Management"
date: 2020-04-26T16:43:44+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["software", "notes"]
draft: false
---

Because software changes frequently, systems, can be thought of as a **set of versions**, each of which has to be maintained and managed.   
**Configuration management (CM)** is concerned with the **policies, processes and tools for managing changing software systems**.       

# CM Activities
### Change management
+ Keeping track of requests for changes to the software from customers and developers, working out the costs and impact of changes, and deciding the changes should be implemented.
### Version management
+ Keeping track of the multiple versions of system components and ensuring that changes made to components by different developers do not interfere with each other.
### System building
+ The process of assembling program components, data and libraries, then compiling these to create an executable system.
### Release management
+ Preparing software for external release and keeping track of the system versions that have been released for customer use.

## Change Management
Change management is intended to ensure that **system evolution is a managed proces**s and that **priority** is given to the most urgent and cost-effective changes.    
The change management process is concerned with analyzing the **costs and benefits** of proposed changes, approving those changes that are worthwhile and tracking which components in the system have been changed.    

### The change management process
![Pic](/images/2020/04/se-notes12/Picture1.png)

## Version Management
Version management (VM) is the process of keeping track of **different versions** of software components or configuration items and the systems in which these components are used.     
It also involves ensuring that changes made by different developers to these versions **do not interfere with each other**.         
Therefore version management can be thought of as the process of **managing codelines and baselines**.      

### Codelines and baselines
+ A **codeline** is a **sequence of versions of source code** with later versions in the sequence derived from earlier versions.
+ Codelines normally apply to components of systems so that there are **different versions of each component**.
+ A **baseline** is a definition of a **specific system**.
+ The baseline therefore specifies the component versions that are included in the system plus a specification of the libraries used, configuration files, etc. 
![Pic](/images/2020/04/se-notes12/Picture2.png)

### Version Management Systems
+ **Version and release identification**
+ **Storage management**
+ **Change history recording**
+ **Independent development** (The version management system keeps track of components that have been checked out for editing and ensures that changes made to a component by different developers do not interfere. )
+ **Project support** (A version management system may support the development of several projects, which share components.)

## System Building
System building is the process of creating a complete, executable system by compiling and linking the system components, external libraries, configuration files, etc.  
**System building tools and version management tools must communicate as the build process involves checking out component versions from the repository managed by the version management system**.     

![Pic](/images/2020/04/se-notes12/Picture3.png)

### Development, build, and target platforms
![Pic](/images/2020/04/se-notes12/Picture4.png)

## Release Management
A system release is **a version of a software system that is distributed to customers**.    
For mass market software, it is usually possible to identify two types of release: **major releases** which deliver significant new functionality, and **minor releases**, which repair bugs and fix customer problems that have been reported.         
For custom software or software product lines, releases of the system may have to be produced for each customer and individual customers may be **running several different releases of the system at the same time**.      

### Release tracking
In the event of a problem, it may be necessary to reproduce exactly the software that has been delivered to a particular customer.      
When a system release is produced, it must be documented to ensure that it can be re-created exactly in the future.     

### Release reproduction
To document a release, you have to record the specific versions of the source code components that were used to create the executable code.     
You must keep copies of the source code files, corresponding executables and all data and configuration files.      
You should also record the versions of the operating system, libraries, compilers and other tools used to build the software.       

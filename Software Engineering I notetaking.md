# Software Engineering I

[TOC]



## Introduction to SE

| Question                                                     | Answer                                                       |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| What is software?                                            | Computer programs and associated documentation. Software products may be developed for a particular customer or may be developed for a general market. |
| What are the attributes of good software?                    | Good software should deliver the required functionality and performance to the user and should be maintainable, dependable and usable. |
| What is software engineering?                                | Software engineering is an engineering discipline that is concerned with all aspects of software production from initial conception to operation and maintenance. |
| What are the fundamental software engineering activities?    | Software specification, software development, software validation and software evolution. |
| What is the difference between software engineering and computer science? | Computer science focuses on theory and fundamentals.<br />Software engineering is concerned with the practicalities of developing and delivering useful software. |

## Software Processes

![Software Process - SDLC](/Users/welkin/Library/Application Support/typora-user-images/image-20191216085610533.png)

#### The fundamental process activites:

- ***Software specification 软件规格*** the process of establishing what services are required and the constraints on the system’s operation and development. 

* ***Software development*** converting the system specification into an executable system. It consists of design and implementation activities. 
* ***Software validation 软件验证*** this is to show that a system conforms to its specification and meets the requirements. 
* ***Software evolution 软件演化*** ensures flexibility and conforming to changes. 

### Software process models

#### Waterfall model

This takes the fundamental process activities of specifica- tion, development, validation, and evolution and **represents them as separate process phases such as *requirements specification*, *software design*, *implemen-tation*, and *testing***.

##### Advantages

- Easy to understand and implement.
- Widely used and known (in theory!).
- Reinforces good habits: Define before Design, Design before Code.
- Identifies deliverables and milestones.
- Document driven. (文档驱动，意味着每个环节都有相应的文档详细说明)
- Works well on mature products and weak teams.

##### Disadvantages

- Doesn't reflect iterative nature of exploratory development. (无法反映开发过程中的迭代性或者说反复性)
- Unrealistic to expect accurate requirements so early in project. （早期立项就明确需求不现实）
- Difficult to integrate risk management.
- Difficult and expensive to make changes to documents.
- Significant administrative overhead, costly for small team and projects.

##### Appropriate for

- Embedded systems
  - The software has to interface with hardware systems
- Cirtical systems
  - need for extensive safety and security analysis of the specification and design. Safety-related problems in the specification and design are usually very expensive to correct at the implementation stage.
- Large software systems
  - part of broader engineering systems developed by several partners

###### Not appropriate for

- Informal communication is possible

- Software requirements change quickly

  #### Incremental(evolutionary) development

Defined to encompass flexibility and change anticipation.

System is developed as a series of versions.

##### Advantage

- The cost of accommodating changing customer requirements is reduced.
- The amount of analysis and documentation that has to be redone is much less than is required with the waterfall model.
- It is easier to get customer feedback on the development work that has been done.
- Customers can comment on demonstrations of the software and see how much has been
  implemented.
- More rapid delivery and deployment of useful software to the customer is possible.
- Customers are able to use and gain value from the software earlier than is possible with a
  waterfall process.

##### Disadvantages

- The process is not visible.
- System structure tends to degrade as new increments are added.

  #### Integration and configuration

Configuring reusable components for use in a new setting and integrating them into a system.

- Requirements specification
- Software discovery and evalutaion
- Requirements refinement
- Application system configuration
- Component adaptation and integration

##### Advantages

- Reduced costs and risks as less software is developed.
- Faster delivery and deployment of the system.

##### Disadvantages

- Requirements compromises are inevitable.
- Loss of control over evolution of reused system components.

  #### Code and fix

It starts with little or no initial planning. You immediately start developing, fixing problems as they occur, until the project is complete(hopefully).

##### Advantages

- No administrative overhead.
- Signs of progress code early.
- Low expertise, anyone could use it.
- Useful for small "proof of concept" projects.

##### Disadvantages

- Dangerous.
- No visibility / control
- No resource planing
- No deadlines
- Mistakes hard to detect / correct
- Impossible for large projects, communication breakdown, chaos.

  #### Spiral

- Is favoured for large, expensive and complicated projects.
- Used for risk management that combines the iterative development process.

##### Advantages

- Good for projects with many unknown risks.
- Good for large project.
- Early releases.

##### Disadvantages

- Spiral Model is not suitable for small projects as it is expensive.
- Too much dependable on Risk Analysis hence it needs very highly experienced expertise.
- Time management issues.

  #### Rapid prototyping

  #### Unified Process

  #### Agile -> XP

  #### Three stages of software testing:

  * ***Component testing 组件测试*** each component is tested individually usually by its own developer.
  * ***System testing*** tests the whole system as an integrated software and tries to find out any problem which is due to interaction between the component.
  * ***Acceptance(customer) testing 验收测试*** the system is tested by the user/customer/potential customer. It also shows how well the software is meeting the requirements.

  #### Real-time system

  Real-time systems usually involve many hardware components which are not easy to change and cannot be incremental. Also real-time systems usually safety critical which needed be built based on **well planned process**. 

  #### Functional & non-functional requirements

  * ***Functional requirements***

  Stataments of services the system should provide how the system should react to particular inputs.

  * ***Non-functional requirements***

  Constraints on the services or functions offered by the system such as timing constraints, constraints on the development process, standards, etc.

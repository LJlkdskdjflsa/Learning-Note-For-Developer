# Data Flow Diagram
source: https://www.lucidchart.com/pages/data-flow-diagram#:~:text=data%20flow%20diagram.-,Symbols%20and%20Notations%20Used%20in%20DFDs,-Three%20common%20systems
## What is a Data Flow Diagram
- flow of information for any process or system
- to show data 
    - inputs
    - outputs
    - storage points
    - routes between each destination
- can be used to 
    - analyze an existing system
    - model a new one

## Symbols and Notations

Using any convention’s DFD rules or guidelines, the symbols depict the four components of data flow diagrams.
- External entity
  - outside system that sends or receives data, communicating with the system being diagrammed
  - sources and destinations of information entering or leaving the system
  - might be an outside organization or person, a computer system or a business system
  - typically drawn on the edges of the diagram
- Process
  - any process that
    - changes the data
    - producing an output
  - perform
    - computations
    - sort data based on logic
    - direct the data flow based on business rules
  - A short label is used to describe the process, such as “Submit payment.”
- Data store
  - hold information for later use
    - file
    - repository
  - Ex
    - database table
    - membership form
  - Each data store receives a simple label, such as “Orders.”
- Data flow
  - route that data takes between the external entities
    - processes
    - data stores
  - portrays the interface between the other components
  - shown with arrows
  - labeled with a short data name, like “Billing details.”

## DFD rules and tips
- Each process should have at least one input and an output.
- Each data store should have at least one data flow in and one data flow out.
- Data stored in a system must go through a process.
- All processes in a DFD go to another process or a data store.

## DFD levels and layers: From context diagrams to pseudocode

A data flow diagram can dive into progressively more detail by using levels and layer
DFD levels are numbered 0, 1 or 2, and occasionally go to even Level 3 or beyond

### Level 0 (Context Diagram)
- basic overview of the whole system or process being analyzed or modeled
- designed to be an at-a-glance view
- showing the system as a single high-level process (with its relationship to external entities)
- should be easily understood by a wide audience
  - stakeholders
  - business analysts
  - data analysts
  - developers

### Level 1
detailed breakout of pieces of the Context Level Diagram
- highlight the main functions carried out by the system
- break down the high-level process of the Context Diagram into its subprocesses

### Level 2
goes one step deeper into parts of Level 1. It may require more text to reach the necessary level of detail about the system’s functioning

### Levels 3, 4 and beyond is possible
but going beyond Level 3 is uncommon(create complexity that makes it difficult to communicate, compare or model effectively)

> Using DFD layers, the cascading levels can be nested directly in the diagram, providing a cleaner look with easy access to the deeper dive.

> sufficiently detailed in the DFD, developers and designers can use it to write pseudocode (combination of English and the coding language, facilitates the development of the actual code)

## how DFDs can be used
well suited for analysis or modeling of various types of systems in different fields

DFD in business analysis
- Business analysts use DFDs to analyze existing systems and find inefficiencies
  - Diagramming process can uncover steps that might be missed or not fully understood
  - model a better, more efficient flow of data through a business process

DFD in agile development
- visualize and understand
  - business
  - technical requirements
  - plan the next steps
- can be a simple yet powerful tool for communication and collaboration to focus rapid development

DFD in system structures
- Any system or process can be analyzed in progressive detail to improve it, on both a technical and non-technical basis


## Compare

### DFD vs. Unified Modeling Language (UML)
- DFD 
  - illustrates how data flows through a system
  - provide a good starting point
- UML
  - modeling language used in Object Oriented Software Design to provide a more detailed view
  - use when actually developing the system
  - UML diagrams such as class diagrams and structure diagrams to achieve the required specificity

### Logical DFD vs. Physical DFD
two categories of a data flow diagram
- Logical DFD
  - visualizes the data flow that is essential for a business to operate
  - focuses on
    - business
    - information
    - not on how the system works or is proposed to work
  - processes would be business activities
- Physical DFD
  - how the system is actually implemented now
  - how it will be
  - processes would be programs and manual procedures

## How to make a data flow diagram
Tools
- draw.io
- Lucidchart

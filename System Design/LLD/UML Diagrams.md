# UML Diagrams

## UML Overview
- UML -> Unified Modeling Language
- It is a standard visual language for mapping out how a software system is structured and how it behaves.
- UML helps to understand the software.
- UML helps to:
  - Catch design mistakes before they end up in code.
  - Get everyone on the same page without reading thousands of lines.
  - Leave behind documentation that actually makes sense months later.
  - Plan changes without guessing what will break. 
- Every UML diagram belongs to one of the 2 camps:
  1. Structural: It shows what the system is made of. Classes, objects, components, how they connect.
  2. Behavioral: It shows what the system does. How the objects interact, how the data moves, how the system responds to events. 
  <img width="429" height="102" alt="image" src="https://github.com/user-attachments/assets/dc46d683-86f1-4500-aaa1-72ee42c7a76c" />

- Let's understand each one:
  1. Structural diagrams:
     1. class diagrams:It shows the class, what data does it hold, what it can do, and how they relate to each other. This is the backbone of any OOP design. 
     2. object diagrams: It is the class diagram, but frozen at one moment in time, it has real objets with actual values instead of blueprints. 
     3. component diagram: It steps back from classses and shows bigger chunks of modules, services, libraries and how they plug into each other. 
     4. deployment diagram:It shows where our software actually lives, runs servers, containers, cloud nodes, and devices. 
     5. package diagram:It groups related classes into packages and shows how those packages depend on each other. It is useful for large codebase. 

  2. behavioral diagrams:
     1. use case diagram: It shows what our system does from the user's point of view. No code, no implementation, just what actions users can take. 
     2. sequence diagram: It shows the exact order in which objects talk to each other. It is useful for tracing how a single request moves through our system. 
     3. activity diagram: It is the flowchart of the system. Here, steps, decision, parallel paths maps a process from start to finish. 
     4. state diagram: It tracks an object through every state it can be in and what triggers i to move from one state to the next. 
     5. communication diagram: It is similar to sequence, but focused on which objects are connected to which, rather than order of messages.
<img width="362" height="201" alt="image" src="https://github.com/user-attachments/assets/25a0918d-59e3-4105-89a4-46ceeea2150d" />
 <img width="356" height="310" alt="image" src="https://github.com/user-attachments/assets/533a43de-9932-468d-a1a7-200bb255095b" />

- IMP -> class diagram and sequence diragm (these are mostly used)
- A diagram earns its place only if it makes the next conversation shorter.

## Class Diagram
- This is the most commonly used diagram.
- A class diagram of a system shows what classes exist, what data do they hold, what they can do, and how they relate to each other.

1. Anatomy of class:
   - Class is a box, chopped in 3 parts, viz, class name, data, and action etc.
 <img width="339" height="308" alt="image" src="https://github.com/user-attachments/assets/06d0a880-4787-44a9-bfb5-c48e78bddb42" />
     a. name: class name written in PascalCase.
     b. data: (varNameL:type) ("-" denotes provate data/field)
     c. action/ method: (methodName: returnType) ("+" denote spublic method)
   <img width="367" height="180" alt="image" src="https://github.com/user-attachments/assets/2ca5eca8-606d-42c6-8a30-4b8868f8bac3" />
  
2. Special class types:
   - Below:
     1. interfcaes:
        - Any class that connects to "Trackable" with dashed arrow is signing up to deliver every method written inside it. No exceptions. 
        <img width="353" height="174" alt="image" src="https://github.com/user-attachments/assets/7ca22ad2-6bbf-40ef-bd3f-890583f3b4ab" />

     2. abstract classes:
        - An abstract class is a blueprint that is only partially filled in. Some methods come ready to use, others are left blank on purpose for child classes to complete. The <> tag marks it at the top. 
         <img width="348" height="287" alt="image" src="https://github.com/user-attachments/assets/9c633890-4280-4eb1-ae83-fbe6da517a0a" />
        - Pick an abstract class when child classes share real data or common behaviour. Pic an interface when completely unrelated classes just need to follow the same set of rules. 
     3. enumerations:
        - An enumeration pins down a fixed collection of named values. The <> tag identifies it and the possible values are listed in the body.
           <img width="254" height="311" alt="image" src="https://github.com/user-attachments/assets/cfe2d197-2927-417e-bf85-ad8cddaa1194" />

        - Hardcoding the values as plain strings is asking for typos and bugs. An enum keeps the options locked and safe. 

3.  Relationshipd:
    - The lines between classes shwo the relation between them.
    - There are 6 types of relationships:
      
      1. Dependency loosest connection:
      2. Association:
      3. Aggregation:
      4. Composition:
      5. Inheritance:
      6. Realization: 
    - 



5.  



- 

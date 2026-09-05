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

3.  Relationships:
    - The lines between classes shwo the relation between them.
    - There are 6 types of relationships:
      
      1. Dependency loosest connection: 
         - Here, one class briefly reaches into another to get somethign done, maybeb as a method argument or as a temporary local variable.
         - Here, nothing is stored. Once the method call ends, the connection vanishes.
         - "---->" is the UML symbol for dependency.
         - example: Here, NotificationService borrows EmailClient for one operation and then lets it go. It never keeps it around as field. 
                         <img width="365" height="72" alt="image" src="https://github.com/user-attachments/assets/cdcf3032-de88-4332-81d3-0f3d0681d7ab" />

         - Use it when, class A needs class B only inside a single method and stores nothing afterward. 

      2. Association:
         - Here, one class holds onto another as a permanent field. The bond outlasts any single method call. Each object has its own lifespan and neiher depends on the other to exist.
         - "->" (solid arrow) is the UML symbol for association.
         - example: Doctor keeps a list of patient objects as field. If the patient is removed, the doctor still exists. And the other way around too. 
                        <img width="358" height="113" alt="image" src="https://github.com/user-attachments/assets/860f4f15-c585-4dd9-8556-47ff44ffa6fc" />
         - Use it when: class A keeps reference to class B as a field and both libe independently. 

      3. Aggregation:
         - Here, a has-a relationship where the container holds the parts but does not own them. The parts were brought in from outside and can outlive the container or even belong to several containers at once.
         - The hollow diamond is UML symbol for aggregation.
         - Example: Close the library and the books still exist, they can move to another library or sit in storage. They were never exclusively owned.
                <img width="368" height="117" alt="image" src="https://github.com/user-attachments/assets/1ae30c58-54bc-4c04-9e39-dcf036c1ceca" />

         - Use it when: One class groups other class objects together, but the grouped objects have their own independent existence. 

      4. Composition:
         - A has-a relationship where the container fully owns the parts. The parts are born inside the container and cease to exist the moment te container is destroyed. They have no life outside it.
         - The filled diamond is the UML symbol for composition.
         - Example: An InvoiceItem is created inside Invoice and belongs to it completel. Delete the Invoice and every item on it disapprars with it. An item on its own mean nothing.
                        <img width="365" height="102" alt="image" src="https://github.com/user-attachments/assets/71602e1c-ec7d-4785-96ea-3215f303eb8a" />
  
         - Use it when: The parts exists purely to serve the whole and has no purpose or identity on its own. 

      5. Inheritance:
         - A is-a relationship. 
         - The child class absorbs everything the parent has, all its fields and methods, and is free to add more or override what is inherited.
         - The solif like with hollow triangle pointing at the parent is UML symbol for inheritance.
         - Example: Both PushNotification and EmailNotification inherit title, message, and getTitle(). Each overrides send() in its owns way and brings its own extra behavior.
                 <img width="383" height="283" alt="image" src="https://github.com/user-attachments/assets/f882da5a-5361-43b2-8210-6ae31fc467c1" />

         - Use it when: A real is-a relationship exists and the child genuinely needs the parent's data and behaviour. If it feels like stretch, composition is better call.  

      6. Realization: 
         - The bond between an interface and the class that fulfills it. The class is making a binding commitment to implement every single method the interface declares.
         - The dashed line with hollow triangle pointing at interface is UML symbol for realiation.
         - Example: SalesReport and InventoryReport share no parent class and have nothing in common structurally but both honor the Exportable contract and deliver their own version of each method. 
               <img width="373" height="253" alt="image" src="https://github.com/user-attachments/assets/1afce777-94f5-4274-942a-dd2a4840b0a7" />

    - **Inheritance vs Realization: ** Inheritance hands us actual working code from parent. Realization hands us a set of rules to follow and trust us to write the code ourselves. Solid line for inheritance, dashed line for realization.
    - **Relationship strength spectrum:** Always reach for the weakest relationship that gets the job done. If we don't need a stored reference, dependency beats an association. If the part can live on its own, aggregation beats composition. Looser coupling means our code bends without breaking.
             <img width="581" height="74" alt="image" src="https://github.com/user-attachments/assets/f31c7765-a432-4f08-b9d4-aaae07d8fb6f" />


4.  Hospital management system : Example:
    <img width="547" height="271" alt="image" src="https://github.com/user-attachments/assets/78f0b149-27f6-4e19-b53e-d2e8748a5e0f" />


## Use case diagram
- Here, the use case diagrams don't care about code. It doesn't care about databases or APIs. They simply capture who interacts with the system and what those people need it to do, nothing mroe, nothing less.
- A use case diagram is a behavioral UML diagram that shows who interacts with your system and what they can do with it.
- Think of it as a menu for your system. Not how the kitchen prepares the food just what is available to order and who is allowed to order it.
- It answers one simple question: "Who does what with this system?"
-  It does not touch implementation, class design, database structure, or any technical detail whatsoever. A use case diagram lives purely at the requirements level it captures scope and user goals and stops right there
-  Example:
      <img width="373" height="199" alt="image" src="https://github.com/user-attachments/assets/de63217d-5037-45fa-814f-871541b58dc7" />

- Use case diagrams are typically created early right after requirements are gathered and before any detailed design begins. They act as a bridge between what the business needs and what the development team will build.
- Interview Tip In LLD interviews ->  you rarely need to draw a formal use case diagram on the spot. But taking sixty seconds to list out the actors and what each one needs to do before jumping into classes signals structured thinking and stops you from missing key requirements halfway through your design.

### Building blocks of use case diagram
- A use case diagram has four building blocks: actors, use cases, the system boundary, and relationships. Simple on their own powerful when combined.

1. Actor:
   - An actor is anything that lives outside the system and interacts with it. Usually a person a customer, an admin, a driver but it can also be an external system like a payment gateway or a notification service.
   - Actors come in two types and where you place them matters:
   - Primary actors kick things off. They have a goal and they come to the system to achieve it. A customer placing an order is a primary actor. Draw them on the left.
   - Secondary actors get called upon. The system reaches out to them during a use case. A payment gateway does not decide to charge someone on its own the system calls it during checkout. Draw them on the right.
             <img width="366" height="54" alt="image" src="https://github.com/user-attachments/assets/b61db160-c273-41fa-85ee-dd8450587a18" />

2. Use cases:
   - A use case is a specific goal the system helps an actor accomplish. It represents a complete interaction something that starts, does something meaningful, and ends with a result.
   - Three rules for writing good use cases:
     1. Start with a verb. Write "Place Order" not "Order Placement." The verb keeps it action-focused.
     2. Represent a complete goal. "Place Order" is a full goal. "Click the submit button" is a single step too small.
     3. End with a meaningful result. After "Place Order" completes, the customer has a confirmed order. That is a real outcome.
     
3. System boundary:
   - The system boundary is a rectangle that wraps around all the use cases. It draws a clear line between what your system does and what lives outside it. The system name sits at the top.
   - Actors always sit outside the box. Use cases always sit inside.
                <img width="383" height="316" alt="image" src="https://github.com/user-attachments/assets/55736e6b-2937-415c-a64b-78ffda69ef38" /> 

4. Relationships:
   - Relationships are the lines that connect actors to use cases and use cases to each other. There are four types.
     1. Association:
        - A solid line between an actor and a use case. It simply means this actor takes part in this use case. The most common line you will draw.
              <img width="344" height="66" alt="image" src="https://github.com/user-attachments/assets/c516357c-a188-4acd-a13b-8aacc46f6a4d" />

     2. Include:
        -  One use case always triggers another as a required step. The base cannot finish without it. The arrow points from the base use case to the one it pulls in, labeled <>.
        -  "Place Order" always includes "Process Payment." There is no order without a payment. Every single time, no exceptions.
                   <img width="348" height="62" alt="image" src="https://github.com/user-attachments/assets/d6d1c236-d31b-42ab-9bd2-658b16c70e8d" />

     3. Extend:
        - One use case optionally adds extra behavior to another under certain conditions. The base use case runs perfectly fine on its own. The arrow points from the optional use case back to the base, labeled <>.
        - "Apply Promo Code" extends "Place Order." Most customers just order without a code. But when they have one, the option is there. The base use case never changes.
                <img width="350" height="49" alt="image" src="https://github.com/user-attachments/assets/18483a0b-522e-4423-9f2a-f5e48f05ad60" />
   
     4. Generalization:
        - Inheritance between actors or between use cases.
        - A child actor inherits every use case the parent has access to, and can have additional ones of their own.
        - The arrow points from child to parent with hollow triangle tip.
        - An ADMIN is also a User. They can do everything a regular user can, such as browse items, place order etc.. But, they can also manage entire platform on top of that.
                <img width="354" height="68" alt="image" src="https://github.com/user-attachments/assets/b073ab1a-8099-43ba-9a4c-9737a714b63f" />

- Steps to build a Use case Diagram:
  - Example: Online food delivery app
    1. Identify the actors:
       - Ask who or what interacts with this system from the outside?

         -  Customer (primary): place order, track deliveries
         -  restaurant owner (primary): manages menu and incoming orders
         -  delivery drivers (primary): picks up delivery and delivers orders
         -  payment gateway (secondary): Gets called by system during checkout 

    2. list the use cases:
       - Ask what does each actor actually want to accomplish?
   
         - Customer: Browse restaurants, place order, track order, cancel order, leave review
         - restaurant owner: manage menu, accept order, view earning
         - delivery driver: accept delivery, update delivery status
         - system level: process payment (triggered during placeing order)

    3. Define the system boundary:
       - Draw a box around all the use cases and label it. Everything inside is what our system builda and owns. Everything outside the actors is external.

    4. draw associations:
       - Connect each actor with the use cases they take part in with solid lines.
         - Customer connects to browse restaurants, place order, track order, cancel order, leave review
         - Restaurant owner connects to manage menu, accept order, view earning
         - Delivery driver connects to accept drlivery, update delivery status
         - payment gateway connects to process payment 

    5. Add include, extend and generalization:
       - Look for mandaory steps and optional additions:
         - Place order and process payment: Every order must go through payment, no exceptions.
         - place order and apply promo code: Customers can optionally apply a discount code.
         - Premium customer generalizes customer a premium customer can do everything a regular customer can, plus access exclusive deals.

            <img width="446" height="365" alt="image" src="https://github.com/user-attachments/assets/5277ccb8-7978-4acd-a509-d8002e336885" />

- Reading the Diagram
  - Green nodes on the outside are actors they live outside the system boundary
  - Blue nodes inside the box are use cases things the system provides
  - Red node is the optional extending use case
  - Yellow node is the secondary actor called by the system, never initiates
  - Solid lines are associations, dashed labeled arrows show include and extend, and the hollow arrow shows generalization

## Sequence diagram:
-  Every step happens in a fixed order. Each person/ system waits for the previous one to finish before doing their part.
-  That is exactly what a Sequence Diagram shows in software.
    <img width="357" height="207" alt="image" src="https://github.com/user-attachments/assets/40f3f8a0-4897-41a2-8c40-40e39b3716db" />

- A sequence diagram is a UML diagram that shows how different parts of a system talk to each other to get something done.
- It focuses on three things who the participants are, what messages they exchange, and in what order those messages happen.

### Importance of sequence diagrams
1. See runtime behavior at a glance:
   - Code execution jumps across classes, services, and layers. A sequence diagram puts the entire flow on one page, every call, every response, every handoff, without reading a single line of code. 
2. Debug complex flows faster:
   - When something breaks in production, we need to know the exact order of events. Sequence diagram make these flows easy to understand. 
3. Bridge the gap between requirements and code: 
   - A use case diagram tells the requirements. A class diagram shows which classes exist. Neither one tells us the exact sequence of calls that make it happen. Sequence diagram fill that gap.
  
### Building blocks of sequence diagram:
- Every sequence diagram is built from four elements
1. Actors and participant:

2. Lifelines:

3. Activation Bars:

4. Messages:


- 

# **DDD Concept**

## **What is Domain-Driven Design?**

Domain-Driven Design is a software design approach that focuses on modelling the software to match the business domain.  
  
The book uses the term "model" to express the _organized and selectively abstracted knowledge of the domain experts_. 
Meaning that it's not about a specific diagram, document or "model" in the context of programming. 
But basically whatever shows what the system should do and how to do it is referred to as the model.  
  
The domain is used in the book to refer to the business domain so if your software is for booking taxis, 
the domain would be the business of ride hailing.

### **The Utility of a Model**

In the preface of **part I**, the factors defining the utility of a model are:

1. Model and Implementation shape each other
2. Model language is used by all teams (technical and business)
3. The model is the shared understanding of the domain (shared by the developers and the domain experts)

## **Chapter 1: Crunching Knowledge**
The first chapter explains the process of creating a good model. The process should involve meetings between the 
developers and the domain experts to create the model and refine it to achieve the business goal.

The process is iterative, meaning that it's not a single meeting but multiple meetings that all incrementally create 
an effective model.

### **Effective Modeling**

These are how modelling should be to produce an effective model

1. Bind the model to the implementation.
2. Cultivate a common language based on the model.
3. Develop a knowledge-rich model that shows the objects' behavior and how that solves the problem.
4. Distill the model by removing unnecessary details.
5. Brainstorming and experimenting.

## **Chapter 2: Communication and the Use of Language**

The model's language is the set of terms and relationships that are used in the model.  
As explained in chapter 1, the model's language should be understood by both domain experts and developers. 
Meaning, it should be tailored to the domain and be precise enough to be used for technical development.

As the model keeps changing to better fit its purpose, the language evolves as well by input from both the domain 
experts and the developers. A good language should be used by all teams and if that's not the case; then the model 
should be enriched to provide a good common language.

In the meetings, involved parties should play with words and phrases to find the easiest way to express the domain 
and model it. It's ok to abstract technical details for the domain experts but the core model should be known to them.

### **Diagrams**

It's important to know that diagrams do not need to have the full context of the model. The goal of the diagram 
is to explain the model, and they should be as simple as possible. You should use diagram and documents to explain 
the model, but they don't have to be a replica of the model. In my opinion, a good model should be simple enough 
that it's understood from the code itself.

## **Chapter 3: Building Model and Implementation**

This chapter stresses on the importance of binding the model to the implementation. A model can be very 
descriptive of the domain while being unrepresentable in code. If the implementation is not considered while 
building the model:

* The model can have objects and relations that cannot be stored in a database.
* Model will detail irrelevant subjects and ignore important subjects.
* Some discoveries about the domain might not be apparent without considering the implementation details.

### **Design Model vs Analysis Model**

There are 2 types of models:

* ***Analysis Model***: Captures the fundamentals of the domain concepts in comprehensible, expressive way. (Focuses on 
the domain)
* ***Design Model***: Specify a set of components that can be constructed using programming tools and correctly solves 
the problem. (Focuses on the implementation)  

The book introduces another approach ***Model-Driven Design***.

### **Model-Driven Design**

Model-Driven Design integrates both Analysis and Design models, and it serves both purposes.

To build the model:

1. Design the model to reflect the domain in every literal way.
2. Revisit the Model and modify it with implementation in mind.  

If you have multiple systems then they can have different models, however, within a single sub-system you should 
have only one model. Development of that system then becomes an iterative process of refining the model, the design 
and the code as a single activity. If the code changes then the model must be changed and vice versa.

### **Modeling Paradigms and Tool Support**

***Object-oriented languages*** (Java) are powerful for implementing the model since they are already based on a 
modeling paradigm.

***Logical languages*** (Prolog) are also a good fit for model-driven design because they declare a set of logical rules 
for the design to follow.

***Purely procedural languages*** (C) are not a good fit because it has no modeling paradigm. Procedural languages rely on 
telling the computer what to do step by step and that's it.

### **Letting the Bones Show**

The underlying model of the system should be the same model shown to users. Otherwise, confusion will arise and 
the language will not be descriptive enough. Having different models can also cause unexpected behaviors to the 
users. An example to that is how Microsoft Internet Explorer modeled the bookmarks.

For the user, "Favorites" was a list of URLs but to the actual implementation a "Favorite" is a file that contains
a URL. This meant that URL names had to follow windows file naming restrictions. So if a user saved a URL and 
named it "AOT: 3" they will get an error stating that file names cannot contain special characters like ":" which 
is very confusing.

!!! warning "Hands-On Modelers"

    Separating the responsibility of the modeling and coding between different team members violates binding the model
    to the implementation. It eventually renders the model useless therefore the developers should be able to express
    the model and contribute to it and anyone who touches the model should participate in the implementation.

## **Chapter 4: Isolating the Domain**

It is important to understand that most of the code is not about the domain but about the technology of the software
itself. We need to isolate the domain objects from the rest of the code so we do not lose sight of the domain.

For example, when you set the destination in a ride hailing app, you go through different parts of the system:

1. The frontend widget
2. The query that gets the possible destinations from the database
3. User input validation
4. Associate selected destination with the trip
5. Commit changes to the database

Only a little portion of that is part of the business domain.

Often UI, database and other code is written directly in the domain objects because it is faster to get things done. 
This makes the project harder to understand and harder to scale. To avoid that, systems should be implemented with 
separation of concerns and that's what ***Layered Architecture*** aims at.

### **Layered Architecture**

Layered architecture divides the program into layers where each layer is responsible for an aspect of computer 
science. The layers are:

1. UI or Presentation layer
2. Application layer which coordinates tasks between domain objects and it has no business rules
3. Model layer or domain layer which is the most important layer for Model-Driven Design
4. Infrastructure layer which provide technical capabilities to support the above layers

An example to that can consist of:

| **Layer**      | **Component**                                                    |
|----------------|------------------------------------------------------------------|
| Presentation   | Frontend web interface                                           |
| Application    | Backend for Frontend (bff)                                       |
| Domain         | Specialized Microservices handling different entities each       |
| Infrastructure | Interfaces that utilize infrastructure (ex. Rabbit MQ, Database) |

#### **Layered Architecture Specifications**

1. Partition system to layers according to the responsibilities
2. Each layer should depend only on the layers below it. This is achieved by using the public interfaces of the 
layers below.
3. Each layer is loosely coupled to the layer above it. To achieve that, layers should communicate with the layers 
above them through callbacks or observers.
4. Infrastructure layer should know nothing about the domain and should be used as services.
5. The Domain-Driven Design approach requires only the domain-layer to be separated, other layers can be mixed and 
the system can still follow Domain-Driven Design.	
	
#### **Advantages of Using Layered Architecture**
1. Design is a lot cleaner
2. Less expensive to maintain since layers are loosely coupled
3. Easier to scale because each layer can evolve separately

#### **Disadvantages of Using Layered Architecture**

1. Has a learning curve that inexperienced developers might struggle through
2. Takes longer time to design and implement	
	
That's why another architecture is introduced as an "anti-pattern" to Layered Architecture and that is Smart UI.

### **Smart UI**

If the project is not very ambitious or is not expected to scale, or if the developing team is inexperienced 
then Smart UI is probably the way to go.

#### **Smart UI specifications**

1. Business logic is implemented in the UI
2. Application is divided to small functions and each function is implemented as a separate UI.
3. Relational databases are used as a shared data repository
4. Use the most automated UI building tools available. (I'm not sure what the book means by that exactly but I think it's about tools that let you drag and drop components to form the UI and then wire your code to it.)
5. Use fourth generation languages (4GL)

#### **Advantages of Using Smart UI**

1. Fast and easy to develop
2. Can be changed quickly since it's divided to small functions and small UIs
3. Easy to expand the system since the UIs are decoupled
4. Using relational databases provides integrity at data levels
5. 4GL tools work well
6. Maintenance team can quickly redo parts of the code that they don't understand since changes should be localized 
to each separate UI

#### **Disadvantages of Using Smart UI**

1. Integrating applications is difficult since the tools and languages are not flexible
2. Business problem is not abstracted which means behavior cannot be reused
3. Lack of abstraction limits refactoring options
4. No flexibility to add richer behavior
5. If need for scalability arises later, system cannot migrate from this approach except by replacing the entire app.

## **Chapter 5: Connecting Model and Implementation**

This chapter focuses on connecting model and implementation. The chapter is large, and it has a lot of important 
information that's why I decided to split it.

### **Patterns of Model Elements**

***Entities***: Something with an identity relevant to the domain. It has a state and is continuous through the 
system even if the state changes. (example: a transaction in a banking system)  

***Value Objects***: Describes an attribute or a value of something else. It has no identity or state and is 
often created for an operation then destroyed (example: address of the customer in a banking system)  

***Services***: Actions or operations that are done on client's requests. They belong to no specific object. 
(example: sending an email of a transaction in a banking system)

=== "**Entities**"

    * Entities are objects that are in the core of the domain. The system needs to have the state of those objects and 
    they need to be identified.

    * The implementation of entities should focus on the life cycle of the entity more than the attributes. The entity 
    itself is important regardless of its attributes.

    * Across a system, entities might have different implementations in the sub-systems. However, In all implementations 
    the objects must refer to the same entity.

    * Entities have an operation that gives a different value for each instance of the entity. This operation is used 
    for identifying the instance.

    * The domain model defines what it means for 2 objects to be equal.

    * A quick way to identify an entity is to answer the question "Does the user need to know if X is the same entity 
    as before?" or in other words "Does the user care about the identity of it?"

    * An example to that is having a customer object in a banking system. The customer itself is important to the User 
    and the system must allow for the identification of different customers. The customer might fail to pay a loan, or 
    they might own a number of accounts. The identity of the customer is important to know which customer owns which
    accounts or which customer failed to pay.

    !!! note "Tips: Modeling Entities"

        1. Don't focus on the attributes, strip the object down to the most fundamental characteristics. The attributes 
        that define the object and that are used to match the object with other objects that belong to the entity.

        2. Add behavior that is essential to the identification of the entity and attributes that are used by that behavior.

        3. Remove other behaviors and attributes to other objects and associate them with the entity.

        4. An example to that is a customer object. a customer would have a unique id, a name and an address. According 
        to the domain of banking, the customer is not identified by the address but it is identified by its id and name. 
        Thus, the address attributes should be moved to another "Address" object.


    !!! note "Tips: Designing the Identity Operations"

        1. Defining attributes must be unique. For example, the national ID of a customer.

        2. If there is no unique attributes, then attach a unique ID to the object. Most databases do that automatically.

        3. Identify what makes 2 objects the same entity.

        4. Identity operations can require human input if the ID does not correspond to something in the business domain.

        5. When an entity needs matching on two different systems whose IDs are different, a third system is used for 
        attaching for example the national ID or the Phone number

=== "**Value Objects**"

    * Value objects have no identity but their attributes describe something.

    * If you only care about the attributes of the object but not the identity, then it is a value object

    * An example to that is a marker pen. If you're coloring, you don't care which pen you use, but you care about its
    color. So all instances of the pen that have the same color are the same, and it doesn't matter which pen you're using.
    The pen is defined by its attributes but the pen itself has no identity.

    * ***Is an address a value object or an entity?***
    This is an important question. The short answer to that is "It depends". It depends on the domain. 
    If you're running a delivery service, then knowing if 2 people who made an order live in the same address is 
    not important so it is a Value Object. However, if the system plans the delivery route then it is important to 
    know that they have the same address so they are delivered together then it is an Entity.
    The same object can be an Entity or a Value Object depending on the domain.

    * ***Designing Value Objects***

        1. Which instance of the object we have doesn't matter as long as they have the same attributes.
        2. Caring only about the attributes and not the identities allows for more optimization. If the value 
        object is an electrical outlet in a home designing system, then we can either create multiple copy of the 
        outlet or create one and refer to it a hundred times. ***When should we copy and when should we share?***
  
    Copying will send only a copy of the object, but it means that you use more space to store redundant copies. 
    However, sharing sends a reference to the object, but it also sends a message for every change to the object so 
    the main object changes. In short, Copying uses more space but less network overhead while sharing uses less 
    space but more network overhead unless the object is immutable.

    1. Associations should not be bidirectional. If bidirectional associations are needed then consider changing 
    the object to an entity. That's because Value Objects have no identity so having a bidirectional relationship
    makes no sense.

=== "**Services**"

    * Services are operations that belong to no specific object, but they are important to the domain.

    * Not anything that has no home in an Entity or a Value Object is a Service. You must first try to place the operation in an Entity or a Value Object and not give up and make it a Service.

    * Services have no state and no meaning beyond their operation.

    * The parameters and results should be domain objects.

    * To keep the design clear, those operations are declared as services instead of creating an object that represents nothing in the domain.

    !!! note "Tips: Good Service Characteristics"

        1. The operation is part of the domain but it is not a natural part of an Entity or a Value Object.
        2. The interface is defined in terms of other domain elements.
        3. The operation is stateless. A service can use global system info or change them but the service itself should have no state.

    * ***Service and the Isolated Domain Layer***  
    This is a pattern focused on distinguishing domain layer services from services in other layers. Usually, 
    a Service is something in the infrastructure layer like a mailing service, and they lack any domain meaning. 
    Services that have domain meaning are in the domain layer. But differentiating between services in the application
    layer and domain layer can be confusing. For example, If we have an operation that exports the transactions as excel
    then it should be on the application layer because the domain has no concept of file formats. However, an operation
    that transfers money from one account to the other should be in the domain layer because that's where the business
    rules reside and the action is a core part of the domain. Here is a more detailed example from the book 
    (I made some changes to it).  
  
    A request to make a transfer from one account to another might be processed like this:

    !!! note "Example: Exporting Transactions"

        1. Application Layer: 
            - Get input request 
            - Ask domain layer service to do the transfer and wait for response 
            - Send notification through mailing service in infrastructure layer

        2. Domain Layer:
            - Check if transfer is possible according to business rules 
            - Make changes in account balances to transfer fund 
            - Return result to application layer service

        3. Infrastructure Layer:
            - Send notification email

    * ***Granularity***  
    Making services medium-grained allows for more reusability because the service has a lot of functionalities.
    Having a fine-grained service can cause the application layer to have a lot of domain details so it can coordinate 
    those services which blurs the line between application and domain layers. It is better to have medium-grained 
    services to avoid that.

### **Low Coupling and High Cohesion**

The first rule of creating good modules, is that modules should have low coupling between each others and each module should be highly cohesive in itself.

#### **Low Coupling**

Low coupling means that each module should be independent of other modules in the system. When that's achieved; it gets easier to understand the model since you're dealing with a small part of the system at a time without the need of thinking about the whole system (objects outside the module).

#### **High Cohesion**

High cohesion between the objects of the systems which have related responsibilities means that the components of the module are all related thus making it easier to understand what the module does as a self-contained subsystem as each module should focus on one idea/responsibility.

### **Choosing Modules**

The meaning of the objects in the domain should drive the choice of the module and keep in mind that putting some classes together in a module tells the other developers that they should think of these classes together.

If the model is telling the story of the system then the modules are the chapters and they should be named in ways that convey their meanings. The names should be part of the Ubiquitous language (the common language between the technical team and the domain experts)

So modules should:

1. Tell the story of the system
2. Contain cohesive set of concepts
3. Have low coupling with other modules
4. Have meaningful names that become part of the Ubiquitous language

You might need to choose between technical cohesion and conceptual cohesion. In that case, choose conceptual cohesion because the developers can handle the results of low technical cohesion if they correctly understand the story of the model.

### **Agile Modules**

Modules will need to be refactored as the system grows, however, since modules are large; refactoring them is disruptive and harder than refactoring model objects. That leads to modules reflecting a much older version of the model.

Initially, modules are highly coupled because the model changes as the project goes on. Lack of refactoring keeps that inertia going. That's why modules should be easy to refactor and easy to communicate to other developers what modules do.

Some frameworks divide entities into modules based on their technical responsibilities (example: separating entity's business logic from data access) but this makes modules harder to understand and therefore increases the cost of development and refactoring. The justification given for doing that is that the modules might be deployed on different machines but most of the time that doesn't happen and doing that is costly because:

1. The code no longer reveals the model
2. The developers no longer understand the model or reason about it as meaningful pieces. 

Minimize the technical partitioning restrictions and apply only the essential ones. The most important purpose of the module is to separate the domain layer from other code.

### **Modeling Paradigms**

This part in the book talks about object-oriented languages and how they are the most fitting for the domain model. That's because at the time of writing this book, the usage of object oriented languages was on the rise and that's why it was considered to be the best fit.

However, currently many paradigms are being adopted and the book describes how to mix paradigms as well. That's why instead of talking about object-oriented paradigm I will try to generalize the rules of mixing the paradigms because I think it's more relevant for the current trends in software engineering.

Note that domain-driven design highly relies on OOP and most of the techniques are specific for OOP but I will try to generalize the concepts when possible. But that is my personal opinion and it's important to know that as per the book, Object-oriented languages are the best fit for applying Domain-Driven Design.

#### **Rules for Mixing Paradigms**

1. Don't fight the dominating paradigm: first try to model the domain object in the paradigm you're already using. You can often rethink the model and look at it from another perspective so try to do that before using a new paradigm.
2. Lean on the ubiquitous language: If you're using a new paradigm; stick to the ubiquitous language. That way the model can still be reasoned about.
3. Don't be limited by the modeling tools: if you're using a modeling tool like UML don't distort the model into what's easier to represent with the tool. Use tools that fit the paradigm.
4. Be skeptical: Is the paradigm you're introducing really worth its weight or can the object be represented with the current paradigm? Sometimes objects are not obvious to model but they can often be modeled to different paradigms.

## **Reference**
1. Domain-Driven Design by Eric Evans [Part I](https://dev.to/ielgohary/domain-driven-design-by-eric-evans-part-i-5d8m)
2. Domain-Driven Design by Eric Evans [Part II](https://dev.to/ielgohary/layered-architecture-and-smart-ui-domain-driven-design-by-eric-evans-part-ii-chapter-4-1il7)
3. Domain-Driven Design by Eric Evans [Part III](https://dev.to/ielgohary/domain-driven-design-entities-value-objects-and-services-chapter-51-22cm)
4. Domain-Driven Design by Eric Evans [Part IV](https://dev.to/ielgohary/modules-in-ddd-9b7)
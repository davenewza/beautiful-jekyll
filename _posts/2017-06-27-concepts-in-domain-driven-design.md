---
layout: post-draft
title: Concepts in Domain-driven Design
tags: [domain-driven-design]
---

## Ubiquitous Language

_A language structured in and around the domain model and used by all team members to connect all the activities of the team with the software._

A **rich domain model** should fluently describe the domain problem, requirements and intentions.

It is important that the domain model is not dependent on any infrastructural concerns.  The domain model should be ignorant as to how data is stored, where systems are hosted, and anything else which is not applicable to the domain problem and that which would influence the design of such a model. Business invariants become lost and unacknowledged as infrastructural code begins to dominate the codebase.

Through a rich design, the domain model can effectively describe the invariants of the domain.  It is the core of a common language for a software project.  This vastly improves communication between team members as the system is now described in code, and not in filing cabinets, emails, requirements documents and, worst of all, in our memory.

## Entities

_An entity is a real thing in our domain which is not defined by its properties but rather by its identity._

 * Only create something as an entity if it is uniquely identifiable.  Litmus test:  If two instances of the same object have different attribute values, but same identity value, are they the same entity?
 * Do perform invariant validation so that entities can enforce their own consistency.
 * Do not use property bags as they are promiscuous and will create gaping holes in the domain.
 * Do not design entities from database tables.  Data storage is constrained by technology, which we do not want reflected in our domain model.

## Value Objects

*Value objects describe something in the domain which is identified by the value of its properties and not by some identity attribute.*

 * ​An object that represents a descriptive aspect of the domain with no conceptual identity is called a value object.
 * Two value objects with the same property values are equal.
 * A value object cannot live on its own without an entity.  You don't need a repository or a data mapper for a value object.  The repository or mapper for a given entity will take care of all related value objects.
 * Do create value objects as immutable.​

## Aggregates

_An aggregate is a cluster of associated objects that we treat as a unit for the purpose of data consistency, integrity and invariant preservation._

 * The aggregate draws consistency boundaries around related entities and value objects.
 * All external access to the aggregate is done through a single root entity – the aggregate root.
 * Referencing an aggregate root from an external entity should be done by identity.  This prevents complicated deep object graphs.  These referencing entities are not part of the same transactional unit.
 * External entities can hold references to any aggregate root, but never to any other entity or value object within the aggregate. To access any other part of the aggregate, you must navigate from the aggregate root.
 * An aggregate is persisted by a repository.

**Vaughn Vernon's Effective Aggregate Design series:**

 * [Part I: Modeling a Single Aggregate](http://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_1.pdf)
 * [Part II: Making Aggregates Work Together](http://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)
 * [Part III: Gaining Insight Through Discovery](http://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_3.pdf)

## Repositories

_A repository mediates between the domain and data mapping layers using a collection-like interface for retrieving, adding and updating aggregates._

 * One repository serves only one aggregate, and one aggregate is served by only one repository.
 * Repository interfaces are defined in the domain model and so should have domain-like methods, e.g.  GetUsersByLastName().
 * The implementation of a repository is not relevant to the domain.  They could use some SQL database, file storage, NoSQL, public APIs, memory, etc.

## Domain Services

_A domain service is used when a process or transformation in the domain is not a natural responsibility of an entity or value object._

 * Do model entities with the logic that relates to them and their children. But, when domain logic spans across multiple aggregates or needs to deal with external responsibilities, use a service.  Domain services contain domain logic that can't naturally be placed in an entity or value object.
 * Don't overuse domain services.  Stripping out entity and value object behaviour into a service will lead to anaemic domain models. 
 * Domain services are different from infrastructural services because they embed and operate upon domain concepts and are part of the ubiquitous language.

## Domain Events

_Domain events are used to capture an occurrence of something that happened in the domain._

 * Events exists and are raised in the domain.  For example, an email infrastructure service can handle a domain event by generating and transmitting an appropriate email message.
 * The domain layer doesn't care about the specifics or how an event notification is delivered; it only cares about raising the event.

## Infrastructural Services

 * Domain services are different from infrastructural services because they embed and operate upon domain concepts and are part of the ubiquitous language.  Infrastructural services are instead focused encapsulating the "plumbing" requirements of an application; usually concerns such as file system access, database access, email, etc.
 * Repositories are infrastructural services.​

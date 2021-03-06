# Domain Driven Design Distilled - Reading Notes

## Chapter 1 - DDD for Me

### Why use DDD?

DDD can be an advanced technique, but it can help you with tackling complexity in software.

### Skipping Design

There's no such thing as skipping design to speed up a project - design is always there. It's either good or bad. Intentional or accidental. We should aim for _effective_ design.

### Common Problems

There are many common problems that DDD can help businesses avoid. Too much focus on DB, too much focus on technology, bad language, etc.

### Types of Design

There are two types of design:

- Strategic design: Broad brushstrokes. Highlight what is strategically important to your business, how to divide the work up, and how to integrate.
- Tactical design: Using a thin brush to paint the fine details of your domain model.

### How does DDD work?

DDD is primarily discovery (building a shared mental model) with Domain Experts and developers through group conversation and experimentation.

At a high level, with DDD, you:

- Use techniques like _Event Storming_ to build a shared mental model.
- Segregate domain models using _Bounded Contexts_. Challenge and unify.
- Develop a _Ubiquitous Language_ within each _Bounded Context_ by engaging _Domain Experts_.
- Use _Subdomains_ to deal with the unbounded complexity of legacy systems.
- Integrate multiple _Bounded Contexts_ via _Context Mapping_.
- Group entities and value objects into _Aggregates_.
- Use _Domain Events_ to notify _Bounded Contexts_ of important happenings within your domain model.

## Chapter 2 - Strategic Design with Bounded Contexts and the Ubiquitous Language

### What is a Bounded Context?

It's a semantic contextual boundary. It has its own _Ubiquitous Language_, even though other _Bounded Contexts_ may share some of the same words.

For example, the term "flight" has different meanings in different contexts.

### Problem Space vs Solution Space

- Bounded Contexts start off conceptual in the problem space, but transition to the solution space as code gets written.
- Problem Space: High-level strategic analysis.
- Solution Space: Where the solution is actually implemented for your _Core Domain_.

### Core Domain

This is the _Bounded Context_ that is a key strategic initiative for your organization. You can't excel at everything, so focus on your _Core Domain_.

### Teams and Source Code

- There should be only one team assigned to work on a _Bounded Context_.
- There should be a separate code repository for each _Bounded Context_.
- Cleanly separate the source code and database schema.
- Acceptance tests and unit tests should be kept with the main source code.

### Challenge and Unify

- _Big Balls of Mud_ occur when a monolith is built as a single unbounded model.
- Challenge and unify - remove models that are Out of Context. Then join them into their own Bounded Contexts. "What is core?"

### Develop a Ubiquitous Language

- Use _Event Storming_
- Write scenarios
- Use Behavior-Driven Development to write Acceptance Tests

### Architecture

- The domain model should be free of technology.
- Common components:
  - Input Adapters (UI, REST endpoints, message listeners)
  - Application Services (domain model, transaction management)
  - Output Adapters (persistence, message senders)
- Microservices map very nicely to _Bounded Contexts_.

## Chapter 3 - Strategic Design with Subdomains

### Subdomains

- Every project has _Bounded Contexts_. One is the _Core Domain_.
- A _Subdomain_ is a sub-part of your overall business domain. A clear area of expertise. Usually at least one _Domain Expert_.
- If designed with DDD, each Bounded Context is also a _Subdomain_. Ideally 1-to-1 relationship.
- It is possible to have multiple _Subdomains_ in one _Bounded Context_, but this is not ideal.

### Subdomain Types

- _Core Domain_: Your company's competitive edge and highest priority.
- _Supporting Subdomain_: Custom development because off-the-shelf solution doesn't exist. Outsource if possible. Still important since it supports
- _Generic Subdomain_: Like supporting, but off-the-shelf solution likely exists.

### Big Balls of Mud

- These are often unbounded domains.
- However, many logical domain models may exist inside that legacy system.
- You can think of each of these logical domain models as a _Subdomain_.
- You can therefore treat the problem space as if it had been developed using DDD with _Bounded Contexts_. (Even though the solution space may be legacy code you're not going to modify.)

### One-to-one with Bounded Contexts

- You can draw _Subdomains_ as box with dashed lines and a text label.
- If you must create a second _Subdomain_ model in the same _Bounded Context_, you should segregate it from your _Core Domain_ using a separate _Module_. The segregation would happen in the solution space.

## Chapter 4 - Strategic Design with Context Mapping

### Context Mapping

- _Context Mapping_ is the integration between two _Bounded Contexts_.
- It can be presented as a line connecting two _Bounded Contexts_.
- It serves as a translation between two layers.

### Types of Context Mappings

- _Partnership_: Drawn with a thick line. Represents both teams failing or succeeding together. Close synchronization -- high commitment.
- _Shared Kernel_: Drawn by having two _Bounded Contexts_ overlap. Literal sharing of a common model.
- _Customer-Supplier_: Drawn with a line, with U on the upstream context and a D on the downstream context. The Supplier is in charge and determines what the Customer gets. Customer needs to communicate its needs to the Supplier.
- _Conformist_: Drawn same as Customer-Supplier. When the upstream team has no motivation to support downstream, so downstream has to conform to upstream.
- _Anticorruption layer_: Drawn as above, but with an "ACL" box attached to the downstream node. The most defensive _Context Mapping_ relationship, where the downstream team creates a translation between its Ubiquitous Language and that of the upstream model.
  - Whenever possible, you should try to create an _Anticorruption Layer_ so you can isolate yourself away from foreign concepts.
- _Open Host Service_: Similar to above, but with OHS attached to the upstream node instead. "Open" means it's easy to integrate with. Defines a protocol or interface. The language is easier to consume.
- _Published Language_: Similar to above, but with PL attached to upstream node. A well-documented information exchange language, like XML, JSON, and so on. _Open Host Services_ will often serve and consume a _Published Language_.
- _Separate Ways_: Sometimes it's not worth it to integrate to another _Bounded Context_. A specialized solution within the current context may be best.
- _Big Ball of Mud_: Don't do this. If you have to integrate to one, do so with an _Anticorruption Layer_ between. "Don't speak their language!"

### Making Good Use of Context Mapping

Avoid integration with database or file system directly when possible. If you must, use an _Anticorruption Layer_.

There are three most trustworthy integration types. From least robust to most robust:

- _RPC with SOAP_: Basically a remote function call. Implies tight coupling. Possible to have complete network failure or latency. Lacks robustness- failures are absolutes. \* If you must use it, have the service be an _Open Host Service_ using a _Published Language_ and design the client context to have an _Anticorruption Layer_.
- _RESTful HTTP_: Like above, should use an OHS / PL with an ACL. Can fail due to the network as well, but it's expected as it's Internet based.
  _ Common mistake: Having resources in the API directly reflect *Aggregates* of the domain model. This forces a *Conformist* relationship, where if the model changes shape, then the resources in the API do too.
  _ Instead, design resources "synthetically" to follow client-driven use cases. What the client needs _drives the design of the REST resources_, not the domain models' current composition.
- _Messaging_: _Domain Events_ being sent to and from _Bounded Contexts_. Most robust since it removes temporal coupling. Latency is anticipated. \* Messaging system should support At-Least-Once-Delivery, and subscribing _Bounded Contexts_ should be implemented as _Idempotent Receivers_.

# Chapter 5 - Tactical Design with Aggregates

- What's inside a _Bounded Context_? _Aggregates_, _Entities_, and _Value Objects_.

## Entities and Value Objects

- An _Entity_ is a unique thing. It's most often mutable, but can be immutable. It has individuality and uniqueness.
- A _Value Object_ (or just _Value_) is an immutable conceptual whole. It does not have an identity. Equivalence just uses its values.
  - It's not a thing. It is often used to describe, quantify, or measure an _Entity_.

## Aggregates

- An _Aggregate_ is composed of one or more _Entities_.
  - One _Entity_ is the _Aggregate Root_.
  - _Aggregates_ may also have _Value Objects_.

## Aggregate Roots

- The _Root Entity_ owns all other elements clustered inside it. The name should be the _Aggregate_'s conceptual name.

## Transactions

- _Aggregates_ form a transactional consistency boundary for business rules.
- Only update one Aggregate per transaction.

## Aggregate Rules of Thumb

1. Protect business invariants inside _Aggregate_ boundaries.
2. Design small _Aggregates_. (Think Single Responsibility Principle)
3. Reference other _Aggregates_ by identity only.
4. Update other _Aggregates_ using eventual consistency. (_Domain Events_)

## Modeling Aggregates

- Avoid _Anemic Domain Models_. These are Aggregates that only have getters and setters but no business behavior.
  - This is a sign that the focus was too technical with not enough business insight during modeling.
- Keep your business logic in your domain model. Don't let it leak into the Application Services above it.
- There are some special exceptions when it comes to Functional Programming. This book addresses DDD from an OO perspective.

## Technical Components of Aggregates

- Create your _Aggregate Root Entity_ -- most likely with a base class like _Entity_.
- Ensure that your _Aggregate Root Entity_ has a globally unique identity. (This could be a compound key -- not just one.)
- Capture attributes and fields which are needed to find the _Aggregate_.
- Avoid public setters -- less likely to have an _Anemic Domain Model_ that way.
- Create functions for complex behavior. Name them using the Ubiquitous Language.
- Use what you've modeled -- Aggregate should have been drawn out already. Be in harmony with _Domain Experts_.

## Choose Abstractions Carefully

- High level concepts that line up with _Domain Experts_. Scrum = good. ScrumElementContainer = bad.
- Make sure the language of the software model matches the mental model of the _Domain Experts_.
- Making abstraction level too high will lead to too many special cases and complexity with too much code and time waste.
- You can't address all future needs anyway.

## Right-Sizing Aggregates

- Want to prevent the design of large clusters.

### Recommended Design Steps

1. Design small _Aggregates_. Start with just one _Entity_ in each. Populate each _Entity_ with fields that are required to identify and find the _Aggregate_.
2. Protect business invariants inside _Aggregate_ boundaries. For each _Aggregate_, ask _Domain Experts_ if other _Aggregates_ must be updated in reaction to changes made to the current one. List these out. The heading is the current aggregate, and items beneath it are the aggregates that would be updated in reaction.
3. Ask _Domain Experts_ how much time can pass for each reaction update. It'll either be immediately or within N seconds/minutes/hours/days.
4. If immediate timeframe, the two _Entities_ should probably be in the same _Aggregate_ boundary. Join these Aggregates. (Be careful not to make every case the immediate case. Does the business really need immediate consistency?)
5. For non-immediate timeframes, update _Aggregates_ using eventual consistency.

## Testable Units

- _Aggregates_ should be designed to be encapsulated well for unit testing.
  - Remember that this is separate from validating business specifications via acceptance tests.
- The unit tests will be associated with the _Bounded Context_ and kept with its source code.

# Chapter 6 - Tactical Design with Domain Events

## Domain Events

- _Domain Events_ are records of business-significant occurrences within a _Bounded Context_.
- They are often modeled as part of the _Core Domain_ during tactical design.
- They are used for eventual consistency. This can include causal consistency.

## Designing, Implementing, and Using Domain Events

### Creating Domain Events

- Every _Domain Event_ should have a date and time when it occurred. Use a base class for this.
- Name _Domain Events_ using _Ubiquitous Language_. Use the past tense.
  - Examples: "ProductCreated", "ReleaseScheduled", "BacklogItemCommitted"
- To determine properties of the _Domain Event_:
  - Deduce which command would publish the _Domain Event_
  - Determine the properties the command would need to create the _Domain Event_
  - Have the _Domain Event_ include those too at the least.
    _ Sometimes, enrich the *Domain Event* with additional data so query-backs aren't needed.
    _ But don't do it so much that the _Domain Event_ loses its meaning.

### Using Domain Events

- Persist the _Domain Event_ in an event store, then publish it.
- Consumers of _Domain Events_ need to recognize proper causality. (Handling repeats, out of order, etc.)

### Commands and Domain Events

- Some _Domain Events_ are not caused by commands, but rather by things like timers hitting.
- Difference between commands and _Domain Events_: Commands can be rejected, but _Domain Events_ are a part of history and always happen.

## Event Sourcing

- _Event Sourcing_ is persisting all _Domain Events_ that occurred for a specific _Aggregate_ instance instead of persisting the _Aggregate_ state as a whole.
- Performant, since the event store is append-only.
- Using the Actor model is an easy way to keep _Aggregate_ state cached.
- Snapshots can be made of the _Aggregate_ so its state doesn't always have to be reconstituted from _Domain Events_.
- _Event Sourcing_ is good for keeping a full history and for debugging.

# Chapter 7 - Acceleration and Management Tools

## Event Storming

- _Event Storming_ is a rapid design technique that engages _Domain Experts_ and developers.
- It is focused on the business and business process -- not on nouns and data.
- Very cheap to perform - a matter of hours rather than weeks. (Two hours chunks with breaks for 3-4 sessions.)
- Can be performed for both big-picture and design-level modeling.

### Materials

- Sticky notes: Orange, Purple/Red, Light Blue, Pale Yellow, Lilac, and Pink. Green may be useful as well. Extra sticky variety. Square shapes OK.
- One black marker pen per person, fine-tip.
- A wide wall - at least one meter tall, but width should be unlimited. 10 meters at least.
- Long roll of paper to match the dimensions described above. Use strong tape to attach to wall.

### Steps

1. Storm out the business process by putting _Domain Events_ on orange sticky notes in time order.
   - Focus on the business process.
   - Just write the name. A verb in past tense. "ProductCreated", "BacklogItemCommitted"
   - If events happen in parallel, use vertical space and put them in the same column.
   - If you find a trouble spot, mark it with purple or red sticky note with text saying why.
   - If a _Domain Event_ is a _Process_ that needs to run, name it on a lilac sticky note. \* Draw a line from the _Domain Event_ to the _Process_.
   - If you think you're done, take a break and continue another day.
2. Create the _Commands_ that cause each _Domain Event_ on light blue sticky notes. Just write the name.
   - An imperative, like "CreateProduct" or "CommitBacklogItem".
   - Put them on the left of the _Domain Event_ they create.
   - If a specific user role performs the action and it's important, use a small bright yellow sticky note to the lower left of the _Command_ with a stick figure and the name of the role. \* Example: Product Owner
   - If a _Command_ starts a _Process_, name it on a lilac sticky note.
     _ Draw a line from the *Command* to the *Process*.
     _ The _Process_ will cause one or more _Commands_ and subsequent _Domain Events_. Create them now if you know them and show them emitting from the _Process_.
   - Go in time order, left to right.
   - You may discover new _Domain Events_. Create them as you go.
   - It's OK for a _Command_ to cause multiple _Domain Events_. Just put it to the left of them.
3. Associate _Entities/Aggregates_ with the _Command_ that is executed and the _Domain Event_ that occurs. Do this on pale yellow stickies.
   - These are nouns, like "Product" or "BacklogItem".
   - Place them behind, above, and overlapped by both the _Command_ and the _Domain Event_.
   - If the same _Aggregate_ is used in multiple places, create multiple copies. Don't re-arrange what is there already. \* The point is to model the business process, which happens over time.
   - You may discover new _Domain Events_. Create them and their associated _Commands_ as you go.
   - If you discover _Aggregates_ that are too complex and need to be broken into managed _Processes_, mark them with lilac stickies.
4. Draw boundaries and lines with arrows to show the flow.
   - Use solid lines for _Bounded Contexts_.
   - Use dashed lines for _Subdomains_.
   - Use pink stickies to name your _Bounded Contexts_.
   - Draw lines with arrowheads to show direction of _Domain Events_ flowing between _Bounded Contexts_.
5. Identify the various views that your users will need to carry out their actions and any important roles.

   - Green sticky notes for the views. You can draw a quick mockup too.
   - Use bright yellow sticky notes to represent user roles.

## Other Tools

- Write _Acceptance tests_ in a given/when/then approach.
- _Impact Mapping _ to make sure the software you are designing is a _Core Domain_.
- Look into _User Story Mapping_, which can be used to focus on your _Core Domain_.

## Managing DDD on an Agile Project

- Focus on _knowledge acquisition_.
- Hire good people.
- Use _SWOT Analysis_ - Strengths, Weaknesses, Opportunities, and Threats.
- Use _Event Storming_.

### How to Size

- Use the components discovered from _Event Storming_ to do time estimates.
- Recommended table structure:

| Component Type | Easy (Hours) | Moderate (Hours) | Complex (Hours) |
| -------------- | ------------ | ---------------- | --------------- |
| Domain Event   | 0.1          | 0.2              | 0.3             |
| Command        | 0.1          | 0.2              | 0.3             |
| Aggregate      | 1            | 2                | 4               |

- You can also create rows for user interface components, services, persistence, and so on.
- Add up all the esimation units for components in the current sprint.
- You can expect around 20% accuracy.

### How to Implement

1. Do a quick _Event Storming_ session.
2. Talk to a _Domain Expert_ to refine concrete scenarios.
3. Create acceptance tests to exercise each scenario.
4. Create components to allow tests and specifications to execute. Iterate quickly as you refine.
5. Create additional tests and scenarios as they are discovered.

### Interacting with Domain Experts

- Always include them in _Event Storming_.
- They are needed for the creation of model scenarios.
- They are needed to review tests to verify model correctness.
- They are needed to refine the _Ubiquitous Language_, _Aggregate_ names, _Commands_, _Domain Events_, and so on.
- Limit _Event Storming_ sessions to 2-3 hours each. Maybe do three to four consecutive days.
- Each scenario should be discussed in 10-20 minutes of time.
- Have them spend some time reviewing your tests.

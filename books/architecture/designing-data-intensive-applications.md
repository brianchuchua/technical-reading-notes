# Designing Data-Intensive Applications

## Chapter 1 - Reliable, Scalable, and Maintainable Applications

- Many applications are _data-intensive_ as opposed to _compute-intensive_.
- _Data systems_ often use similar pre-built tools, but the challenge is with selecting which tools and how to combine them.
- Data systems require:
  - Reliability: Work correctly in the face of adversity.
  - Scalability: Have reasonable ways of dealing with growth.
  - Maintainability: Maintainers should be able to maintain and adapt to new use cases productively.

### Reliability

- An application should be _fault-tolerant_/_resilient_, capable of working correctly even when things go wrong.
- _Faults_ differ from _failures_. _Failure_ is the system stopping. _Fault_ is a component deviating from its spec.
- Triggering faults intentionally can be very beneficial for testing robustness. See Netflix's Chaos Monkey.
- We generally prefer tolerating faults over preventing them, but sometimes that is not feasible. (Security.)
- Reliability is important even for non-critical systems -- losing family photos in an app is given as an example.

#### Hardware Faults

- Tend to be random, independent events.
- Can add physical redundancy.
- Software fault-tolerance is now preferred in the cloud -- multiple nodes with no downtimes during reboots or upgrades.

#### Software Errors

- Tend to be correlated or chain together.
- Can be systemic.
- Can lie dormant for a long time.
- Can be mitigated with good abstractions and thorough testing.

#### Human Errors

- Configuration errors are the leading cause of outages.
  - Good admin interfaces and APIs can mitigate this risk.
- Sandbox environments can separate the place where people make mistakes from the place that leads to failures.
- Thorough testing, from unit to integration tests, can also help.

### Scalability

- Load can be described using _load parameters_. What these are depends on the application.
- Twitter is used as an example. They receive 300k requests/sec for home timeline views, and up to 12k/sec tweets.
  - They used to just use a tweets table and insert into it.
  - But then switched to pushing tweets to every follower's cache for the timeline.
  - But now use a hybrid approach due to the massive fan-out of celebrity accounts.

#### Describing Performance

- Two approaches:
  1. When increasing a load parameter but keeping resources fixed, how is performance affected?
  2. When increasing a load parameter, how much do you need to increase resources to keep performance unaffected?
- Response time tends to be the most important for online applications.
- Measuring the median across multiple percentiles (50th, 95th, 99th, 99.9th) is most common.
  - High percentages are also known as _tail latencies_.
  - Some companies guarantee these with SLOs and SLAs.
  - Queueing delays are most common the cause of the worst-case scenarios.
  - When making many backend calls, the slowest one will bog down the entire operation.

#### Approaches for Coping with Load

- _Scaling-up_ vs _scaling-out_, vertical vs horizontal. Horizontal is also known as _shared-nothing_ architecture.
  - Vertical is often simpler but expensive. Hybrid approaches are best.
  - Some systems are _elastic_, others needs to be scaled manually.
- Databases tend to prefer to scale up.

### Maintainability

- It's good to avoid building a "legacy" system.
- We need operability, simplicity, and evolvability.

#### Operability

- Access to good metrics.
- A good DevOps team.
- Great documentation.
- Effective strategies for rollbacks and disaster recovery.

#### Simplicity

- Avoid a big ball of mud.
- _Accidental complexity_ is the thing to avoid -- complexity that has nothing to do with the problem that is being solved.
- Clean abstractions are key.

#### Evolvability

- Systems need to change all the time.
- Agile methodologies help, including:
  - TDD.
  - Refactoring.
- This is very linked to its simplicity and abstractions.

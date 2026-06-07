# Queueing theory

Queueing theory is the mathematical study of waiting lines, or queues. We use
queueing theory in our software development, for purposes such as analyzing and
optimizing our practices and processes, such as our customer service
responsiveness, project management kanban planning, inter-process communication
message queues, and devops continuous deployment pipelines.

Contents:

- [Introduction](#introduction)
  - [Customer service responsiveness](#customer-service-responsiveness)
  - [Project management kanban planning](#project-management-kanban-planning)
  - [Inter-process communication message queues](#inter-process-communication-message-queues)
  - [Devops continuous deployment pipelines](#devops-continuous-deployment-pipelines)
- [Queue terminology](#queue-terminology)
  - [Queue types and service types](#queue-types-and-service-types)
- [Queueing theory notation](#queueing-theory-notation)
  - [Count](#count)
  - [Arrival rate, service rate](#arrival-rate-service-rate)
  - [Utilization ratio a.k.a. traffic intensity](#utilization-ratio-aka-traffic-intensity)
  - [Throughput rates: total rate, success rate, failure rate, skip rate](#throughput-rates-total-rate-success-rate-failure-rate-skip-rate)
  - [Service rate disambiguation](#service-rate-disambiguation)
  - [Skip rate](#skip-rate)
  - [Error ratio](#error-ratio)
  - [Lead time, step time, work time, wait time](#lead-time-step-time-work-time-wait-time)
  - [Standard notation](#standard-notation)
- [Activity tracking](#activity-tracking)
  - [Activity examples](#activity-examples)
  - [Little's Law](#littles-law)
  - [Key performance indicators (KPIs)](#key-performance-indicators-kpis)
- [Service metrics](#service-metrics)
  - [Mean Time To Respond, Repair, Recover, Resolve](#mean-time-to-respond-repair-recover-resolve)
  - [DORA metrics](#dora-metrics)
- [Queue of queues](#queue-of-queues)
  - [Double Diamond queue of queues](#double-diamond-queue-of-queues)
  - [Funnel queue of queues](#funnel-queue-of-queues)
- [Insights](#insights)
- [Epilog](#epilog)
  - [See also](#see-also)
  - [Thanks](#thanks)

## Introduction

We use queueing theory in our projects for many purposes:

- Customer service responsiveness

- Project management kanban planning

- Inter-process communication message queues

- Devops continuous deployment pipelines

### Customer service responsiveness

For example, we want to analyze how customers request sales help and support help, and how fast we respond.

Some relevant products are e.g. [Salesforce](https://wikipedia.org/wiki/Salesforce), [LiveChat](https://wikipedia.org/wiki/LiveChat), [Zendesk](https://wikipedia.org/wiki/Zendesk).

### Project management kanban planning

For example, we want to track the lead times and progress times as a new feature idea evolves from design to delivery.

Some relevant products are e.g. [Asana](<https://wikipedia.org/wiki/Asana_(software)>), [Jira](<https://wikipedia.org/wiki/Jira_(software)>), [Microsoft Project](https://wikipedia.org/wiki/Microsoft_Project).

### Inter-process communication message queues

For example, we want to maximize throughputs and minimize pressures as one program sends requests to another program.

Some relevant products are e.g. [RabbitMQ](https://wikipedia.org/wiki/RabbitMQ), [ActiveMQ](https://wikipedia.org/wiki/Apache_ActiveMQ), [ZeroMQ](https://wikipedia.org/wiki/ZeroMQ).

### Devops continuous deployment pipelines

For example, we want to ensure our continuous integration server has capacity to test our software then deploy it.

Some relevant products are e.g. [Jenkins](<https://wikipedia.org/wiki/Jenkins_(software)>), [Bamboo](<https://wikipedia.org/wiki/Bamboo_(software)>), [Azure DevOps](https://wikipedia.org/wiki/Microsoft_Visual_Studio#Azure_DevOps).

## Queue terminology

Queue terminology is a big topic. This section has some of our common terminology. For the examples, we will use the idea of a customer waiting in line.

### Queue types and service types

Queue types and service types describe how the queue chooses which items to process.

- First In First Out (FIFO): serve the customer who has been waiting for the longest time.

- Last In First Out (LIFO): serve the customer who has been waiting for the shortest time.

- Priority: serve customers based on their priority level; these levels could be based on status, urgency, payment, etc.

- Shortest Job First (SJF): serve the customer who needs the smallest amount of service.

- Longest Job First (LJF): serve the customer who needs the largest amount of service.

- Time Sharing: serve everyone at the same time; service capacity is distributed evenly among everyone waiting.

## Queueing theory notation

Queueing theory uses notation with Greek letters.

Our teams use some of the popular notation; we also add some custom notion that help us with software projects.

### Count

We count items often, and we use this notation:

- κ (kappa): count

Example:

- κ = 100 means there are 100 items in the queue.

- κ > 100 means there are more than 100 items in the queue.

- κ ≫ 100 means there are many more than 100 items in the queue.

### Arrival rate, service rate

The most important notation:

- λ (lambda): arrival rate. This measures how fast new items are coming into the queue.

- μ (mu): service rate. This measures how fast items in the queue are being handled.

Examples of generalities for traditional queues:

- λ = μ means the arrival rate equals the service rate; the queue is staying the same size.

- λ > μ means the arrival rate is greater than the service rate; the queue is getting larger.

- λ < μ means the arrival rate is less than the service rate; the queue is getting smaller.

Be aware that some queueing theory practitioners define service rate differently,
and that service rate may be different due to failures such as errors or skips such as dropouts.

### Utilization ratio a.k.a. traffic intensity

The most important notation that summarizes a queue:

- ρ (rho): utilization ratio = λ / μ

Examples:

- ρ = 1 means the arrival rate is equal to the service rate; the queue is staying the same size.

- ρ > 1 means the arrival rate is greater than the service rate; the queue is getting larger.

- ρ < 1 means the arrival rate is less than the service rate; the queue is getting smaller.

### Throughput rates: total rate, success rate, failure rate, skip rate

Throughput rates come in four broad categories:

- χ (chi): total rate. This measures how many items per time exit the queue for any reason: success, failure, skip, etc.

- α (alpha): success rate. This measures how many items per time period turn out right e.g. correct, complete, accepted.

- β (beta): failure rate. This measures how many items per time period turn out wrong e.g. incorrect, incomplete, rejected.

- σ (sigma): skip rate. This measures how many items per time person skip the queue e.g. dropouts, abandonments, losses.

For typical queueing theory, total rate = service rate + error rate + skip rate:

- χ = α + β + σ (pronounced: chi equals alpha plus beta plus sigma)

### Service rate disambiguation

Depending on context and practitioner, the terminology "service rate" can mean either
"total rate" or "success rate".

This has historical roots and can lead to confusion and mistakes, epsecially for
context and practitioners using queueing theory where there's a significant need
to track failures and skips.

- Theoretical View: The service rate is a pure measurement of speed (e.g., jobs per second), regardless of whether the jobs are successes, failures, skips, etc.

- Processing View: The service rate includes whatever the queue spends time processing, meaning successes or failures, but does not include skips.

- Technology View: In applied fields such as software engineering, the service rate measures only successes, whereas the error rate measures failures, and the skip rate measures dropouts.

Therefore we prefer change from this terminology:

- μ (mu): service rate. This measures how fast items in the queue are being handled.

To this terminology:

- χ (chi): total rate. This measures how many items per time exit the queue for any reason: success, failure, skip, etc.

- α (alpha): success rate. This measures how many items per time period turn out right e.g. correct, complete, accepted.

### Skip rate

Skip rate details:

- σ (sigma): skip rate. This measures how many items skip out the queue unhandled a.k.a. dropouts.

A skip is when an item leaves the queue without any significant processing:

- Abandoning: when a customer starts a queue, then leaves, for any reason.

- Balking: when a customer decides not to start waiting for service because the wait time threatens to be too long.

- Reneging: when a customer who has waited already decides to leave because they’ve wasted too much time.

- Jockeying: when a customer switches between queues in a tandem queue system, trying to get a shorter wait.

### Error ratio

The most important notation that summarizes a queue's success:

- ε (epsilon): error ratio = service failure count / service total count

Examples:

- ε = 0 means no errors.

- ε = 0.1 means 10% of services have an error.

- ε = 1 means every service has an error.

### Lead time, step time, work time, wait time

We track four times:

- τ (tau): lead time = from start of queue to finish of queue

- θ (theta): step time = from one finish to the next finish

- φ (phi): work time = time spent doing actual work a.k.a. processing time

- ω (omega): wait time = time spent not doing actual work a.k.a. pending time

Examples:

- τ = 5s means an item is added to the queue, then serviced 5 seconds later.

- ω = 4s means an item waits in the queue for 4 seconds, then work starts.

- φ = 1s means an item takes 1 second of work, then is complete.

- θ = 1s means there's 1 second between one completion and the next completion.

### Standard notation

Standard notation for queueing theory also uses these symbols:

- n: the number of items in the system.

- A: the arrival process probability distribution.

- B: the service process probability distribution.

- C: the number of servers.

- D: the maximum number of items allowed in the queue at any given time, waiting or being served (without getting bumped).

- E: the maximum number of items total.

## Activity tracking

### Activity examples

Suppose we have something we want to track, and we call it something generic such as "Activity" and abbreviated as "A".

We can efficiently use queuing notation to describe the activity and how it moves through a queue.

Examples:

- Aκ: Activity count: how many items are in the queue.

- Aλ: Activity arrival rate: how many items are incoming per time unit.

- Aχ: Activity total rate: how many items out of the queue per time unit.

- Aμ: Activity service rate a.k.a. success rate: how many items are right per time unit.

- Aβ: Activity error rate a.k.a. failure rate: how many items are wrong per time unit.

- Aσ: Activity skip rate a.k.a. dropout rate: how many items are abandoned per time unit.

- Aρ: Activity utilization ratio: how many items are arriving vs. completing.

- Aε: Activity error ratio: how many items are completed with errors vs. total.

- Aτ: Activity lead time: how much time elapses from requested to completed.

- Aω: Activity wait time: how much time elapses from requested to started.

- Aφ: Activity work time: how much time elapses from started to completed.

- Aθ: Activity step time: how much time elapses from completed to next completed.

### Little's Law

Little's law is a theorem by John Little which states: the long-term average number L of customers in a stationary system is equal to the long-term average effective arrival rate λ multiplied by the average time W that a customer spends in the system.

Example notation:

- L: long-term average number of customers in the system. We prefer κ (kappa).

- λ: long-term average effective arrival rate.

- W: long-term average time an item is in the system. We prefer τ (tau).

- L = λ W is Little's law. We prefer κ = λ τ (kappa = lamba \* tau).

Little's law assumptions:

- All measurement units are consistent.

- Conservation of flow, meaning the average arrival rate equals the average departure rate.

- All work that enters the system then flows through to completion.

- The system is “stable”, meaning the average age of items are neither increasing or decreasing, and the total number of items is roughly the same at the beginning and at the end.

### Key performance indicators (KPIs)

We typically track many things about the activities in the queue, and we want to summarize the results by choosing a shortlist of the most relevant ones for our projects.

We have built many projects, and we believe the most valuable summary indicators are:

- Dτ = Delivery lead time. Product teams may say "from concept to customer" or "from idea to implementation".

- Dμ = Delivery service rate. Devops teams may say "deployment frequency" or "we ship X times per day".

- Dε = Delivery error ratio. Quality teams may say "change fail rate" or "percentage of rollbacks".

- Rτ = Restore lead time. Site reliability engineers may say "time to restore service" or "mean time to restore (MTTR)".

## Service metrics

Useful terms:

- **Service Level Indicator (SLI):** is the metric (e.g., 99.9% uptime)
- **Service Level Objective (SLO):** the internal target for that metric (e.g., aim for 99.95% uptime)
- **Service Level Agreement (SLA):** the formal, contractual promise to the customer (e.g., 99.9% uptime with penalties if missed).

### Mean Time To Respond, Repair, Recover, Resolve

MTTR represents four different measurements:

| Metric                   | Focus               | Measures                             | Start                              | Finish                            |
| ------------------------ | ------------------- | ------------------------------------ | ---------------------------------- | --------------------------------- |
| **Mean Time To Respond** | Initial reaction    | Time to acknowledge and begin action | When incident is detected/reported | When team starts working on it    |
| **Mean Time To Repair**  | Technical fix       | Time to fix the broken item          | When repair work begins            | When item is fixed                |
| **Mean Time To Recover** | Service restoration | Time to restore full service         | When incident/outage starts        | When service is fully operational |
| **Mean Time To Resolve** | Complete resolution | Total time from detection to closure | When incident is detected          | When incident is fully closed     |

**Key distinctions:**

**Respond** measures team readiness and alerting effectiveness. A low response time means your monitoring and on-call processes are working well, even if the actual fix takes longer.

**Repair** is narrowly technical—how long does it take to actually fix the broken thing once you're working on it? It excludes detection time, triage, and verification.

**Recover** is about service availability from the customer's perspective. The system might be "repaired" but not yet "recovered" if you still need to restore data, warm up caches, or verify functionality.

**Resolve** is the most comprehensive—it captures the entire incident lifecycle from first detection through post-incident verification and closure.

Example scenario:

- 2:00 AM — Outage begins
- 2:15 AM — Alert fires, on-call acknowledges → Response time: 10 min
- 2:45 AM — Root cause identified, fix deployed → Repair time: 20 min
- 3:00 AM — Service verified working for users → Recovery time: 30 min
- 3:30 AM — Incident ticket closed after verification → Resolution time: 40 min

Related metrics:

- MTBF (mean time before failure)
- MTTF (mean time to failure)
- MTTA (mean time to acknowledge)

### DORA metrics

DevOps Research and Assessment (DORA) metrics are industry standard key performance indicators (KPIs) specifically for software engineering devops teams.

- **Change Lead Time** - Time to implement, test, and deliver code for a feature (measured from first commit to deployment). Be specific what you mean by start and finish. For example, some teams like to measure from when a developer starts work on a ticket, to when the the devloper pushes the work to a continuous integration process; this approach tends to be more effective for measuring person productivity.

- **Deployment Frequency** - Number of deployments in a given duration of time. Be specific what you mean by deployment and how you're tracking frequency. For example, some teams do a deployment to a fleet of production servers and track the full rollout, whereas other teams do demployment to a single user acceptance test (UAT) server and track when the project manager sends an email announcement to the users to try testing.

- **Change Failure Rate** - Percentage of failed changes over all changes (regardless of success). Be specific what you mean by failure. For example, some teams count a failure when they start on a work ticket then realize the work ticket doesn't have enough information to proceed, so the team rejects the work ticket back to the specification team.

- **Mean Time to Recovery (MTTR)** - Time it takes to restore service after production failure. Be specific what you mean by recovery. For example, some teams count recovery as complete resolution meaning total time from when an incident is detected to when an incident is fully closed; this approach is better described as Mean Time To Resolve.

- **Reliability** - A broader, often qualitative, assessment of service consistency. This metric helps show that high velocity does not come at the expense of system capability. Be specific what you mean by reliability. Some examples are availability, uptime, latency, accuracy, and service level objectives.

## Queue of queues

Queueing theory is easy to extend to a queue of queues, such a higher-level processes that contains lower-level stages, or phases, or gates.

### Double Diamond queue of queues

For example, consider the "Double Diamond" innovation process model, which specifies four stages:

1. Discover: Understand the issue rather than merely assuming what it is.
2. Define: With insight from the discovery phase, define the challenge.
3. Develop: Give different answers to the clearly defined problem.
4. Deliver: Test different solutions at a small scale then improve them.

Queueing theory can model this as one process queue that contains four stage queues:

- The arrival rate of of the process is the arrival rate of stage 1.
- The success rate of of the process is the success rate of stage 4.
- The error count of the process is the sum of the stages' error counts.
- The skip count of the process is the sum of the stages' skip counts.

### Funnel queue of queues

In our practical work, we frequently use the word "funnel" to loosely describe a queue-of-queues that tend to reduce the item count at each stage:

- Hiring queue that uses an outreach stage, interview stage, and offer stage. For this kind of queue, we aim to maximize the stage 1 arrival rate, meaning increasing our outreach, such as via promotions.

- Purchasing queue that uses a customer search stage, shopping cart stage, and payment stage. For this kind of queue, we aim to minimize the stage 2 skip rate, meaning reducing cart abandonment, such a via reminders.

- Programming queue that uses an integration stage, user acceptance stage, and production rollout stage. For this kind of queue, we aim to minimize the error rate of the production rollout stage, such as via canary telemetry.

## Insights

[Seven insights into queueing theory by Bob Wescott](seven-insights-into-queueing-theory-by-bob-wescott.pdf)

1. The slower the service center, the lower the maximum utilization you should plan for at peak load.

2. It’s very hard to use the last 15% of anything.

3. The closer you are to the edge, the higher the price for being wrong.

4. Response time increases are limited by the number that can wait.

5. Remember this is an average, not a maximum.

6. There is a human denial effect in multiple service centers.

7. Show small improvements in their best light.

## Epilog

### See also

Wikipedia:

- [Queueing theory](https://en.wikipedia.org/wiki/Queueing_theory)

- [M/M/1 queue](https://en.wikipedia.org/wiki/M/M/1_queue)

- [Little's law](https://en.wikipedia.org/wiki/Little%27s_law)

- [Markov chain](https://en.wikipedia.org/wiki/Markov_chain)

Wikipedia areas where we use queues in many projects:

- [Project management](https://en.wikipedia.org/wiki/Project_management)

- [Message queue](https://en.wikipedia.org/wiki/Message_queue)

- [DevOps](https://en.wikipedia.org/wiki/DevOps)

Introductions by John D. Cook:

- [The science of waiting in line](https://www.johndcook.com/blog/2019/01/23/queueing/)

- [Server utilization: Joel on queuing](https://www.johndcook.com/blog/2009/01/30/server-utilization-joel-on-queuing/)

- [What happens when you add a new teller?](https://www.johndcook.com/blog/2008/10/21/what-happens-when-you-add-a-new-teller/)

Introductions with more detail:

- [Queuing Theory: Simple Definition, Notation and Terminology](https://www.statisticshowto.datasciencecentral.com/queuing-theory/)

- [The most important thing to understand about queues - By Dan Slimmon](https://blog.danslimmon.com/2016/08/26/the-most-important-thing-to-understand-about-queues/)

- [Operations Research - Notes. By J E Beasley](http://people.brunel.ac.uk/~mastjjb/jeb/or/queue.html)

- [Little’s Law – the basis of Lean and Kanban](http://itsadeliverything.com/littles-law-the-basis-of-lean-and-kanban)

- [Investopedia: Queueing theory](https://www.investopedia.com/terms/q/queuing-theory.asp)

- [It's time for some queueing theory - By Kottke](https://kottke.org/19/01/its-time-for-some-queueing-theory)

### Thanks

[Accelerate: The Science of Lean Software and DevOps: Building and Scaling High Performing Technology Organizations. By Nicole Forsgren, Jez Humble, Gene Kim](https://www.amazon.com/dp/B07B9F83WM). This book is excellent for high level devops, and directly informs our choice of KPIs. The KPIs on this page align with the book's recommendations.

## Tracking

- Package: queueing-theory
- Version: 2.0.0
- Created: 2019-01-25T04:17:56Z
- Updated: 2026-02-07T10:42:20Z
- License: MIT or Apache-2.0 or GPL-2.0 or GPL-3.0 or contact us for more
- Contact: Joel Parker Henderson <joel@joelparkerhenderson.com>

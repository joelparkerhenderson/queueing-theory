# Queueing theory

Queueing theory is the mathematical study of waiting lines, or queues. We use queueing theory in our software development, for purposes such as project management kanban boards, inter-process communication message queues, and devops continuous deployment pipelines.

Contents:

* [Introduction](#introduction)
  * [Project management kanban boards](#project-management-kanban-boards)
  * [Inter-process communication message queues](#inter-process-communication-message-queues)
  * [Devops continuous deployment pipelines](#devops-continuous-deployment-pipelines)
* [Queue terminology](#queue-terminology)
  * [Queue types and service types](#queue-types-and-service-types)
  * [Queue dropouts](#queue-dropouts)
* [Queueing theory notation](#queueing-theory-notation)
  * [Arrival rate, service rate, dropout rate](#arrival-rate-service-rate-dropout-rate)
  * [Utilization ratio](#utilization-ratio)
  * [Error ratio](#error-ratio)
  * [Lead time, wait time, work time, step time](#lead-time-wait-time-work-time-step-time)
  * [Count](#count)
  * [Standard notation](#standard-notation)
* [Activity tracking](#activity-tracking)
  * [Activity examples](#activity-examples)
  * [Little's Law](#little-s-law)
  * [Key performance indicators (KPIs)](#key-performance-indicators-kpis)
* [Epilog](#epilog)
  * [See also](#see-also)
  * [Thanks](#thanks)


## Introduction

We use queueing theory in our software projects for many purposes:

  * Project management kanban boards

  * Inter-process communication message queues

  * Devops continuous deployment pipelines


### Project management kanban boards

For example, we want to know how a new feature progresses from design to delivery.

Some relevant products are e.g. [Asana](https://en.wikipedia.org/wiki/Asana_(software)), [Jira](https://en.wikipedia.org/wiki/Jira_(software)), [Microsoft Project](https://en.wikipedia.org/wiki/Microsoft_Project).


### Inter-process communication message queues

For example, we want to know how one program sends requests to another program.

Some relevant products are e.g. [RabbitMQ](https://en.wikipedia.org/wiki/RabbitMQ), [ActiveMQ](https://en.wikipedia.org/wiki/Apache_ActiveMQ), [ZeroMQ](https://en.wikipedia.org/wiki/ZeroMQ).


### Devops continuous deployment pipelines

For example, we use a continuous integration server to create a software release candidate, test it, then deploy it.

Some relevant products are e.g. [Jenkins](https://en.wikipedia.org/wiki/Jenkins_(software)), [Bamboo](https://en.wikipedia.org/wiki/Bamboo_(software)), [Azure DevOps](https://en.wikipedia.org/wiki/Microsoft_Visual_Studio#Azure_DevOps).


## Queue terminology

Queue terminology is a big topic. This section has some of our common terminology. For the examples, we will use the idea of a customer waiting in line.


### Queue types and service types

Queue types and service types describe how the queue chooses which items to process.

  * First In First Out (FIFO): serve the customer who has been waiting for the longest time.

  * Last In First Out (LIFO): serve the customer who has been waiting for the shortest time.

  * Priority: serve customers based on their priority level; these levels could be based on status, urgency, payment, etc.

  * Shortest Job First (SJF): serve the customer who needs the smallest amount of service.

  * Longest Job First (LJF): serve the customer who needs the largest amount of service.

  * Time Sharing: serve everyone at the same time; service capacity is distributed evenly among everyone waiting.


### Queue dropouts

Queue dropouts are when a customer does not make it through the queue.

  * Balking: when a customer decides not to start waiting for service because the wait time threatens to be too long.

  * Reneging: when a customer who has waited already decides to leave because they’ve wasted too much time.

  * Jockeying: when a customer switches between queues in a tandem queue system, trying to get a shorter wait.


## Queueing theory notation

Queueing theory uses notation with Greek letters.

Our teams use some of the popular notation; we also add some custom notion that help us with software projects.


### Arrival rate, service rate, dropout rate

The most important notation:

  * λ: arrival rate. This measures how fast new items are coming into the queue.

  * μ: service rate. This measures how fast items in the queue are being handled.

  * σ: dropout rate. This measures how fast items are skipping out the queue unhandled.

Examples:

  * λ = μ means the arrival rate equals the service rate; the queue is staying the same size, other than dropouts.

  * λ > μ means the arrival rate is greater than the service rate; the queue is getting larger, other than dropouts.

  * λ < μ means the arrival rate is less than the service rate; the queue is getting smaller, other than dropouts.


### Utilization ratio

The most important notation that summarizes a queue:

  * ρ: utilization ratio = λ / μ

Examples:

  * ρ = 1 means the arrival rate is equal to the service rate; the queue is staying the same size.

  * ρ > 1 means the arrival rate is greater than the service rate; the queue is getting larger.

  * ρ < 1 means the arrival rate is less than the service rate; the queue is getting smaller.


### Error ratio

The most important notation that summarizes a queue's success:

  * ε: error ratio = service failure count / service total count

Examples:

  * ε = 0 means no errors.

  * ε = 0.1 means 10% of services have an error.

  * ε = 1 means every service has an error.


### Lead time, wait time, work time, step time

We track four times:

  * τ: lead time = from arrival to finish

  * ω: wait time = from arrival to start of work

  * φ: work time = from start of work to finish

  * θ: step time = from finish to next finish

Examples:

  * τ = 5s means an item is added to the queue, then serviced 5 seconds later.

  * ω = 4s means an item waits in the queue for 4 seconds, then work starts.

  * φ = 1s means an item takes 1 second of work, then is complete.

  * θ = 1s means there's 1 second between one completion and the next completion.


### Count

We count items often, and we use this notation:

  * κ: count

Example:

  * κ = 100 means there are 100 items.

  * κ > 100 means there are more than 100 items.

  * κ ≫ 100 means there are many more than 100 items.



### Standard notation

Standard notation for queueing theory also uses these symbols:

  * n: the number of items in the system.

  * A: the arrival process probability distribution.

  * B: the service process probability distribution.

  * C: the number of servers.

  * D: the maximum number of items allowed in the queue at any given time, waiting or being served (without getting bumped).

  * E: the maximum number of items total.


## Activity tracking


### Activity examples

Suppose we have something we want to track, and we call it something generic such as "Activity" and abbrievated as "A".

We can efficiently use queuing notation to describe the activity and how it moves through a queue.

Examples:

  * Aκ: Activity count: how many items are in the queue.

  * Aλ: Activity arrival rate: how many items are incoming per time unit.

  * Aμ: Activity service rate: how many items are completed per time unit.

  * Aσ: Activity dropout rate: how many items are abandoned per time unit.

  * Aρ: Activity utilization ratio: how many items are arriving vs. completing.

  * Aε: Activity error ratio: how many items are completed with errors vs. total.

  * Aτ: Activity lead time: how much time elapses from requested to completed.

  * Aω: Activity wait time: how much time elapses from requested to started.

  * Aφ: Activity work time: how much time elapses from started to completed.

  * Aθ: Activity step time: how much time elapses from completed to next completed.


### Little's Law

Little's law is a theorem by John Little which states: the long-term average number L of customers in a stationary system is equal to the long-term average effective arrival rate λ multiplied by the average time W that a customer spends in the system.

Example notation:

  * L is the long-term average number of customers in the system.

  * λ is the long-term average effective arrival rate.

  * W is the average time that a customer spends in the system.

  * L = λ W is Little's law.

Little's law assumptions:

  * All measurement units are consistent.

  * Conservation of flow, meaning the average arrival rate equals the average departure rate.

  * All work that enters the system then flows through to completion.

  * The system is “stable”, meaning the average age of items are neither increasing or decreasing, and the total number of items is roughly the same at the beginning and at the end.


### Key performance indicators (KPIs)

We typically track many things about the activities in the queue, and we want to summarize the results by choosing a shortlist of the most relevant ones for our projects.

We have built many projects, and we believe the most valuable summary indicators are:

  * Dτ = Delivery lead time. Product teams may say "from concept to customer" or "from idea to implementation".

  * Dμ = Delivery service rate. Devops teams may say "deployment frequency" or "we ship X times per day".

  * Dε = Delivery error ratio. Quality teams may say "change fail rate" or "percentage of rollbacks".

  * Rτ = Restore lead time. Site reliability engineers may say "time to restore service" or "mean time to restore (MTTR)".



## Epilog


### See also

Wikipedia:

  * [Queueing theory](https://en.wikipedia.org/wiki/Queueing_theory)

  * [M/M/1 queue](https://en.wikipedia.org/wiki/M/M/1_queue)

  * [Little's law](https://en.wikipedia.org/wiki/Little%27s_law)

  * [Markov chain](https://en.wikipedia.org/wiki/Markov_chain)

Wikipedia areas where we use queues in many projects:

  * [Project management](https://en.wikipedia.org/wiki/Project_management)

  * [Message queue](https://en.wikipedia.org/wiki/Message_queue)

  * [DevOps](https://en.wikipedia.org/wiki/DevOps)

Introductions by John D. Cook:

  * [The science of waiting in line](https://www.johndcook.com/blog/2019/01/23/queueing/)

  * [Server utilization: Joel on queuing](https://www.johndcook.com/blog/2009/01/30/server-utilization-joel-on-queuing/)

  * [What happens when you add a new teller?](https://www.johndcook.com/blog/2008/10/21/what-happens-when-you-add-a-new-teller/)

Introductions with more detail:

  * [Queuing Theory: Simple Definition, Notation and Terminology](https://www.statisticshowto.datasciencecentral.com/queuing-theory/)

  * [Operations Research - Notes. By J E Beasley](http://people.brunel.ac.uk/~mastjjb/jeb/or/queue.html)

  * [Little’s Law – the basis of Lean and Kanban](http://itsadeliverything.com/littles-law-the-basis-of-lean-and-kanban)

  * [Investopedia: Queueing theory](https://www.investopedia.com/terms/q/queuing-theory.asp)

Blog posts:

  * [It's time for some queueing theory - By Kottke](https://kottke.org/19/01/its-time-for-some-queueing-theory)


[Seven Insights Into Queueing Theory](http://www.treewhimsy.com/TECPB/Articles/SevenInsights.pdf):

  * The slower the service center, the lower the maximum utilization you should plan for at peak load. 

  * It’s very hard to use the last 15% of anything.

  * The closer you are to the edge, the higher the price for being wrong.

  * Response time increases are limited by the number that can wait.

  * Remember this is an average, not a maximum.

  * There is a human denial effect in multiple service centers.

  * Show small improvements in their best light.


### Thanks

[Accelerate: The Science of Lean Software and DevOps: Building and Scaling High Performing Technology Organizations. By Nicole Forsgren, Jez Humble, Gene Kim](https://www.amazon.com/dp/B07B9F83WM). This book is excellent for high level devops, and directly informs our choice of KPIs. The KPIs on this page align with the book's recommendations.

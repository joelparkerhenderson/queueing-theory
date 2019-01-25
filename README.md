# Queueing theory

Queueing theory is the mathematical study of waiting lines, or queues. We use queueing theory in our software development, for purposes such as project management kanban boards, inter-process communication message queues, and devops continous deployment pipelines.

Contents:

* [Introduction](#introduction)
  * [Project management kanban boards](#project-management-kanban-boards)
  * [Inter-process communication message queues](#inter-process-communication-message-queues)
  * [Devops continous deployment pipelines](#devops-continous-deployment-pipelines)
* [Queueing theory notation](#queueing-theory-notation)
  * [Arrival rate, service rate, dropout rate](#arrival-rate-service-rate-dropout-rate)
  * [Utiltization ratio](#utiltization-ratio)
  * [Error ratio](#error-ratio)
  * [Lead time, wait time, work time, step time](#lead-time-wait-time-work-time-step-time)
  * [Count](#count)
* [Activity tracking](#activity-tracking)
  * [Activity examples](#activity-examples)
  * [Key performance indcators (KPIs)](#key-performance-indcators-kpis)


## Introduction

We use queueing theory in our software projects for many purposes:

  * Project management kanban boards

  * Inter-process communication message queues
 
  * Devops continous deployment pipelines


### Project management kanban boards

For example, we want to know how a new feature progresses from design to delivery. 

Some relevant products are e.g. [Asana](https://en.wikipedia.org/wiki/Asana_(software)), [Jira](https://en.wikipedia.org/wiki/Jira_(software)), [Microsoft Project](https://en.wikipedia.org/wiki/Microsoft_Project).


### Inter-process communication message queues

For example, we want to know how one program sends requests to another program.

Some relevant products are e.g. [RabbitMQ](https://en.wikipedia.org/wiki/RabbitMQ), [ActiveMQ](https://en.wikipedia.org/wiki/Apache_ActiveMQ), [ZeroMQ](https://en.wikipedia.org/wiki/ZeroMQ).


### Devops continous deployment pipelines

For example, we use a continuous integration server to create a software release candidate, test it, then deploy it.

Some relevant products are e.g. [Jenkins](https://en.wikipedia.org/wiki/Jenkins_(software)), [Bamboo](https://en.wikipedia.org/wiki/Bamboo_(software)), [Azure DevOps](https://en.wikipedia.org/wiki/Microsoft_Visual_Studio#Azure_DevOps).


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


### Utiltization ratio

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

  * ω = 4s means an item waits in the queue for 3 seconds, then work starts.

  * φ = 1s means an item takes 1 second of work, then is complete.

  * θ = 1s means there's 1 second between one completiong and the next completion.


### Count

We count items often, and we use this notation:

  * κ: count

Example:

  * κ = 100 means there are 100 items.

  * κ < 100 means there are less than 100 items.

  * κ ≫ 100 means there are a large number of items, many more than 100.


## Activity tracking


### Activity examples

Suppose we have something we want to track, and we call it something generic such as "Activity" and abbrievated as "A".

We can efficiently use queuing notation to describe the activity and how it moves through a queue.

Examples:

  * Aκ: Activity count: how many items are in the queue.

  * Aλ: Activity arrival rate: how many items are requested per time unit.

  * Aσ: Activity dropout rate: how many items are abandoned per time unit.

  * Aμ: Activity service rate: how many items are completed per time unit.

  * Aε: Activity error ratio: how many items are completed with errors vs. total.

  * Aτ: Activity lead time: how much time elapses from requested to completed.

  * Aω: Activity wait time: how much time elapses from requested to started.

  * Aφ: Activity work time: how much time elapses from started to completed.

  * Aθ: Activity step time: how much time elapses from completed to next completed.


### Key performance indcators (KPIs)

We typically track many things about the activities in the queue, and we want to summarize the results by choosing a shortlist of the most relevant ones for our projects.

We have built many projects, and we believe the most valuable summary indicators are:

  * Dτ = Delivery lead time. Some product teams know this as "from concept to customer" or "from idea to implementation".

  * Dμ = Delivery service rate. Some devops teams know this as "continuous deployment frequency" or "shipping X times per day".

  * Dε = Delivery error ratio. Some quality assurance teams call this "change fail rate" or "percentage of deployment rollbacks".

  * Rτ = Restore lead time. Some site reliability engineers call this "time to restore service" or "mean time to restore (MTTR)".


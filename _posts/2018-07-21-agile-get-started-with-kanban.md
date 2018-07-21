---
title: 'Agile - Get Started with Kanban'
date: 2018-07-21T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/07/agile-get-started-with-kanban
categories:
  - development
tags:
  - development
  - software
  - methodology
  - kanban
  - management
  - agile
  - project
---

After I introduced Scrum in my last article, today will be about Kanban. 
Like Scrum, Kanban is an agile software development methodology and I have to say that I apply the principles 
I present today to most of my personal side projects.

Kanban is Japanese and can be translated as "billboard". 
Kanban was first introduced by Toyota in the automotive industry and is now mostly associated with just-in-time concepts. 
But I don't want to bore you with historical details, I want to show you the usage in software development.

Before we start, you will certainly want to know what advantages Kanban can offer you: 
Kanban helps you to increase quality and speed up the realisation of tasks as well as to identify and eliminate bottlenecks. 
This is done by minimizing the time that tasks spend in queues.

There are two basic concepts on which Kanban is based: On the one hand this is visualization, 
on the other hand the limitation of parallel work (Work in Progress, WIP).

The visualization of the work offers various advantages. Tasks are not forgotten, 
which can reduce stress and make it easier to make good decisions. 
It also simplifies the estimation of how long a task will take to complete.

Limiting parallel work reduces the waiting time of tasks and thus increases the flow of the process. 
At this point I would like to define two key indicators: Lead time and cycle time.

Lead time describes the period from receipt of a request to delivery of an associated feature. 
It is therefore the time period that is visible to the user. 
Cycle time describes the duration of a task, starting from the beginning of the work until its result is ready for delivery.

The goal of Kanban is to reduce the response time of a system. This can be achieved in two ways: 
Increasing the throughput of the system or reducing parallel work and thus reducing the time of tasks in queues.

Let us illustrate this with an example. The workflow of a software company is as follows: 

* After a problem is reported (1), it remains in the ticket list until the next meeting (2).
* At this meeting the relevance of the problem is prioritized, in the following days the solution will be implemented by a developer (3)
* Next comes the test of the implementation (4), planning of the release (5) and finally the deployment (6)
* There are always periods of time between these individual stations that are not used to add value

![Value Stream Map in Kanban](/assets/2018/20180722_kanban_valuestreammap.png)

The following key figures result for this process:
	• Total time (value adding): 6 h 15 min
	• Total waiting time: 20 days
	• Lead Time: 485.25 hours
	• Efficiency: Time (value-adding) / Lead Time = 6.25 / 485.25 = 1.29
	• WIP = Lead Time / Time (value-adding) = 485.25 / 6.25 = 77.64

As you can see, the efficiency is anything but great. According to Littles Law, there is a WIP of about 78 elements. 
By reducing this to half, the time required could also be reduced to half.

But now really to software development: 
The idea to use Kanban in software development is to establish a Kanban board to organize tasks. 
Such boards are divided into different columns that define the corresponding stages of a task. 
Typically these are things like "In planning", "Development", "Test", "Deployment". 
You can see an example of such a board in the following screenshot:

![Kanban Board Example](/assets/2018/20180722_kanban_board.png)

Different tasks are then moved into these columns, with each task going through the development process from left to right. 
As already mentioned, an important principle of Kanban is the limitation of parallel work. 
That is why each column may only contain a certain predefined maximum number of elements. 
If a task spends too long in a certain column, problems with the specific task or bottlenecks in the process can be identified. 
These are then solved as a team, keeping the stress of the people involved at the appropriate points as low as possible.

The conversion of a kanban board can be carried out both physically and by software. With a physical kanban board, 
the tasks can be displayed with post-items. The advantage here is the ubiquity of the board, 
each team member is always aware of the current status. 
Alternatively, software solutions such as Trello or Jira offer themselves, which follow exactly the same principle, 
whereby tasks can be provided here with task assignments, checklists, deadlines and comments.

When transferring tasks from one area to the next, it is important to establish a pull system. 
Completed tasks should therefore not be pushed to the next level by the people who have completed them. 
Instead, the employees there should pull the task to themselves.

## Kanban and Scrum

Now that you have a rough idea what Kanban is, let's compare the methodology briefly with Scrum.
Both are concepts of agile software development, but what is the best solution for which purpose?

Both concepts are based on a pull system. 
Kanban uses this to move tasks to the next area, Scrum using the agreement of user stories for the sprint backlog. 
Both concepts are based on transparency and aim to reduce parallel work to a minimum. 
Scrum implements this rather indirectly, since the requirements that are implemented within a single sprint are clearly defined.

The product is also constantly being updated and kept release-ready in both approaches. 
The self-organization of the team also plays an important role in both Scrum and Kanban and both concepts require the division 
of requirements into as small subtasks as possible.

But of course there are also various differences. Kanban does without the iterative model of Scrum, for example, 
tasks run through the entire cycle individually. A Scrum team agrees before each sprint which tasks are to be implemented. 
Kanban, on the other hand, makes these commitments optional. 

Kanban does not prescribe roles like the division into Scrum Master, Product Owner and Team as known from Scrum.

It is difficult to define exactly when which methodology should be used. 
As in the previous article, it is often the case that none of the methodologies is used in its pure form, 
but a mixed version according to the team' s requirements.

Kanban is suitable for various IT use cases. In my opinion, Kanban is a more flexible way of dealing with requirements than Scrum is.
The concept makes sense here for applications where malfunctions and short-term maintenance are to be expected more frequently.
Here, the interval cycles of Scrum are simply too long. 
In addition, Kanban may be easier to implement in corporate structures that still work with the waterfall model, 
since Kanban allows a slow changeover.

## One last thing

Finally, in my opinion, Scrum is also excellently suited for the realization of own projects, 
in principle in the form of a function-enhanced TODO list. You can then combine the tasks for yourself with the Pomodoro technique, 
for example, to really make efficient use of your private time.

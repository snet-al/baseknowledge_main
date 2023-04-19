# 2. Scrum

Scrum is the process and practises
which help the developer give the highest value.
Scrum is one of the most successful implementation of agile methodology. It is based on empirical process control theory, or **empiricism**.
<br>Empiricism asserts that knowledge comes from experience and making decisions based on what is known.
<br><br>Scrum employs an iterative, [incremental](https://www.d.umn.edu/~tcolburn/cs4531/slides/software_engineering/principles/principles.xhtml) approach to optimize predictability and control risk.
Three pillars uphold every implementation of empirical process control: `transparency`, `inspection`, and `adaptation`.

## 2.1 Transparency

Significant aspects of process must be visible to those responsible for the outcome. This is because the decisions that should be made for a specific task, is the best only if all the information (and other aspects) needed for it is fully transparent.
Transparency requires that those aspects be defined by a common standart so observers share a common understanding of what is being seen.
There are two elements which should be defined:

- "DONE" <br>The definition of Done, which specifies when a task is considered completed, and what is the **value** that the work done to complete to the task has **incremented** the **product**. <br>

  - Acceptance criteria: will be one of the elements which will be used to help the definition of "Done".<br> If the DoD is general refer: [DOD](./Artifacts/Done/).

* "Scrum Artifacts" <br>Are the documents with which the participants communicate with each other aspects of process and product. The transparency requires that these artifacts share a simple and descriptive language. We will list some of the most used scrum artifacts"

  - Product Vision: Is an artifact to define the long-term goal of the project/product. It sets the overall direction and guides the Scrum Team. Everyone should be able to memorize the Product Vision; therefore it must be short and precise.

  - Sprint Goal: Helps to focus the Sprint. It is the objective that will be met within the Sprint through the implementation of the forecasted Product Backlog items, and it provides guidance to the Development Team on why it is building the Product Increment

  - Product backlog: Is an ordered list of everything that might be needed in the product and is
    the single source of requirements for any changes to be made to the product. The Product
    Owner is responsible for the Product Backlog, including its content, availability, and ordering.
  - Sprint backlog: Is the set of Product Backlog items selected for the Sprint, plus a plan for
    delivering the product **Increment** and realizing the **Sprint Goal**

  - Product backlog item: **User Story** as we will call it from now on, is the smallest artifact we will describe in details and be the base for the two previous artifacts.

    - Please refer to link: [User Story](./Artifacts/US/).

## 2.2 Inspection and adaption

The scrum as an iterative process, bases  gathering of information and knowladge on specific intervals, called scrum events. These events should not be so frequent that inspection gets in the way of the work, and are most beneficial when diligently performed by skilled inspectors at the point of work.
So every event should have one purpose, to help the process to go toward the goal, and to adapt if detecting undesirable variances.
<br><br>
Based on best practises the scrum process is organized in intervals called __sprints__, which are the heart of the scrum.
And during the sprint these main events are defined for inspection and adaption:

1. Sprint planning
2. Daily Scrum
3. Sprint Review
4. Sprint Retrospective

### 2.2.1 Sprint
The heart of Scrum is a Sprint, a time-box of two weeks during which a “Done”, useable, and potentially releasable product Increment is created.
The basic idea of a "sprint" is "to syncronize the team __work__ with client needs" 
<br>
The main artifact of the sprint is the __SPRINT GOAL__ which is negotiated by PO and the development team, and this goal should not be changed during the sprint.
Each sprint should be considered as a project with a plan defined in __SPRINT GROAL ARTIFACT__.
<br>
The responsibiltity of each team member, is to work to accomplish the goal.

### 2.2.2 Sprint planning
The work to be performed in the Sprint is planned at the Sprint Planning. This plan is created by the collaborative work of the entire Scrum Team. Sprint Planning is time-boxed to a maximum of 4 hour.
The input to this meeting is the Product Backlog, the latest product Increment, projected capacity of the Development Team during the Sprint, and past performance of the Development Team. The number of items selected from the Product Backlog for the Sprint is solely up to the Development Team. Only the Development Team can assess what it can accomplish over the upcoming Sprint.
<br><br>
After the Development Team forecasts the Product Backlog items it will deliver in the Sprint, the Scrum Team crafts a Sprint Goal. The Sprint Goal is an objective that will be met within the Sprint through the implementation of the Product Backlog, and it provides guidance to the Development Team on why it is building the Increment.

### 2.2.3 Daily Scrum
The Daily Scrum is a 15-minute time-boxed event for the Development Team to synchronize activities and create a plan for the next 24 hours. Main points of the meetings are:
-  What did I do yesterday that helped the Development Team meet the Sprint Goal?
- What will I do today to help the Development Team meet the Sprint Goal?
- Do I see any impediment that prevents me or the Development Team from meeting the Sprint Goal?

### 2.2.4 Sprint Review
A Sprint Review is held at the end of the Sprint to inspect the Increment and adapt the Product Backlog if needed. This is the moment the PO inspect the progress of the goal, and compares it with the remainging work. This report is made transparent to all stakeholders.

### 2.2.5 Sprint Retrospecitve
The Sprint Retrospective is an opportunity for the Scrum Team to inspect itself and create a plan for improvements to be enacted during the next Sprint.

## 2.3 Scrum Team

- PO (Product owner) : [PO](./Team/PO/README.md).
- Scrum Master
- Development team (Self organized, cross-functional team)

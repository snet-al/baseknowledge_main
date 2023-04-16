# 1. Engineering

Engineering is the process and practises which help the product of a developer has the highest value. For software determining such value may involve a high degree of uncertainty and may vary strongly depending on the owner. But based on best practices we will point some standards we believe will increase the value of each line we code.

## 1.1 Creating value

The software is a series of lines of code and from the perspective of engineering, to give value to a software we should:

### Create value from each line of code.

So each developer should give the _right time_ and _effort_ to create a line of code which is:

- Readable
- Optimized

> The value created by readability is the time saved for searching and analyzing the actual code. As most of the statistics and professional programmers says that 80% of the time the programmer read the code and only 20% write. If we create a readable code we not only help our selves, but also the others how could use our code, work on our code or just review our code.
> <br>
> Another value added by readability is the team work and **pair programming**. As we intend to use pair programming specially for training and integration of junior developers in a project, a process where the developer gives the right time to create the line of code, help the other developer to be more into the process.

> The value of optimization is more measurable at the end of the process of coding, during the utilization of the software by the customers. But one important aspect of optimization is that it should be non blocking for the task. The best way to do that is to separate the concerns of writing the business logic and optimization. Those should be in an overlapping but separate processes, as agile suggests.

### Create value for each module

- Reusability
- Maintainability

> Reusability is the most valuable part in code management.
> Of course this is a difficult process and is expected by senior developers, but as agile (scrum) suggest everything is an incremental process, so first we create a reusable function, then an class, then an module, and then a full software.

As we will explain in details in clean code section there are two points i would like to focus:

1. Layers of abstraction
2. Structure of code

To see more details refer to: [Clean Code](./Engineering/CleanCode).

## 1.2 Code management

To create and manage the value of the code, version control systems like git emerged. Git is a tool for managing code in developer level and team level, so agile encourage using it.

At section [CodeManagement](./Engineering/CodeManagement) will show the usage of git naming conventions.

## 1.3 Clean Code

Clean code is a list of standards and patterns which help everyone in developing his/her skills not only in developing, but also in managing and communicating. We are based on the book of Robert C. Martin (Uncle Bob), [Clean Code](./CleanCodeBook) to extract some of the main points he emphasis, but i strongly recommend you read it.

Clean code is organized in two section:

1. General Standards [View here](./Engineering/CleanCode)
2. Technological specific standart

The second point will be modified by specific deparment

> [PHP Coding Standards ...](./Engineering/CleanCode/php)

> [Javascript Coding Standards ...](./Engineering/CleanCode/js)

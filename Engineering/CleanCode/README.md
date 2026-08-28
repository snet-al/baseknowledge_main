### Clean Code

One of the best names Uncle Bob gives to developers is **authors**. We are authors.
And one thing about authors is that they have readers, so it is our responsibility to
communicate clearly with our readers. This responsibility is even higher when we enter
an Agile and Scrum environment, where the main concepts are the team and collaboration.

But it is not enough to write the code well; it also has to be kept clean over time.
We have all seen code rot and degrade as time passes. We must take an active role in preventing this degradation.

So the first rule of Clean Code is:

> If we all checked in our code a little cleaner than when we checked it out, the code simply could not rot.
> The cleanup does not have to be something big. Change one variable name for the better.

### Part 1. Naming is the Root of Programming

> The name of a variable, function, or class should answer all the big questions.
> It should tell you why it exists, what it does, and how it is used.
> If a name requires a comment, then the name does not reveal its intent.

Naming is the base of programming.

- **Why**

  When asking why a name exists, we are first asking about its `layer of abstraction`.
  For example, `PersonEntity` — or simply `Person` in some cases — tells us that it contains domain logic and therefore tells us what it should know.
  If we add `SQL` knowledge to that class, the class now knows something other than the domain, so we may be violating the Single Responsibility Principle.

- **What**

  A variable, class, function, or other software element should tell us what it represents or does through its name.

- **How**

  This is combined with **Why**, so we have as much knowledge as possible about what the content of a variable, class, or function should contain.

1. **Be explicit**

   As cited in the Clean Code book, variables should not only be simple to see:

   `$a = 1`

   They should reveal their meaning:

   `$statusOfGame = STATUS_ONE`

2. **Avoid disinformation**

   Avoid short names and words that can have more than one meaning, such as:

   `hp`, `aix`, and `sco`

   Do not refer to a grouping of accounts as an `accountList` unless it is actually a `List`.
   The word `list` means something specific to programmers. If the container holding the accounts is not actually a `List`, it may lead to false conclusions.

   Therefore, `accountGroup`, `bunchOfAccounts`, or simply `accounts` may be better.

   Avoid redundant names. Inside a `User` class, it is not necessary to declare a variable such as `userSurname`; `surname` is enough.

   Noise words are redundant. How is `NameString` better than `name`? Would a name ever be a floating-point number?
   Distinguish names so that the reader understands what difference each name communicates.

3. **Use searchable names**

   With the use of IDEs, a lot of programming work has become searching for code that has already been written.
   To support this process, think in advance and name variables — and especially functions — in a searchable way.

4. **Class names should be nouns, not verbs**

   Avoid vague words such as `Manager`, `Processor`, `Data`, or `Info` in the name of a class unless they communicate a precise responsibility.

5. **Method names should be verbs**

6. **Use pronounceable names**

### Part 2. Multi-dimensional Space of Software Principles

> Software-development principles are guidelines that help us create software that is easier to understand, maintain, change, extend, test, run, deploy, and operate over time.

Before studying individual principles, we should understand one foundational idea:

> **Software can be viewed from multiple dimensions. Each dimension asks a different question about the same software.**

Think of software as an object in a multi-dimensional space. We can examine this object from different directions, which we call **axes**.

For example, we can ask:

- Why does this code change?
- What technical responsibility does this code have?
- What does this code depend on?
- How easy is it for another developer to understand?
- How easy is it to extend?
- How does it behave at runtime?
- How easy is it to test?
- How easy is it to deploy and operate?

Each question gives us a different view of the same software.

#### Axis, Principle, Pattern, Paradigm, and Quality Attribute

These concepts are related, but they are not the same.

| Concept | Meaning | Example |
| --- | --- | --- |
| **Axis** | A direction or question from which we analyze software | Reason to change |
| **Principle** | Guidance for decisions viewed from one or more axes | Common Closure Principle |
| **Pattern or practice** | A reusable way to apply principles | Feature-based folders or co-location |
| **Paradigm** | A broad model for thinking about and constructing software | Object-Oriented or Functional Programming |
| **Quality attribute** | A property we want the software to have | Maintainability, testability, performance, or deployability |

Their relationship can be understood approximately like this:

```text
                    SOFTWARE
                       │
              observed through
                       │
                      AXIS
                       │
                evaluated using
                       │
                   PRINCIPLES
                       │
                implemented with
                       │
               PATTERNS / DESIGN
                       │
                  influences
                       │
               QUALITY ATTRIBUTES
```

This distinction is important.

Object-Oriented Programming is not simply an axis. It is a paradigm that gives us concepts such as objects, encapsulation, polymorphism, inheritance, and composition.

Functional Programming is another paradigm that gives us concepts such as pure functions, immutability, function composition, and explicit data transformations.

A paradigm can influence several axes and quality attributes at the same time.

#### Look at One Axis at a Time

When reviewing software, consciously select one axis and ask the questions of that axis before moving to another one.

This does not mean that the other dimensions do not exist. Separating them helps us reason clearly.

If we mix naming, dependencies, domain boundaries, performance, security, testing, deployment, and folder organization into the same argument, it becomes difficult to identify the exact problem we are trying to solve.

A useful engineering process is:

```text
Choose an axis
      ↓
Ask the question of that axis
      ↓
Identify the problem
      ↓
Apply the relevant principles
      ↓
Evaluate trade-offs on the other axes
```

Cleaning and designing software is an incremental process. We analyze one property, improve it, and then examine the software again from another direction.

#### Example: Two Axes of Organization

Consider a system containing users and products.

One possible axis is:

> **Reason to change**

The Common Closure Principle tells us that classes that change for the same reason should be kept together.

From this direction, we may organize the software like this:

```text
user/
    UserService
    UserController
    UserRepository

product/
    ProductService
    ProductController
    ProductRepository
```

The important question is:

> **Which pieces of software are likely to change together?**

If User functionality changes, `UserService`, `UserController`, and `UserRepository` may often participate in the same change. Keeping them close can reduce the distance and scope of that change.

This is the fundamental idea behind feature-based organization and code co-location.

However, we can look at the same software from another axis:

> **Technical responsibility**

From this direction, we may organize the same classes like this:

```text
controllers/
    UserController
    ProductController

services/
    UserService
    ProductService

repositories/
    UserRepository
    ProductRepository
```

Now the important question is:

> **Which pieces of software perform the same technical responsibility?**

This organization also has advantages. A developer looking for persistence-related code, for example, immediately knows where repositories are located.

#### Both Views Can Be Valid

The two structures may initially appear contradictory:

```text
Group by feature or domain.

vs.

Group by technical responsibility.
```

But one is not automatically correct and the other wrong. They are projections of the same software from different axes.

```text
                    SOFTWARE

          Reason-to-Change Axis
                  │
        User ─────┼───── Product
                  │

       Technical-Responsibility Axis
                  │
 Controller ── Service ── Repository
```

The architectural question is therefore not only:

> "Which folder structure is correct?"

A better question is:

> **Which axis is more important for this system, at this level, and what trade-offs are we accepting on the other axes?**

This reasoning applies beyond folder organization. It can be applied to architecture, dependencies, data, testing, runtime behavior, deployment, security, performance, and team organization.

#### Axes Are a Reasoning Model

We can imagine the axes as directions that are 90 degrees from each other so that we can study one property without confusing it with another.

However, this is a reasoning model, not a claim that software properties are mathematically independent.

A decision on one axis can influence another:

- Reducing dependencies can improve maintainability and testability.
- Increasing abstraction can improve extensibility but make code harder to understand.
- Co-locating related code can reduce change distance but move away from familiar framework conventions.
- Improving deployment independence can increase operational complexity.

The axes are analytically separate viewpoints, but architectural decisions usually affect several dimensions at once.

#### Principles Are Not Absolute Rules

A principle should be understood together with:

1. The **axis** it uses to examine software.
2. The **quality attribute** it tries to improve.
3. The **context** in which it is applied.
4. The **trade-offs** it creates on other axes.

Principles sometimes appear to contradict each other. For example:

```text
DRY
Avoid duplicating knowledge.

vs.

Low coupling
Avoid unnecessary dependencies.
```

Removing duplication may require a shared abstraction. That abstraction can remove duplication while coupling parts of the system that previously evolved independently.

Neither principle automatically wins. The engineer must understand which quality is more important in that context.

> **Architecture is the management of trade-offs across multiple dimensions of software.**

There is no architecture that maximizes every quality attribute at the same time.

- A design may improve maintainability while reducing runtime performance.
- A design may improve extensibility while increasing complexity.
- A design may improve deployment independence while increasing operational cost.
- A design may improve abstraction while making the system harder for a junior developer to understand.

Our job is not to follow principles blindly. Our job is to understand what each principle optimizes, why that property matters, and what we sacrifice by applying it.

#### The SNET Engineering Map

This document does not attempt to define every possible software axis immediately. The map should grow as the SNET engineering knowledge base grows.

An initial map can include:

```text
Reason to Change
    SRP, CCP, co-location

Extension / Modification
    OCP, plugin design, Strategy pattern

Behavioral Substitutability
    LSP, behavioral subtyping

Client Dependencies
    ISP

Dependency Direction
    DIP, dependency injection, ports and adapters

Human Understanding
    Naming, formatting, KISS, conventions

Duplication / Knowledge Ownership
    DRY, single source of truth

Runtime
    Performance, concurrency, caching, scalability

Deployment / Operations
    Statelessness, containerization, configuration, observability

Data Correctness
    Transactions, consistency, idempotency, auditability

Security
    Least privilege, defense in depth, secure defaults
```

Some principles will belong mainly to one axis. Others will affect several axes. That is not a classification failure; it means the principle acts like a vector rather than moving in only one direction.

This gives us a foundational rule for the SNET engineering knowledge base:

> **Do not learn only the rule. Learn the axis from which the rule looks at software, the quality it tries to improve, the trade-offs it creates, and the context in which it should be applied.**

When documenting a principle, we should answer:

```text
WHY does this principle exist?
        ↓
WHAT dimension does it examine?
        ↓
WHAT quality does it try to improve?
        ↓
WHAT trade-offs does it introduce?
        ↓
WHEN should we apply it?
```

Understanding these questions is more important than memorizing the rule itself.

#### SOLID Principles

SOLID is a set of five design principles that helps developers reason about responsibilities, extension, substitution, interfaces, and dependencies.

SOLID does not represent one axis. Each principle looks at software from a different primary direction:

1. **Single Responsibility Principle (SRP)**  
   A class should have only one reason to change.  
   Primary axis: **reason to change**.

2. **Open/Closed Principle (OCP)**  
   Software entities should be open for extension but closed for modification.  
   Primary axis: **extension versus modification**.

3. **Liskov Substitution Principle (LSP)**  
   Subtypes must be substitutable for their base types without changing the correctness of the program.  
   Primary axis: **behavioral substitutability**.

4. **Interface Segregation Principle (ISP)**  
   Clients should not be forced to depend on interfaces they do not use.  
   Primary axis: **client dependencies**.

5. **Dependency Inversion Principle (DIP)**  
   High-level policy should not depend directly on low-level implementation details. Both should depend on abstractions.  
   Primary axis: **dependency direction**.

The same method should be used throughout this knowledge base:

> **Do not learn only what a principle says. Learn the dimension from which it is looking at the software.**

### Part 3. Functions

The first rule of functions is that they should be small.

The second rule is that they should be smaller than that.

1. **Block and indentation**

   Functions should have at most two levels of indentation.

2. **Functions should do one thing**

   Avoid having one function do two things, such as calculating a value and printing it.

3. **One level of abstraction per function**

   Statements in a function should be at the same level of abstraction.

4. **Step-down rule**

   We want to read the code like a top-down narrative.

5. **Number of arguments**

   The ideal number of arguments for a function is zero.

6. **Prefer exceptions instead of returning error codes**

7. **DRY principle — Don't Repeat Yourself**

8. **Structured programming**

   Structured programming, as specified by Dijkstra, says that every function — or even a block within a function — should have a single entry point and a single exit.
   Following this rule strictly, a function should have a single `return` statement, no `break` or `continue` in `for` statements, and never a `goto`.

   Every communication of a function with other functions should occur as little as necessary inside a function.

   For example, consider a FizzBuzz application.

   **First solution:**

   ```javascript
   for (let i = 1; i < 100; i++) {
      if (i % 3 === 0 && i % 5 === 0) {
         console.log("FizzBuzz")
      } else if (i % 3 === 0) {
         console.log("Fizz")
      } else if (i % 5 === 0) { // condition 1
         console.log("Buzz")
      } else {
         console.log(i)
      }
   }
   ```

   This is a solution after debugging and correcting errors, but let us review it.

   Look at `condition 1`. Is it possible to determine whether that condition is checked by viewing only that line?
   The answer is no. That line depends on the conditions that came before it.

   In this example the code is small, but imagine the same type of dependency in a more complex codebase with many files.

   The first step toward creating more locally understandable code is to remove those hidden dependency relationships.

   **Second solution — remove the `else`:**

   ```javascript
   for (let i = 1; i < 100; i++) {
      if (i % 3 === 0 && i % 5 === 0) {
         console.log("FizzBuzz")
      }

      if (i % 3 === 0 && i % 5 !== 0) {
         console.log("Fizz")
      }

      if (i % 5 === 0 && i % 3 !== 0) {
         console.log("Buzz")
      }

      if (i % 3 !== 0 && i % 5 !== 0) {
         console.log(i)
      }
   }
   ```

   At first, this solution may look more complex than the first one.
   However, the philosophy of Clean Code is to concentrate on one concept at a time, improve that concept, and then continue to the next one.

   Cleaning code is an incremental process.

   Now we want to structure the code further, so the next phase is to use a variable.

   **Third solution:**

   ```javascript
   for (let i = 1; i < 100; i++) {
      let toPrint = ""

      if (i % 3 === 0) {
         toPrint = "Fizz"
      }

      if (i % 5 === 0) {
         toPrint += "Buzz"
      }

      if (i % 3 !== 0 && i % 5 !== 0) {
         toPrint = i
      }

      console.log(toPrint)
   }
   ```

   The importance of this variable is the separation of function calls.
   `console.log` should be considered external communication, so we should call external functions only as often as necessary.

### Part 4. Formatting, Consistency, and Existing Projects

Code formatting is important because code is communication, and communication is one of the professional developer's first responsibilities.

Formatting is broader than spaces, indentation, and line length. It also includes naming conventions, file organization, the position of functions, error-handling styles, architectural patterns, and the general way in which a project expresses its ideas.

When entering an existing project, the first responsibility of a developer is not to immediately introduce a preferred style, pattern, or new paradigm. The first responsibility is to understand the language that the project and its team already use.

> **In an existing codebase, consistency with the project is usually more valuable than local perfection.**

A solution may be modern, elegant, or theoretically better in isolation and still make the complete system harder to understand when it is introduced only in one part of the project.

For example, imagine that an existing application organizes its code by technical responsibility:

```text
controllers/
services/
repositories/
```

A developer may prefer a feature-based structure:

```text
user/
product/
orders/
```

The feature-based structure may be valid, but changing only one module creates two organizational languages inside the same project. A developer must now understand both structures and remember which rule applies in each part of the codebase.

The result may be locally cleaner code but a globally less coherent system.

This gives us an important SNET engineering rule:

> **Do not introduce a second architecture accidentally.**

A new paradigm is not automatically better merely because it is newer. Before introducing it, we must understand:

- What problem in the current project it solves.
- Which software axis it improves.
- Which quality attribute it targets.
- What migration and learning cost it introduces.
- Whether the team has agreed to use it consistently.
- Whether the existing code can be migrated safely and incrementally.

The correct process when entering an existing project is:

```text
Observe the existing project
          ↓
Understand its conventions and reasons
          ↓
Work consistently with what is already there
          ↓
Identify concrete problems, not only personal preferences
          ↓
Propose improvements to the team
          ↓
Adopt and migrate deliberately when there is agreement
```

This does not mean that an existing project should never change. Existing conventions can be incomplete, outdated, or harmful. However, improvements should be conscious architectural decisions rather than isolated personal decisions.

#### Understand Before Changing

Before creating or modifying code in an existing project, inspect similar parts of the codebase and understand:

1. How files and folders are organized.
2. How classes, functions, variables, and database objects are named.
3. Where business logic is placed.
4. How errors and exceptions are handled.
5. How dependencies are created and passed.
6. How data moves between layers.
7. Which patterns are already used by the team.
8. Which formatting and linting tools are enforced.

Do not assume that a convention exists without a reason. Some decisions may reflect technical constraints, framework conventions, deployment requirements, backward compatibility, or previous team experience.

At the same time, do not assume that every existing decision is correct. First understand it; then evaluate it.

#### Distinguish Problems From Preferences

A developer should distinguish between:

```text
A concrete project problem
    High coupling, repeated defects, unclear ownership,
    difficult changes, poor performance, or security risk.

and

A personal preference
    A different folder name, syntax style, framework,
    pattern, or paradigm that the developer likes more.
```

A concrete problem can justify a project-level change. A personal preference usually does not justify creating inconsistency.

The burden of proof belongs to the proposed change. The developer proposing a new convention should explain the problem, the target quality, the trade-offs, and the migration path.

#### Prefer Project Consistency by Default

When the existing convention is reasonable and no broader change has been agreed upon:

1. Follow the naming style already used by the project.
2. Place new files where equivalent files are already placed.
3. Use the existing architectural boundaries.
4. Follow the established approach to dependencies and data flow.
5. Match the surrounding formatting and code style.
6. Avoid rewriting unrelated code only to match personal preferences.
7. Keep the scope of each change focused and understandable.

Consistency reduces cognitive load. A developer can learn one set of expectations and apply it across the codebase.

#### Change Conventions Deliberately

When a convention genuinely needs to change, the change should be explicit and coordinated.

A deliberate convention change should normally include:

1. A clearly described problem.
2. The axis and quality attribute being improved.
3. The expected benefits and trade-offs.
4. Agreement from the responsible team members.
5. A migration strategy for existing code.
6. Documentation and examples of the new convention.
7. Automated rules where possible.

During a migration, temporary inconsistency may be unavoidable. It should be controlled by a clear boundary and a known direction, not created randomly.

For example, a team may decide that all new modules use a feature-based structure while old modules are migrated one by one. In that case, the boundary and migration rule must be documented so that the project does not become permanently divided between two accidental styles.

#### Formatting Rules

After understanding the project's existing conventions, use the following rules where they do not conflict with an established team standard:

1. Keep files focused and reasonably small. A common guideline is about 200 lines per file and no more than 500, but responsibility and clarity are more important than an exact number.

2. Avoid unnecessarily long lines. A common limit is 120 characters unless the project defines another standard.

3. Use blank lines to separate logical sections such as package declarations, imports, properties, and functions.

4. Keep instance variables in a consistent and easy-to-find place.

5. Place local variables as close to their usage as reasonably possible.

6. If one function calls another, keep them vertically close when this improves the reading flow, with the caller normally above the called function.

7. Keep related functions close to each other.

8. Eliminate unnecessary alignment that draws attention to formatting rather than meaning.

9. Use whitespace around operators where it improves readability, and remove unnecessary whitespace, especially at the end of a line.

10. Use the project's formatter, linter, editor configuration, and static-analysis rules rather than manually applying a different personal style.

11. **Team rules override individual preferences.** A team should agree on a consistent style, document it, automate it where possible, and apply it throughout the project.

The final objective is not for every developer to leave their personal style in the codebase.

The objective is for the codebase to read as if it were written by one coordinated team.

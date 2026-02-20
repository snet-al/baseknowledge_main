### Clean Code

One of the best names the Uncle Bob give to the developers is authors. We are authors.
And one thing of authors is that they have readers, so is our responsibility to
communicate clearly with our readers. This responsibility is higher when we enter
the agile and scrum environment where the main concept is the team, and collaboration.
<br>
But its not enough to write the code well, but it has to be kept clean over time.
We’ve all seen code rot and degrade as time passes. So we must take an active role in preventing this degradation.
So the first rule of clean code is:

> If we all checked-in our code a little cleaner than when we checked it out, the code simply could not rot. The cleanup doesn’t have to be something big.
> Change one variable name for the better.

### Part 1. Naming is the root of programming   

> The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

1. Explicitly<br>
   As cited in the clean code book, the variables for example should not be clean as in
   simple to see:
   <br>
   `$a = 1`
   <br>
   but must be explicit as:
   <br>
   `$statusOfGame = STATUS_ONE`

2. Avoid disinformation<br>
   Avoid short names, and words which have more than one meaning:
   `hp, aix, and sco`
   Do not refer to a grouping of accounts as an `accountList` unless
   it’s actually a `List`. The word `list` means something
   specific to programmers.
   If the container holding the accounts is not actually a `List`,
   it may lead to false conclusions.
   So `accountGroup` or `bunchOfAccounts` or just plain `accounts` would be better.

   `Redundancy of names`: inside a class User its not neccesary to declare variables like : `userSurname` , `surname` is enough.
   Noise words are redundant. How is NameString better than name? Would a Name ever be a floating point number? 
   If so, it breaks an earlier rule about disinformation.
   Distinguish names in such a way that the reader knows what the differences offer.


3. Use searchable names<br>
   With the use of IDE the work of programming is transformed a lot in searching
   code already written, so to help in this process is better to think in advance
   and name our variables, but mostly our functions in a searchable way.

4. Class Names should be nouns and not verbs. Avoid words like Manager, Processor, Data, or Info in the name of a class.

5. Method names should be verbs.

6. Use Pronounceable Names


### Part 2: Multi-dimensional Space of Principles

> The principles of software development are a set of guidelines that help developers create software that is easy to maintain, extend, and understand. These principles are based on the idea that software should be designed in a way that allows for flexibility and adaptability over time. By following these principles, developers can create software that is more robust and less prone to bugs and errors.

Consider the concept of principles as some directions on a multi-dimensional space. Each directon is a paradigm, or layer of abstraction which tries to see the software prespective.
For example one view of software is the OOP view. This view tries to see the software as a translation of the real world, into objects, and interacions with them.
When we are taking into consideration this view (direction) we should only view every detail of our review of the code based on this view, and not to mix it with other views, for example the functional programming view, which is another direction, and which tries to see the software as a composition of functions, and the interactions between them. So when we are looking to the code we should try to see it from one direction, and not to mix it with other directions, because this will create a mess in our review, and we will not be able to see the problems of the code, and how to fix them.

There are always multiple dimensions to look at the code, and each dimension has its own principles, so we should try to look at the code from different dimensions, and apply the principles of each dimension to fix the problems of the code.

I will give another exaple of two different dimensions:
a) The axis is "reason to change"(CCP, Common Closure Principle). This direction checks the code only based on the reason to change. This view tells you to put on the same folder Clasess that are related like: UserService, UserController, UserRepository, and to put in another folder the classes related to the product like: ProductService, ProductController, ProductRepository. This is a good way to organize the code because it makes it easier to find the classes related to a specific reason to change.
b) The axis is "type of change". This direction checks the code only based on the type of change. This view tells you to put on the same folder all the services, all the controllers, and all the repositories. This is a good way to organize the code because it makes it easier to find the classes related to a specific type of change.
Both these view have their own advantages and disadvantages, and we should try to use both of them to organize our code in a way that makes it easier to maintain and extend over time.
As you can deduce these are simingly different directions, and they are, but this is the concept of multi-dimensional space, and the more dimensions we have the more we can see the code from different perspectives, and we need to decide which perspective is more important for us in a specific moment, and to apply the principles of that perspective to fix the problems of the code.


SOLID principles are a set of five design principles that help software developers create maintainable and scalable software. The SOLID principles are:
1. Single Responsibility Principle (SRP): A class should have only one reason to change, meaning it should have only one responsibility or job.
2. Open/Closed Principle (OCP): Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means that you should be able to add new functionality to a class without changing its existing code.
3. Liskov Substitution Principle (LSP): Subtypes must be substitutable for their base types. This means that objects of a derived class should be able to replace objects of the base class without affecting the correctness of the program.
4. Interface Segregation Principle (ISP): Clients should not be forced to depend on interfaces they do not use. This means that you should create specific interfaces for different clients rather than a single general-purpose interface.
5. Dependency Inversion Principle (DIP): High-level modules should not depend on low-level modules. Both should depend on abstractions. This means that you should depend on abstractions rather than concrete implementations, and that you should use interfaces or abstract classes to decouple high-level modules from low-level modules.


### Part 2. Functions

First rule of functions is that they should be small
<br>
The second rule is that they should be smaller than that

1. Block and indention
   <br>
   Functions should have at most two levels of indentions

2. Functions should do **one** thing
   <br>
   Avoid that one functions does two things, for example like calulating the value and printing it.

3. One level of abstraction per function
   <br>
   Statements in a functions should be at the same level of abstraction

4. Stepdown rule
   <br>
   We want to read the code like a top-down narrative

5. Number of arguments
   <br>
   The ideal number of arguments for a function is 0.
6. Prefer Exception instead of returning error codes
7. DRY principle (Dont repeat yourself)
8. Structured programing
   <br>
   Structured programing as specified by Dijkstra, says that every function, or even a block within a function should have a single entry point and a single exit. Following this rule a function should have a single `return`statement, no `break` or `continue` on `for` statements, and never a `goto`.
   Every communication of a function with other functions should occur only once inside a function. For example FizaBuzz app:

   First solution: 
   ```
   for (let i = 1; i < 100; 1++) {
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

   This is the solution after a lot of debbuging and errors, but let have some review here:
   if we take a moment and look to condition 1, and we ask if its possible to determine if that condition is checked by only viewing that line the answer is no. The reason is that the code (this part of the code) is "dependent"  by another piece of code which in this case might be small but imagine in a more complex code and with more files.
   So the first step in creating a structured code is to remove the dependency parts.
   
   So the second solution is to remove the `else` :
   ```
   for (let i = 1; i < 100; 1++) {
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

   When you check this solution you might think at first that is more complex than the first one, but the filozofy of clean code is to concentrate only on one concept at one time , fix that and than go to the next concept. Cleaning the code is an incremental process.

   Now we want to structure more the code so the next phase is to use variables to help us.

   Solution 3:
   ```
   for (let i = 1; i < 100; 1++) {
      let toPrint = "";

      if (i % 3 === 0) {
         toPrint ="Fizz"
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

   The importance of this usage of variables is the separation of funciton calls, as we noticed we have a console.log function which in our code should be considered an external communication, so we have to call external functions as little as possible.


### Part 3. Formatting
Code formatting is important. Code formatting is about **communication**, and communication is the professional developer’s first order of business.



1. Keep files small, which translates to about 200 lines per file (and no more than 500)

2. Don't make lines longer than 120 characters

3. Set blank lines to separate package declaration, the import(s), and each of the functions.

4. Instance variables should be grouped into an easy-to-find, well-known place.

5. Variables should generally be placed as close to their usage as possible.

6. If one function calls another, they should be vertically close, and the caller should be above the called. 

7. Keep functions close if they are related.

8. Eliminate unnecessary, unhelpful alignment that draw your attention to the wrong thing.

9. You’ll want to have whitespace between operators for readability. Otherwise, eliminate that whitespace, especially at the end of a line.

10. **Team Rules**. A team of developers should agree upon a single formatting style, and then every member of that team should use that style.

### Part 4. Testing
1. The Three Laws of TDD
   - First Law You may not write production code until you have written a failing unit test.
   - Second Law You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
   - Third Law You may not write more production code than is sufficient to pass the curently failing test.
2. Test code is important, it must be kept as clean as production code.

3. Clean tests follow five other rules. (F.I.R.S.T)\
   **Fast**. Tests should be fast. They should run quickly.\
   **Independent**.You should be able to run each test independently and run the tests in any order you like.\
   **Repeatable**.Tests should be repeatable in any environment. \
   **Self-Validating**.The tests should have a boolean output. Either they pass or fail. You should not have to read through a log file to tell whether the tests pass.\
   **Timely**.Unit tests should be written just before the production code that makes them pass.\


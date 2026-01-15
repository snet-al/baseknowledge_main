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

### Part 1. Meaningful Names

> The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.
Naming is the base of programing.
- Why

When asking why a name exist, we are asking first its `layer of abstraction`.
For example: PersonEntity, (or simply Person in some cases) -> tells that its a domain logic so tells us what should it know. If in that class we add `SQL` meand that the class know also another thing other then domain, so we are violating the rule of SRP.

- What

Also what a variable, class, etc etc, by his name should tell us what it does.

- How

This is combined with Why, so we have as much knowledge as possible on what insights the content of a variable, class, function should have.
  

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


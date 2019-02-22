# Clean Code - Reading Notes

## Chapter 1 - Clean Code

### There Will Be Code

* There will always be code, because it is ultimately the language that expresses the requirements.
* All high level models and requirements and specifications ultimately will be code.

### Bad Code

* Bad code can bring down companies. (An anonymous example is given.)
* Being impeded by bad code is known as _wading_.
* Bad code is written in a rush, with the idealism that it will be fixed later.
  * LeBlanc's law: _Later equals never_.

### The Total Cost of Owning a Mess

* As code gets more knotted and tangled, it cannot be cleaned up at all.
* Productivity asymptotically approaches zero.

### The Grand Redesign in the Sky

* In many cases, development teams have had enough of old code bases and demand a redesign.
* This leads to a race -- the new team must build a system that does everything the old one did, _and_ keep up with changes to the old system.
* This race can take 10 years. And can often be cyclical.

### Attitude

* We often blame bad code on requirements changes, management, customers, and timelines.
* It is our fault.
* Managers look to us for information they need. Even if we feel it risks our careers to push back. It's _our job_ to defend the code.
* Doctor analogy: Imagine if a patient demanded a doctor stop washing their hands because it was taking too much time.
  * It would be unprofessional for the doctor to bend to the patient's whims.
  * It is likewise unprofessional for programmers to bend to the will of managers who don't understand the risk of making a mess of code.

### The Primal Conundrum

* Previous messes slow us down, but we feel pretty obligated to make messes in order to meet deadlines.
* We don't take the time to go fast.
* In reality, you won't make the deadline by making a mess. It will slow you down instantly.
* The only way to go fast is to keep the code as clean as possible at all times.

### The Art of Clean Code

* Writing clean code is like painting a picture. Everyone can tell if a picture is painted well or poorly. But not everyone knows how to paint.
* Writing clean code requires a painstakingly acquired sense of cleanliness, also known as _code-sense_.
* A programmer with _code-sense_ can look at a messy module and see variations that would be an improvement.

### What is Clean Code?

* (The author goes through a series of quotations from famous developers to paint a picture.)
* Bjarne Stroustrup: Code should be elegant, with complete error handling and do one thing well.
  * Clean code is focused.
    * Broken window analogy: A building with broken windows will get more windows broken as people stop caring.
      * Bad code tempts the mess to grow.
* Grady Booch: Clean code is simple and direct, reading like prose. Intent is clear. Crisp abstractions.
* Dave Thomas: Clean code should be easy for other developers to enhance. Minimal. Unit and acceptance tests.
* Michael Feathers: Clean code looks like it was written by someone who cares. Nothing obvious to do to improve it.
* Ron Jeffries: No duplication, do one thing, expressiveness, tiny abstractions.
* Ward Cunningham: Each routine does what you expect it to. Makes it look like the language was written for the problem.

### Schools of Thought

* Uncle Bob's and his community's opinion will be presented as absolutes.
* Martial art analogy: While no school of martial arts is right, within a particular school you act as though the teachings are right.

### We Are Authors

* Remember that you are writing for an audience.
* Ratio of time spent reading over writing code: 10 to 1.
* Make the code easy to read -- it makes it easier to write.

### The Boy Scout Rule

* Leave the campground cleaner than you found it.
* Code rot can be fought by just making tiny improvements as you go.
* Imagine code _getting better_ as time passes. This principle embodies professionalism.

### Prequel and Principles

* Uncle Bob wrote _Agile Software Development: Principles, Patterns, and Practices_. (PPP)
* He'll refer to concepts that come from that book.
  * Single Responsibility Principle (SRP)
  * Open Closed Principle (OCP)
  * Dependency Inversion Principle (DIP)
  * And more!
* Uncle Bob implies you should refer to the PPP book.

### Conclusion

* The book will show us the door. We will have to walk through it.
* Practice.

## Chapter 2 - Meaningful Names

### Introduction
* Names are everywhere and therefore important.

### Advice

* Use intention-revealing names.
  * Ex. `int d` vs `int daysSinceModification`.
* Avoid disinformation.
  * Ex. `accountList` better be an actual list.
* Make meaningful distinctions.
  * Ex. Avoid `a1`, `a2`. Avoid noise words: `ProductInfo` vs `ProductData`.
* Use pronouncable names.
  * Ex. `genymdhms` vs `generationTimestamp`.
* Use searchable names.
  * Short names are hard to search. Name constants. Length of name should correspond to the size of its scope.
* Avoid encodings.
  * Hungarian notation - No need to encode the type in the name anymore. Can avoid errors with type being changed without the name being updated.
  * Member prefixes - No need for m_ anymore. Functions that they are in should be small anyway. IDEs will just highlight them for you.
  * Interfaces and implementations - Avoid the I prefix. If you must, encode the implementation, not the interface.
* Avoid mental mapping
  * The reader shouldn't have to mentally translate your names into other names they know. Single letter names can be a problem.
* Class names
  * Use nouns. Probably capitalized.
* Method names
  * Use verbs or verb phrases.
  * ***Accessors, mutators, and predicates should be named for their value and prefixed with get, set, and is according to the javabean standard.***
  * ***When constructors are overloaded, use static factory methods with names that describe the arguments.***
    * Ex. `Complex.FromRealNumber(23.0)` is better than `Complex(23.0)`.
* Don't be cute.
  * `HolyHandGrenade` -- people won't remember what it does. Say what you mean, mean what you say.
* Pick one word per concept.
  * Ex. fetch/retrieve/get -- pick one and stick to it.
* Don't pun. (Sorry Alex)
  * Avoid using the same word for two purposes.
  * Ex. If you use the word `add`, make sure the addition it does is always the same. (Don't have addFunc1 concatenate two values and addFunc2 add a value to a collection. That's too different.)
* Use solution domain names.
  * Don't be afraid to use technical names. `AccountVisitor` for the visitor pattern. `JobQueue` for a job queue.
* Use problem domain names.
  * For things that aren't technical, use problem domain names so domain experts can understand them.
* Add meaningful context.
  * Some words don't mean much by themselves. `state` could be part of an address, or it could not.
  * Grouping variables that are related to each other into a class can help and make the code cleaner.
* Don't add gratuitous context.
  * Don't prefix unnecessary. Makes autocompletes useless.
  * Shorter names are better. Just as long as they're still clear.
  * Generic names for classes are OK. Just make their instances concrete.

### Conclusion

* Don't be afraid of renaming things.

## Chapter 3 - Functions

### Small!

* Functions should be small. And then smaller than that.
* No more than 20 lines. Ideally two, three, or four.

### Blocks and Indenting

* The level of indent on a function should not be greater than one or two.

### Do One Thing

* Function should only do one thing, and do it well.
* What does "one thing" mean? 
  * It means only doing the steps one level below the stated name of the function.
  * You can use a "TO paragraph" to describe it.
    * Ex. "TO RenderPageWithSetupAndTeardowns, we check if the page is a test page, and if so, blah blah."
  * If a function is doing more than one thing, you can extract another function from it that isn't just a restatement of its implementation.

#### Sections within Functions

* If a function can be divided into multiple sections, then it is doing more than one thing.

### One Level of Abstraction per Function

* Making sure functions are all at the same level of abstraction helps ensure they are only doing one thing.

#### The Step-down Rule

* Code should read like a top-down narrative, getting lower and lower in level of abstraction.
* It should read like a series of "TO paragraphs".
  * To do X, we do Y and Z. To do Y, we do Y1. To do Z, we do Z1. To do Z1, we do Z2.
  
### Switch Statements

* It's hard to make small switch statements.
* It's hard to make switch statements to one thing. (Since they tend to do N things.)
* They can't be avoided, but they can be buried in a low-level class with polymorphism to avoid repetition.
* Best way: Make an abstract factory, which uses the switch statement to create the appropriate types objects.
  * Example given in the book: Different types of employees with different ways of calculating pay.

### Use Descriptive Names

* Choose a good name and be consistent.

### Function Arguments

* Ideal number of arguments: zero.
  * Followed by one.
  * Followed by two.
  * Followed by three (try to avoid).
  * More than three require special justification.
  * Names: Niladic, monadic, dyadic, triadic, polyadic.
* Arguments make reading and testing harder.
* Output arguments are even harder.
* Single arguments are best.

#### Common Monadic Forms

* Asking a question about the argument. Ex. fileExists(myFile)
* Operating on an argument and transforming it. Ex. fileOpen(myFile)
* Less common: A function receiving an event and changing system state. Be clear with these.
  * Ex. void passwordAttemptFailedNtimes(int attempts)
* Avoid other forms, especially output arguments.

#### Flag Arguments

* Ugly. Passing a boolean into a function is a bad practice.
  * It announces the function does more than one thing.
* Split these up into two functions.

#### Dyadic Functions

* Harder to understand than monadic functions.
* Ordering can be confusing.

#### Triads

* Even harder.

#### Argument Objects

* Grouping arguments into classes/objects is a good thing. (x, y, radius) vs (Point center, radius)

#### Argument Lists

* An argument list counts as a single parameter of type list.

#### Verbs and Keywords

* Monads should be written as a verb/noun pair. Ex. write(name)
* _Keyword_ form of a function name: encoding the names of arguments in the name.
  * Ex. assertEquals vs assertExpectedEqualsActual(expected, actual).
  
### Have No Side Effects

* Side effects are lies. Your functions should do one thing, not do additional hidden things.
* Side effects can create temporal couplings, which are confusing.

#### Output Arguments

* Just avoid them now. They're not needed anymore. 
  * If you have to change state, change the state of its owning object.

### Command Query Separation

* You function should either do something or answer something. Not both.
  * Either change state or report state.

### Prefer Exceptions to Returning Error Codes

* Error codes are a violation of command query separation.
* Error codes force the caller to deal with the code immediately.
  * Better to use exceptions to separate happy path.

#### Extract Try/Catch Blocks

* They're ugly. It's better to extract their bodies into their own functions.

#### Error Handling Is One Thing

* A function that handles errors should do nothing else.

#### The Error.java Dependency Magnet

* Using error codes implies they're stored in a class or enum.
  * This would be a _dependency magnet_, since it'd need to be included in lots of places.
    * Literally leading to rebuilds if there's a change. This would sometimes cause programmers to reuse error codes.
* Exceptions are better, since they are derivatives of the exception class. No recompilation or redeployment needed.

### Don't Repeat Yourself

* Duplication is the root of all evil in software. Most innovations fight it.
  * Examples: Normal forms, object oriented programming, structured programming, even subroutines.

### Structured Programming

* Some, like Dijkstra's, believe that every function and every block should have one entry and one exit.
  * One return statement, no breaks or continue statements, and never any gotos.
* Author agrees, but for small functions, it's not a big concern. It can sometimes be more expressive to break these rules.
  * Except gotos. Raptors will eat you.

### How Do You Write Functions Like This?

* Start messy, then massage it into better forms. It's OK for them to not be perfect to start with.

### Conclusion

* Functions are verbs, and classes are nouns.
* Master programmers think of systems as stories to be told, not programs to be written.
  * Functions are a major part of this narrative.
* Make your functions short, well named, and nicely organized.
* Never forget that your real goal is to tell the story of the system. Your functions must be clear and precise.

## Chapter 4 - Comments

### Introduction

* Comments are not intrinsically good.
* They are a way to compensate for failing to express ourselves in code.
* Comments can diverge from code. They can lie.
* Only the code speaks the truth.

### Comments Do Not Make Up For Bad Code

* If you see bad code, instead of commenting it, clean it up.

### Explain Yourself In Code

* Clean code can be easier to read than some comments. Uncle Bob gives an example.

### Good Comments

* Legal header comments
* Informative comments (Example: Showing the format matched by a pattern in a clear way)
* Explanation of intent behind a decision
* Clarification (for code that cannot be mad easier to read -- Example: A cluster of a.compareTo(booleanExpressions))
* Warning of consequences (a long unit test, thread safety)
* TODO Comments - Keep these to a minimum. (_I personally disagree here -- I think TODOs should be avoided altogether._)
  * Put these as tickets in JIRA / your program of choice.
* Amplification: To show why something is important
* Javadocs in a Public API

### Bad Comments

* Mumbling - unclear comment that serves no purpose
* Redundant comments - comments that describe the code but are no easier to read than the code
* Misleading comments
* Mandated comments - don't require javadocs for non-public APIs
* Journal comments - these were OK before source control
* Noise comments - these state the obvious ("Default constructor")
* Scary noise - Pointless header comments above variables that are copy-paste error prone
* Don't use a comment when you can use a function or variable
* Position banners - use these sparingly or people will tune them out
* Closing brace comments - no need to do this. Make your functions shorter instead
* Attributions and Bylines - Source control can take care of this
* Commented-out code - This is a very bad practice. People may hesitate to delete it, thinking it's important.
  * Delete and let source control remember it
* HTML comments - an abomination. Unreadable. It should be the javadoc consuming tool and inserts HTML, not your comment.
* Nonlocal information - Don't give system-wide information in a local comment. Keep it relevant.
* Too much information - You don't need to put a full RFC in code. Just the RFC number should do.
* Inobvious connection - Just be clear. Don't cause _more_ confusion with comments.
* Function headers - Short functions with well-chosen names don't need function headers.
* Javadocs in nonpublic code - Don't do this.

## Chapter 5 - Formatting

* You want your code to look like it was written by a group of professionals, not drunken sailors.

### The Purpose of Formatting

* Formatting is more important than getting it working. Your code style may long outlive its functionality.

### Vertical Formatting

* Based on comparative data of several projects, files shouldn't be much longer than 200-500 lines.

### The Newspaper Metaphor

* Just like reading newspaper headlines, we want the name of a file to be simple and explanatory. And as we go deeper down the page, you get to lower-level details. This makes it usable.

### Vertical Openness Between Concepts

* Each group of lines should represent a clear thought. Separate these with blank lines.

### Vertical Density

* Lines of code that are closely associated should be vertically dense.

### Vertical Distance

* It sucks to hunt around a file. Things that are closely related should be near each other.

#### Variable Declarations

* Variable declarations should be near their usage.
* Since our functions are small, local variables should appear at the top.
* Control variables for loops should be declared within the loop statement.
  * Sometimes you have to declare them at the top. That's OK.
* Instance variables should be at the top of the class. 
  * Back in C++, with the _scissors rule_, instance variables would go at the bottom.
  * In Java, it's at the top. Uncle Bob prefers that.
    * He gives an example of instance variables hidden between two functions in JUnit, yuck!

#### Dependent Functions

* They should be close to each other.
* The caller should be above the callee -- it gives the program a natural flow.

#### Conceptual Affinity

* If they're related, make the vertical distance short between two items.

#### Vertical Ordering

* Function call dependencies should point in the downward direction.
  * The function that is called should be below the function that does the calling.
    * _This is the opposite of some languages: Pascal, C, and C++_
  * This is because we expect low level details to come last, like reading the newspaper.

### Horizontal Formatting

* How wide should a line be?
  * The old limit was 80. Now 120 is fine.

### Horizontal Openness and Density

* Whitespace horizontally is useful for grouping related things (parameters in a function) and separating things (the left and right sides of an assignment operation).
* It's also useful for highlighting the precedence of operators.
  * Watch out for tools demolishing these choices.

### Horizontal Alignment

* In the assembly days, would one align variable identifiers horizontally in table-like structures.
  * Not needed anymore. A code smell that the list should be shorter.

### Indentation

* Done to show scope in blocks. Standard fare. Keep indenting.
  * Uncle Bob does not declare a space vs tab preference here. :)
* Even when dealing with a potential one liner if statement, he feels it's best to stick to indentation and not collapse them into one line.

### Dummy Scopes

* If you have a blank line, like a while loop with nothing in it, put the semicolon in a separate line to make it visible.

### Team Rules

* Your code should not look like a jumble of different styles.
* Make a team standard and stick to it. Configure IDEs and linters are appropriate.

### Uncle Bob's Formatting Rules

* He gives a code sample with his coding style represented.

## Chapter 6 - Objects and Data Structure

* Programmers often want private variables but ruin them by creating getters and setters.

### Data Abstraction

* Hiding implementation isn't just about putting a layer of functions between variables -- it's about abstractions.
* One should expose abstract interfaces that manipulate the essence of the data without us having to know its implementation.
  * Example: A Point class with an x and y vs a Point class with various functions to get polar and cartesian coordinates.
  * Example: A Vehicle class reporting its actual fuel vs reporting an abstracted percentage.
* Think hard about getters and setters. There's probably a better abstraction.

### Data/Object Anti-Symmetry

* An _object_ hides its data behind abstractions and exposes functions that operate on that data.
* A _data structure_ exposes its data and has no meaningful functions.
* Therefore, objects and data structures are opposites.
  * Example: A Geometry object-oriented class with simple shape data structures (Square, Rectange, and Circle).
    * It has a few methods, like area. It checks the current class to see how it computes the area.
    * Adding a new function is easy -- doesn't affect the shape classes.
    * Adding a new shape is hard -- all of the functions in the class have to be updated.
  * Example: Classes for the data structures with a polymorphic area function from implementing the Shape interface.
    * No Geometry class is necessary.
    * Adding a new shape is easy -- no existing functions are affected.
    * Adding a new function is hard -- all of the shapes must be changed to include its polymorphic implementation to fulfill the interface.
      * There are ways around this with the visitor pattern or dual-dispatch, but they have their own costs and make the program procedural again.
* The above exposes the dichotomy between objects and data structures:
  * Procedural code (with data structures) makes it easy to add new functions without changing existing data structures. (The functions handle the details.)
    * OO code makes it easy to add new classes without changing existing functions. (The classes are responsible for implementing existing functions.)
* The complement of the above is also true:
  * Procedural code (with data structures) makes it hard to add new data structures because all functions must change. (Since the functions need to learn to handle a new class/object type.)
  * OO code makes it hard to add new functions because all classes must change. (Since they're responsible for implementing these new functions.)
* Sometimes you don't want everything to be an object. It depends on whether you need to add new data types or new functions more in the application.
	
### The Law of Demeter

* A module should not know about the innards of the objects it manipulates.
  * A method in a class should only call methods: of that class, of an object the method itself creates, of an object passed to the method, or of an object that's an instance variable of that class.
    * It should also not call methods on objects returned by any of the allowed methods.
      * "Talk to friends, not to strangers."

### Train Wrecks

* Chains of function calls.
  * Example: a().b().c().d()
  * Bad style. Avoid and break into multiple lines.
  * Violation of Demeter's Law if the functions are from objects exposing their innards.
  * If some or all were simple data structures, would be fine. Example: a.b.c.d

### Hybrids

* Sometimes you get hybrid structures that are half object and half data structure.
  * When mutators and accessors make private variables public, this tempts external functions to use those in a way a procedural program would.
    * Similar to "Feature Envy" concept.
* Hybrids are the worst of both worlds: hard to add new functions and hard to add new data structures.

### Hiding Structure

* It's sometimes best to question why there's a train car. 
  * Example: a().doE() may be a better solution than a().b().c().d() if we only needed b, c, and d to accomplish e.

### Data Transfer Objects

* A class with public variables and no functions. 
  * Useful when communicating wiht databases or parsing messages from sockets.
* "Beans" are a common alternative form. They have private variables manipulated by getters and setters.
  * Uncle Bob: This quasi-encapsulation makes OO purists feel better but usually provides no other benefit.

### Active Record

* Special DTOs that have navigational methods like save and find.
* Don't put business rule methods in them -- creates a hybrid.
* Best to keep these as data structures and create separate objects that have the business rules and hides its internal data.

### Conclusion

* Objects expose behavior and hide data.
  * Easy to add new objects without changing existing behavior.
  * Hard to add new behavior to existing objects.
* Data structures expose data and have no significant behavior.
  * Easy to add new behaviors to existing data structures.
  * Hard to add new data structures to existing functions.
* Depending on the system, we either want flexibility to add new data types (prefer objects) or new behaviors (prefer data types and procedures).
* Good software engineers understand these issues and use the best approach for the job at hand.

## Chapter 7 - Error Handling

* If error handling obscures logic, it's wrong.
* Use exceptions instead of return codes - it detangles concerns.
* Write your try-catch-finally statement first
  * Think of them as transactions
  * Start with tests that force exceptions
* Use unchecked exceptions
  * Otherwise, the Open/Closed Principle is violated
    * This is because a low level change can force signature changes at higher levels
* Provide context with exceptions
  * Not just stack traces -- any pertinent information to help debugging
* Define exception classes in terms of the caller's needs
  * Wrap a library that can throw three different exceptions so it only throws one instead
    * The information sent with the exception can be used to distinguish
* Define the normal flow by using the special case pattern
  * Wrap what can throw an exception and handle it yourself
  * That way client code doesn't have to deal with the exceptional behavior at all
* Never return null -- null checks are error prone.
* Don't pass null -- it causes run time errors and is difficult to guard against.

## Chapter 8 - Boundaries

* Third-party code often has more than the client needs.
* It is best to wrap third-party code in an interface to shield the rest of the codebase from breaking changes or unneeded features.
* Robert Martin recommends writing tests to learn how to use third-party frameworks.
  * Jim Newkirk calls these _learning tests_.
  * Robert Martin refers to them as controlled experiments. They should be written the way the software is expected to use the library.
* Robert Martin provides log4j's ConsoleAppender as an example.
* Learning tests are "better than free", since they can prove the library still works after a major update.
  * Even if the learning tests are not needed, Robert Martin advocates _boundary tests_ for any clean boundary.
* Writing interfaces for code that does not exist helps focus client code on what is it trying to accomplish.
  * This can create a convenient seam for test code.
  * Robert Martin provides a Transmitter API example, where he later wrote an adapter once the target API came into being.
* Keep boundaries clean. You want to depend on code you wrote, no others.
  * Wrap boundaries, and use an adapter to convert from your perfect interface to the provided interface.

## Chapter 9 - Unit Tests

Robert Martin recounts his Timer class testing story, including the sing-a-long. :)

### The Threw Laws of TDD

1. You may not write production code until you have written a failing unit test.
2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
3. You may not write more production code than is sufficient to pass the currently failing test.

This will lead to thirty-second loops, and thousands of tests per year. This can be a maintenance problem.

### Keeping Tests Clean

* Robert Martin recounts a story of a team that had "quick and dirty" tests.
  * This is problematic since test code has to change as production code evolves.
  * Eventually dirty tests because a liability, get abandoned, and the entire project suffers.
* Test code is just as important as production code.

### Tests Enable the -ilities

* Unit tests make your code more flexible, maintainable, and reusable because they make you not fear change.

### Clean Tests

* Tests must be readable. Abstract away annoying details.
* Follow the Build-Operate-Check pattern.

### Domain-Specific Testing Language

* A testing language develops as you refactor tests to be readable -- they're a specialized API used by just your tests.
* Not necessarily designed up-front -- it evolves over time.

### A Dual Standard

* It's OK for test code to be less efficient if readability is the trade-off.

### One Assert per Test

* Try to keep the number of asserts per test low.
* Given-when-then is a common pattern for tests.
* The Template Method pattern is recommended.

### Single Concept per Test

* This is the actual rule Robert Martin prefers -- just test one concept per test function.

### F.I.R.S.T

Tests should be:
* Fast (so you want to run them frequently)
* Independent (not depend on each other)
* Repeatable (even offline on a train on a laptop)
* Self-Validating (return pass or fail)
* Timely (written before production code)

## Chapter 10 - Classes

### Class Organization

* Classes should begin with a list of variables.
  * Public static constants first.
  * Then private static variables.
  * Then private instance variables.
  * You really shouldn't have public variables.
* Functions should then follow
  * Public functions first.
  * Private utility functions after the public functions that use them.
    * This follows the step-down rule.

#### Encapsulation

* If you need to make a private function protected for test purposes, that's cool. Tests rule.

### Classes Should Be Small!

* First rule: Classes should be small.
* Second rule: Classes should be smaller than that.
* With functions, you count lines. With classes, you count responsibilities.
  * A class that does everything is called a God class.
* A concise class name is a good sign that a class is small enough.
* Should be able to write a brief description of the class without "if", "and", "or", or "but".
* Follow the Single Responsibility Principle (SRP).
  * A class or module should have one, and only one, reason to change.
* Why are so many classes too big?
  * We have two responsibilities: Get it to work and make it clean.
  * They are separate steps, but often we move on to the next task before cleaning the current.
* Some people fear small classes make it hard to understand the big picture.
  * The reality is that the system still needs to be learned either way.
  * Do you want your tools organized into neat small drawers or everything just tossed into a big drawer?

### Cohesion

* A class is maximally cohesive if all of its instance variables are used by all of its methods.
  * You want to aim for high cohesion.
    * If it's not high, there may be a second class lurking in that first class.
    * When classes lose cohesion, split them. Aiming for cohesion leads to smaller classes.

### Organizing for Change

* Split your classes up so a change in the future only touches a small class. Less chance for large defects.
  * SQL class example being split into InsertSql, SelectWithCriteriaSql, etc.

### Isolating from Change

* Try to depend on abstrations, not concretions.
  * Example of a Portfolio class depending on an interface, StockExchange, instead of TokyoStockExchange.
  * Easier to test.
  * More reuse.
  * This is indeed the Dependency Inversion Principle (DIP) at work.

## Chapter 11 - Systems

* A neat quote about complexity from the Microsoft CTO. "Complexity kills."

### How Would You Build a City?

* Wouldn't manage all the details youself -- teams of people who manage a specific part of the city.
  * Some are responsible for the big picture, but some focus on the deails.
* Clean code helps with lower levels of abstraction, but it's important to stay clean at the _system_ level too.

### Separate Construction of a System from Using It

* The construction phase is separate from the use phase.
  * New hotel in Chicago analogy. Building it vs using it.
* The startup process for an application is when you construct objects and wire up their dependencies.
  * This _concern_ should be separated from others. Many applications skimp on this.
* Avoid lazy initialization/evaluation. (This is having a method, if a variable is null, construct an object for the first time before returning.)
  * This creates a hard-coded dependency which could be hard to test. Violates SRP too.
  * The global setup _strategy_ should not be _scattered_ across the application.

### Separation of Main

* One strategy is to have all aspects of construction move to main, and the rest of the system assumes all objects have been constructed and wired up.

#### Factories

* Another strategy is to have main create a factory, then the application can choose when the construction is done without needing to know _how_ it's done.

#### Dependency Injection

* Using Dependency Injection (DI) and the application of Inversion of Control (IoC) moves secondary responsibilities from an object to other objects.
  * Instead of instantiating a dependency itself, an "authoritative" mechanism can take care of it for the object.
  * This is a global concern, so either a main routine or a special-purpose _container_ will handle it.

### Scaling Up

* You wouldn't want a small village with a 6-lane highway.
* Implement today's stories and refactor and expand as needed.
  * TDD, refactoring, and clean code make this possible at the code level.
  * What about at the system level?
    * Doable, if you maintain the proper separation of concerns.
* Robert Martin gives an example of EJB1/EJB2 beans which have the business logic tightly coupled to the container.
  * Unit testing is hard, and reuse outside of EJB2 architecture was impossible.

### Cross-Cutting Concerns

* Some concerns cut across natural object boundaries, like persistence and security.
  * You should still encapsulate such a module, but handling the intersection of the domains is a problem.
* _Aspect-oriented programming (AOP)_ is a general purpose approach to restoring modularity for cross-cutting concerns.
  * In AOP, _aspects_ define which points in a system should have their behavior modified for a particular cross-cutting concern.
    * Persistence example: Declare which objects should be persisted and then delegate the tasks to the persistence framework.
      * Behavior modifications are made non-invasively (no manual editing of target source code) to the target code by the AOP framework.
* Three examples of similar-to-AOP Java frameworks:

#### Java Proxies

* An example is provided of a Bank class being wrapped by a Proxy class, which intercepts calls to Bank's methods and handles persistence concerns.
  * This isn't very clean because it's a lot of code. High volume and complexity.
  * No way of providing a mechanism for defining system-wide execution points of interest, which is what AOP needs.

#### Pure Java AOP Frameworks

* Spring XML configuration files to define "russian doll" decorators.
  * You think you're calling an inner object, but you're really calling it on the outermost _decorator_.
  * Just a few lines of code tie this to your project, so it stays pretty clean.
* EJB3 followed suit. You can either use XML configuration files, Java 5 annotations, or both.
    * An example is given with annotations, mapping the object to persistence-specific items. (Table names, one-to-many relationships, and so on.)

#### AspectJ Aspects

* They're the most "full-featured" way of separating concerns through aspects.
  * Robert doesn't go into too much detail.

### Test Drive the System Architecture

* Separate your concerns with aspect-like approaches.
  * You don't need to do a _Big Design Up Front_ (designing literally everything).
* An optimal system architecture is made up of modularized domains of concern, implemented with Plain Old Java (or other) Objects. (POJOs)
  * They are integrated together using minimally invasive aspects or aspect-like tools.
* This allows architecture to be test-driven, like code.

### Optimize Decision Making

* It's best to make decisions at the last possible moment. (To have more knowledge.)
  * POJO systems with modularized concerns make this possible.

### Use Standards Wisely, When They Add _Demonstrable_ Value

* Avoid standards when they lose touch with real needs or take too long to implement.

### Systems Need Domain-Specific Languages (DSLs)

* They allow the application components to be expressed as POJOs, both at a high and lower level.
  * This makes intent easier to read.

### Conclusion

* Systems should be clean too.
* Avoid invasive architectures -- they overwhelm domain logic and impact agility.
* Intent should be clear. Use POJOs and aspect-like mechanisms.
* Always use the simplest thing that can possibly work.

## Chapter 12 - Emergence

* Following Kent Beck's four rules of _Simple Design_ can lead to the emergence of good design.
* A design is simple if it:
  * Runs all the tests
  * Contains no duplication
  * Expresses the intent to the programmer
  * Minimizes the number of classes and methods
* Tests enable refactoring to improve design without fear.
  * We can:
    * Increase cohesion
    * Decrease coupling
    * Separate concerns
    * Modularize system concerns
    * Shrink functions and classes
    * Choose better names
* Three final rules of simple design:
  * Eliminate duplication
    * "Reuse in the small"
    * Template pattern can help
  * Ensure expressiveness
    * Good names, small functions, unit tests, and just try.
  * Minimize the number of classes and methods
    * Don't go too far with small classes and methods. There's a balance.

## Chapter 13 - Concurrency

### Why Concurrency?

* Decouple _what_ gets done from _when_.
* Performance -- parallelism where it makes sense.
* Concurrency is hard.

### Myths and Misconceptions

* Concurrency always improves performance.
* Design does not change when writing concurrent programs.
* Understanding concurrency is not important when working with containers.

### Challenges

* There are sometimes thousands of possible execution paths a JIT Compiler would take for any given code, and only _some_ of them would have concurrency issues.
* Concurrency issues can be written off as one-off errors.

### Concurrency Defense Principles

* Single Responsibility Principle
  * Since things should only have a single reason to change, concurrency design should be separated from the rest of the code, especially since its has its own lifecycle and challenges.
* Limit the Scope of Data
  * Have a few sections of data concurrent (using the synchronized keyword in Java) as possible.
* Use Copies of Data
  * Avoiding sharing data in the first place is wise.
* Threads Should Be as Independent as Possible
  * Write your threads as if they exist in their own world, with data coming in from unshared sources and stored as local data.

### Know Your Library

* Know which library classes are thread safe or not.
* Learn any thread-safe collections and tools.
* Use nonblocking solutions when possible.

### Know Your Execution Models

#### Key Terms

* _Bound Resources_: Resources of a fixed size used concurrently. Classic examples: Database connections, read/write buffers.
* _Mutual Exclusion_: Only one thread can access shared data at a time.
* _Starvation_: What can happen if only fast-running threads get to consume data. Slow-running threads may never get a chance.
* _Deadlock_: Two or more threads waiting on each other to finish.
* _Livelock_: Threads in lockstep, each getting in each other's way forever.

#### Producer-Consumer

* Producer threads place work in a queue.
* Consumer threads grab work from the queue and complete it
* The queue is a bound resource.
  * Producers can't put anything in if it's full.
  * Consumers can't start work until there's something in it.

#### Readers-Writers

* Throughput can be an issue.
* Concerns with readers reading something while the writer is updating.
* Needs to be balanced to avoid starvation.
* Depends on number of readers and writers and relative frequencies.

#### Dining Philosophers

* The classic example of philosophers sitting around a circular table, eating from a shared bowl of spaghetti, but each can only eat if they are holding two forks, and there is only one fork between each philosopher.

### Beware Dependencies Between Synchronized Methods

* Avoid using more than one method on a shared object.
* If you must, then either:
  * Use Client-Based Locking
  * Use Server-Based Locking
  * Use an Adapted Server

_(Reader's note: I do wish he went into more detail here.)_

### Keep Synchronized Sections Small

* Locks are expensive, so keep locked portions of code small.

### Writing Correct Shut-Down Code Is Hard

* Without graceful shutdown, deadlock can happen very easily.
* Imagine a producer thread shutting down while a consumer thread was waiting on a message from it.

### Testing Threaded Code

* Tread spurious failures as candidate threading issues
* Get nonthreaded code working first
* Make threaded code pluggable
* Make threaded code tunable
* Run with more threads than processors
* Run on different platforms
* Instrument your code so you can force failures
  * Can be hand-coded (wait(), sleep(), yield(), priority()) or automated.
  * Automated tools can use _jiggling_ strategies to randomize these values.

### Conclusion

* Embrace SRP -- separate out thread-aware code from the rest.
* Understand possible sources of concurrency issues.
* Learn your library and framework.
* Only lock the least amount of code necessary.
* Avoid calling one locked area from another.
* There is no such thing as a one-off error.
* Instrument your code and test.

## Chapter 14 - Successive Refinement

* Robert Martin walks us through how he wrote and refined a command-line argument parser named Args.
* He first shows the final result, which reads very cleanly top to bottom.
* He then asserts that to write clean code, you must first write dirty code and then clean it.
  * Leaving working code in the state it's in once it started working is professional suicide.
* He shows how his original draft started clean, but became a festering pile once he started adding different argument types.
  * He reached a stopping point and began refactoring.
* There's no substitution to reading the chapter. He covers much of what his book has taught.
* He ends with the assertion that code working is not enough. It must be kept clean and simple continuously.
  * TDD is a big element in this. Starting with a full suite of tests out of the gate gives confidence during refactoring and makes code testable.

## Chapter 15 - JUnit Internals

* Robert Martin walks us through how he refactored the ComparisonCompactor in JUnit.
  * It featured some old prefix styles.
  * He identifies hidden temporal coupling.
  * He makes dozens of changes that the book has gone over.
    * Like the previous chapters, a summary would not do it justice. There is no replacement for reading through the code and the incremental changes Robert makes.
* He ends this chapter by stating that while the code was clean already, the _Boy Scout Rule_ asserts it could be improved further.

## Chapter 16 - Refactoring SerialDate

* Robert Martin walks us through how he refactored the SerialDate class in the JCommon library.
  * Again, this summary does not do the chapter justice. There is no replacement for reading the chapter.
* Robert discovered that the unit test coverage wasn't complete. In fact, there were methods that weren't used at all.
  * In response, he wrote his own suite of tests completely independently.
  * He commented out tests that didn't pass as he uncovered bugs in SerialDate
* He removed extraneous comments.
* He renamed it from SerialDate to DayDate.
  * (It was called SerialDAte because it was implemented using a serial number.)
* He converted an inherited class to enums.
* He walked through dozens of changes.
* In some cases, he found the only usages of functions were his new unit tests, so he deleted those functions.
* He ends with a conclusion similar to the prior two chapters.





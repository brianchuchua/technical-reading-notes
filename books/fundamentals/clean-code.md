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




  
	

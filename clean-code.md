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

* Previous messes slow us down, but we feel pretty to make messes in order to meet deadlines.
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

# Chapter 2 - Meaningful Names

## Introduction
* Names are everywhere and therefore important.

## Advice

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
    * Ex. `Complex.FromRealNumber(23.0) is better than `Complex(23.0)`.
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

## Conclusion

* Don't be afraid of renaming things.

# Chapter 3 - Functions


  
	

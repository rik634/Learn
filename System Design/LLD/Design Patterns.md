# Design Patterns
- A design pattern is named, proven solution to a problem that keeps appearing in software.
- A design pattern is reusable solution to a problem that keeps showing up in our software design.
- It is just a proven blueprint we adapt to our situation.
- Design patterns hand us a proven approach, not a finished solution. We adapt it to our codebase, our language, our situation.
- They improve code maintainability, scalability, and make communication between developers easier.

### Categories of Design Patterns:
-  All 23 patterns are organized into 3 categories based on the type of problem they solve.
   1. Creational Pattern: How objects are created:\
      - These patterns control object creation.
      - Instead of calling new directly everywhere, they give us smarter, more flexible ways to build objects.
        <img width="353" height="155" alt="image" src="https://github.com/user-attachments/assets/5f2548bb-91c8-4b75-b9e1-c815112c6936" />

   2. Structural Pattern: How objects are composed
      - These patterns deal with how classes and objects fit together into larger, flexible structures.
        <img width="344" height="178" alt="image" src="https://github.com/user-attachments/assets/291a5cba-6b45-43dc-a677-113f873cb414" />

   3. Behavioral Pattern: How objects communicate
      - These patterns define how objects interact and how responsibilities are distributed between them.
         <img width="434" height="301" alt="image" src="https://github.com/user-attachments/assets/a38d7774-2be7-41c4-88b5-51fa931deff2" />

- **IMP:** Reach for a pattern when we notice the same design problem recurring, or when we can already tell a simple version will need to grow.

### Questions to ask before reaching for a pattern
- These questions are not a formula, but they are a useful gut check before adding a named pattern to a piece of code.
  1. Have I seen this exact shape of problem before, in this codebase or another one
  2. Would a plain function or a plain object solve this just as clearly, with less ceremony
  3. Will this part of the system likely need new variants added later
  4. Would naming this pattern actually help a teammate understand my intent faster
  5. Am I choosing this pattern because it fits, or because I want to practice using it
- If most of the honest answers point toward "yes, this genuinely recurs and naming it would help", a pattern is probably earning its place. If not, plain code is usually the better choice.
- A common, healthy path is to write the plain version first, and only introduce a named pattern once the plain version actually starts showing the strain, extra branches, duplicated logic, or new variants piling up.
- Patterns applied at that point tend to fit naturally, because the code is telling you what shape it wants.
- Patterns applied too early tend to feel forced, because you are guessing at a shape the problem has not asked for yet.

- A design pattern that solves no real problem is not architecture, it is just extra code standing between you and the answer.
-  Every pattern is a trade. You give up some directness and simplicity, and in exchange you are supposed to get flexibility, testability, or clarity that you will actually use.

###  Questions Worth Asking
1.  Will there realistically be more than two or three variants of this behavior within the next year, not in theory but on the actual roadmap?
2.  Does this object get created in more than one place, with different concrete types chosen at runtime?
3.  Is construction genuinely complex, involving multiple steps, optional pieces, or validation that would otherwise be duplicated?
4.  Will a test need to substitute a fake implementation for this specific piece and nothing else?
5. Does removing the pattern and inlining the logic make the code harder to follow, not easier?
- If most of the honest answers are no, the code you have right now, before any pattern was added, is probably the right amount of structure.

- A Useful Reversal: Instead of asking "which pattern fits this problem," try writing the simplest version first. Only reach for a pattern when the simple version visibly strains under real, current requirements. 
- Revisit The Decision Later: A pattern that earned its place a year ago can stop earning it. Requirements shrink, variants get retired, and an abstraction that once tracked three real cases can end up tracking one.

## Anti-Patterns
- An anti-pattern is a common response to a recurring problem that appears reasonable at first but is ultimately counterproductive.
- There are 5 anti-patterns:
  1. The God Object: One class or module that knows and does too much 
  2. Spaghetti Code: Control flow so tangled that no one can trace it confidently 
  3. The Golden Hammer: Forcing a single familiar tool onto every problem 
  4. Copy Paste Programming: Duplicating code instead of extracting shared logic 
  5. Magic Numbers and Strings: Unexplained literal values scattered through the code 
- Naming an anti-pattern gives a team a shared vocabulary. Instead of a vague code review comment, you can say "this is turning into a God Object" and everyone knows roughly what is wrong and what to do about it.

1. God Object:
- A God object is a single class or module that has grown to know about, and do, far more than its name suggests.
- It ends up touching the database, sending emails, calculating prices, and logging everything, all in one place.
- If you cannot describe a class in one short sentence without using the word "and", it is probably drifting toward a God Object.

2. Spaghetti Code:
- Spaghetti Code describes control flow that has no clear structure.
- Conditions nest inside loops inside conditions, functions jump unto each other unpredictably, and state changes happen in places far from where they are read.
- Why It Happens

  - Spaghetti Code usually grows one small addition at a time. Each new rule gets bolted onto the existing function because that is where the related logic already lives, and nobody wants to be the one who stops to restructure it.
  - Under deadline pressure, adding one more if statement always looks cheaper than pulling the function apart. Repeated many times, that cheap choice compounds into something nobody wants to touch.
- How to Refactor Away From It

  - The remedy is to separate concerns into small, named functions, each doing one job, and to replace nested conditionals with early returns or lookup tables.

- If you cannot point to where a piece of logic lives without scrolling through a large function, that is a sign the function is doing the job of several smaller ones.

3. Golden Hammer:
- The Golden Hammer anti-pattern is naming a favorite tool, pattern, or technology and then reaching for it on every problem, whether it fits or not. The phrase comes from the old line that if all you have is a hammer, everything looks like a nail.
- What It Looks Like

  - A team that loves the Observer pattern might wire up event listeners for things that are really just a direct function call. A team that loves inheritance might build a five level class hierarchy to express what a single conditional would have handled.

-  Why It Happens
   - Familiarity is comfortable. A pattern that worked well on a previous project builds confidence, and reaching for it again feels safe even when the new problem has different shape.
   - It is also reinforced by habit, not malice. A developer who just learned about the Strategy pattern is naturally eager to apply it, and the codebase ends up with more ceremony than the problem needs.
- Before reaching for your favorite pattern, ask what would happen if you solved this with the simplest possible code first, and only add structure when a second real requirement shows up.

4.  Copy Paster Programming:
- Copy Paste Programming is duplicating a block of code to reuse its behavior instead of extracting it into a shared function. It is quick in the moment, and it quietly multiplies the cost of every future bug fix
- What It Looks Like: Two functions validating a signup form and a profile update form look nearly identical, because one was copied from the other and tweaked slightly.
- Why It Happens
  - Copying an existing function is the fastest way to get something working right now, especially under deadline pressure. Extracting a shared helper takes a few extra minutes of thought about naming and parameters.
  - It is also encouraged by unclear ownership. If nobody is responsible for the shape of the validation layer as a whole, each new form just gets its own copy rather than reusing what exists

- How to Refactor Away From It : Extract the shared rules into small, named validator functions, and compose them per form. Each rule is written once and tested once.

- The first duplication is often fine to leave alone. The moment you are about to paste a third copy of the same logic, that is the signal to extract a shared function.
  
5. Magic Numbers and Strings
- Magic numbers and magic strings are unexplained literal values sitting directly in the middle of logic. The number 0.07 might be a tax rate, a discount, or a rounding threshold, and the code gives no clue which.
- What It Looks Like
  - Literals like 3, 86400, and "PENDING" are scattered through the code with no name attached, so their meaning has to be guessed from context or from asking whoever wrote them.
  - A reader has to reverse engineer that 86400 is seconds in a day, that 1.0725 is a tax multiplier for California, and that "PENDING" is one of some fixed set of statuses that exists nowhere as a single list.
- Why It Happens
  - Typing a literal value is the path of least resistance while you are focused on getting a calculation right. Naming it as a constant feels like a side task that can happen later, and later often does not come.
  - String literals for statuses are especially easy to let slip in, since there is no compiler error for typing "Pending" in one place and "PENDING" in another, the mismatch just silently fails at runtime.
 
- How to Refactor Away From It
  - Give every meaningful literal a name. Group related constants together, use an enum-like structure for fixed sets of strings, and let the code read close to the business rule it represents

## Summary
- The refactors are not complicated on their own. Split responsibilities, flatten control flow into small named functions, choose tools that match the actual problem, extract shared logic, and name your constants.-
- Clean code is rarely written that way from the first draft, it gets there by someone noticing the anti-pattern and refactoring before it spreads.
- A pattern is not something you reach for because it is elegant. It is something you reach for because something in your system genuinely varies and needs a name.
- Always name what is varying before you name which pattern to use. The variation comes first, the pattern comes second.
- A  decision framework forces you to slow down for thirty seconds and ask a few sharp questions before you touch any pattern name.
  <img width="331" height="343" alt="image" src="https://github.com/user-attachments/assets/b663df5f-be40-4eee-8d62-72cfdb329f22" />

1. Name the axis of variation in one sentence
2. Check whether it repeats or is a one-off decision
3. Check whether it must change at runtime or only at build time
4. Only then look for a pattern that matches the ans

- If you can solve the problem with a single well-named function and no new classes, do that first. Patterns should remove complexity, not add ceremony.
  <img width="434" height="278" alt="image" src="https://github.com/user-attachments/assets/4c8f18c3-ef96-42e0-b81a-8652b84581ad" />
 <img width="485" height="312" alt="image" src="https://github.com/user-attachments/assets/77a41686-3187-400e-a477-e0d8040df510" />
-  Before writing the first line of a pattern implementation, run through a short checklist.
- This takes less than a minute and catches most premature abstraction before it costs you anything.
- The Checklist
  1. Can I state the varying axis in one plain sentence
  2. Have I seen this variation happen at least twice, or do I have strong evidence it will
  3. Do I know whether the choice needs to change at runtime or only at deploy time
  4. Does the quick reference table point me toward one family, not three
  5. Would a plain function or a simple if statement genuinely be harder to maintain than the pattern
- If you answer yes to most of these, you are picking a pattern for the right reasons.
- If you are answering mostly no, it is worth writing the plain version first and revisiting the pattern once the variation actually shows up.
-  When a module introduces Factory Method or Builder, revisit question one, what is varying, and confirm it matches the Creational description of flexible or staged construction.
-  When a module introduces Adapter or Composite, confirm the variation is really about structure or compatibility, not behavior.
-  When a module introduces Strategy, Observer, or State, confirm the variation is about behavior changing over time or across contexts.

-  Creational Design Patterns (7): Singleton, hread-safe Singleton, actory Method, bstract Factory, uilder, rototype, Object Pool:
- Structural Design Patterns (7): Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy:
- Behavioral Design Patterns (10): Strategy, Observer, Command, Chain of Responsibility,  State, Template Method, Iterator, Mediator, Visitor, Memento:

## Creational Design Patterns:
1. Singleton:


2. Thread-safe Singleton:


3. Factory Method:


4. Abstract Factory:


5. Builder


6. Prototype

7. Object Pool:

## Structural Design Patterns:
1. Adapter:
2. Bridge:
3. Composite:
4. Decorator:
5. Facade:
6. Flyweight:
7. Proxy:

## Behavioral Design Patterns:
1. Strategy:
2. Observer:
3. Command:
4. Chain of Responsibility:
5. State:
6. Template Method:
7. Iterator:
8. Mediator:
9. Visitor:
10. Memento:

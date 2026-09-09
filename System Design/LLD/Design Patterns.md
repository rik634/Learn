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


- 

---
layout: post
title: "Book Notes - Code That Fits in Your Head"
date: 2024-10-27
categories: ['jekyll']
tags: ['Book Notes']
---

# Chapter 2

Checklists
- Help as people forget things, particularly if not important now.
- Memory aid
- Things that need doing for a given task
- Things will be forgotten for complex tasks
- Reduces cognitive load by allowing focus to be on hard parts of the task instead of trivial parts. Checklists will remind you of the trivial.
- Help improve outcomes with no increase in skill.
- They support you, not control you.
- Simple list of items
  - *Read-Do* - Do items in sequence.
  - *Do-Confirm* - Do all items then confirm

Notes / Questions:
- Can GitHub action script be run locally - to check CI/CD pipeline is running
- FXCop vs StyleCop

Recommended Checklist Items for a new code base
- Use Git
- Automate the build (Automatic CI/CD Pipeline)
  - Create the smallest amount of code that can be deployed (similar to a walking skeleton - the thinnest slice of real functionality)
- Turn on all error messages - Treat all warnings as errors, treat analyser warnings as errors.
- Turn on analysers - eg: StyleCop  & FxCop
- Turn on Nullable reference types

Turning on these tools for a new code base is easier than trying to apply them to an existing code base, as errors can be dealt with one at a time. It might be frustrating, but will improve the code, and maybe make you a better programmer. These tools are like automated checklists.

In an existing code base gradually turn on the extra guards, and merge into the trunk branch.

Follow the Boy Scout Rule: *leave the code in a better stat that you found it*.

Treat warnings as errors and then do not bypass. The code wont compile. This is a technical decision which you as a technical expert make. How you convey this decision is a moral judgement, which may depend on your organisation (healthy vs unhealthy culture), but:
> Providing technical expertise includes *not* confusing stakeholders with details they can't make sense of or use.

Organisation hacking - enforce requirements via code, then can say it is a technical requirement that must be met, rather than trying to convince others it is needed.


# Chapter 5 - Encapsulation

## Design

- Encapsulation is a contract for an object stipulating that it will behave reasonably
  - Contract that describes valid interactions between objects and callers
  - The object guarantees it will always be in a valid state.
- Minimise the time that code is in a state of not working - move in small increments


## Testing

- Parameterised tests - Add additional test cases to 'drive' the change
- Have positive and negative tests


## Red Green Refactor + TDD

- One of the most 'scientific methodologies' of software engineering


## Coding

- Prefer exceptions that give more info
- Refactoring - improving the internal structure without altering the external behaviour - all tests should still pass
 

## Notes

The 'art' of software engineering - 'the shifting sands of individual experience'

Design by Contract
Don't need to know implementation details, allows thinking of the object more abstractly - the contract is the essential quality of an object
Design explicitely by considering what is and is not valid input. And what guarantees you can give about output.

A domain model is an attempt to describe the 'real world'. Natural numbers (>=1) are everywhere.

Postel's Law - Be conservative in what you send, be liberal in what you accept.
- Be specific in what output you provide. The more guaratees given, the less defensive code the caller has to write.
- The more broad the input can be, the easier the caller can interact with the object. But only allow input you can meaningfully work with.

Cyclomatic Complexity - the number of independant paths through a piece of code.

Domain Models should be 'Always Valid'. Encapsulation should guarantee that an object is always in a valid state. Immutable objects mean you only need to consider validity at the constructor - the object can't be mutated.

Preconditions describe the responsibilities of the caller (eg: input items can't be null, though the domain model should enforce this. Postconditions describe the guaratees given by the object. These pre and post conditions are defined by the contract. These form invariants - conditions about the model that be true.

Don't assert on exception messages. The exception message isn't part of an object's behaviour, it is a form of implementation detail. Also, don't repeat yourself (DRY). Though, the exception message confirms the exception is being thrown the expected part of the code, and isn't actually a different exception.


# Chapter 6

Working with legacy code is hard:
- It take time to learn the code base
- Change is hard

Develop sustainable code that fits in your brain.

What HTTP status to return for an unsuccessful POST?

Code (both tests and production code) should be explicit:
Zen of python: _Explicit is better than implicit_

Perfect is the enemy of good

Triangulation process:
> _As the tests get more specific, the code gets more generic._
> Robert C. Martin

In TDD, the first test case is passed by applying the simplest logic, which is returning for the exact case passed in (Devil's Advocate technique). The second (and subsequent) test cases drives the implementation towards a more general solution.
Devil's Advocate technique is in its simplicity to try and pass all tests with an obviously bad implementation. Adding further tests forces a better implementation. Add more test cases till the Devil is defeated. Remember to apply Red-Green-Refactor to simplify the code.

arrange number expression from smallest on the left. eg: `min <= myValue && myValue <= max`. This makes the expression better match the number line, improving readability. (Therefore `<` or `<=` will be used predominantly).

Repository classes aren't tested directly.

When are there enough tests? There isn't a quantitative answer. Add enough tests for the situation to give confidence that the code doesn't contain mistakes. If a defect (error, issue, bug, etc.) occurs, add a test that reproduces the defect. If it happens in production, it could happen again. Prevent a regression reoccurring with a test. This is risk assessment, weight the adverse outcome with its impact.

The more tests there are, the better the System Under Test is described. Using Devil's Advocate and Red-Green-Refactor helps give good test coverage while minimising redundant test cases.


# Chapter 7 - Decomposition

Code bases gradually deteriorate.

Code Rot - If overall code quality is ignored, then the code base becomes more complicated. When it becomes complicated enough people notice, a legacy code base has formed.

Keep cyclomatic complexity below a threshold. Enforce this using an automatic rule. Seven is a good threshold (based on the brain's working memory capacity). But when needed rules can be broken (for a time). The goal is awareness.

Lines of code is th emost pragmatic predictor of complexity.

Cyclomatic complexity - number of pathways through a piece of code.

## 80/24 rule
Recommended max lines in method should be ~24.
Write small method to improve readability - code that fits in your brain.

maximum line width of 80 characters.

VT100 terminal is 80x24 characters. This is an arbitrary guide.

Consider refactoring a method if it goes over 24 lines.

More code is harder to track, and not immediately comprehensible. It doesn't fit in your short term memory.

> No more than seven things should be going in a single piece of code.

> Things that change at the same rate belong together. Things that change at different rates belong apart.

Chunks of code can shifted and replaced with a reference to the chunk. This replaces many things with one thing. This is possible if you can _abstract_ the _essence_ of the code chunk.

> Abstraction is the elimination of the irrelevant and the amplification of the essential.
> - Robert C. Martin

In object-orientated design pay attention to static methods. Feature envy is a code smell. Try moving the method to the object it seems 'envious' of.

A property in C# is syntactic sugar for a 'getter'/'setter' method.

Domain models provide encapsulation. It protects its invariants (conditions about the system that are true for some state of the system), including pre and postconditions. A properly initialised object is guaranteed to be in a valid state.

Convert (or project) DTOs to domain objects early. This validates the data and gives a stronger representation. It also reduces data validation code (DRY). This is known as _parse, don't validate_.

Guard clauses - statements that check the provided values meet the preconditions.

> **Maybe**
> If the _essence_ of a method can be captured in its signature, then that's a good abstraction.
> Certain languages have a `Maybe` type (or `Option`), which can be recreated in C#, eg: `Maybe<MyType>`.
> This indicates that the object to return may or may not have been created, and it is on the caller to check.
> C#'s nullable references types effectively provide this capability.

A parser is just a function that consumes less-structured input and produces more-structured output. A parser needs a notion of failure, there will be provided values that fall outside the range of the domain.

_Fractal architecture_, at all levels of abstraction the code should fit in your brain. At a given level the goal is to keep complexity low, such as keeping cyclomatic complexity to seven for fewer. At the greatest depth will be methods that don't call any other user code. A fractal architecture is a way to organise code. Lower-lvel details should be represented as a single abstract chunk, while higher level details are either irrelevant at that level, or explicitly visible as method parameters or injected dependencies.

Another possible measure of complexity is variable count. Keep the number of variables (local variables, parameters, used properties) to seven or fewer.


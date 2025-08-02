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
- Be specific in what output you provide.
- The more broad the input can be, the easier the caller can interact with the object. But only allow input you can meaningfully work with.

Cyclomatic Complexity - the number of independant paths through a piece of code.

Domain Models should be 'Always Valid'. Encapsulation should guarantee that an object is always in a valid state. Immutable objects mean you only need to consider validity at the constructor - the object can't be mutated.

Preconditions describe the responsibilities of the caller (eg: input items can't be null, though the domain model should enforce this. Postconditions describe the guaratees given by the object. These pre and post conditions are defined by the contract. These form invariants - conditions about the model that be true.

Don't assert on exception messages. The exception message isn't part of an object's behaviour, it is a form of implementation detail. Also, don't repeat yourself (DRY).

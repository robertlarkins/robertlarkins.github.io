---
layout: post
title: "Book Notes - Code That Fits in Your Head"
date: 2024-10-27
categories: ['jekyll']
tags: ['Book Notes']
---

Sample code can be found here (but requires registering the book): https://www.informit.com/store/code-that-fits-in-your-head-heuristics-for-software-9780137464401


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

Code that is easier to comprhend is more likely to be changed than code that is harder to comprehend. You must actively prevent code from rotting. There are different measures of complexity, that give awareness to code rot, but you must use your own judgement.


# Chapter 8 - API Design

Complex code should be decomposed.
API = Application Programming Interface
Interface is an *affordance* - it *affords* the ability to do something. The set of methods, values, functions, and objects you have at your disposal. An API allows you to interact with an encapsulate package of code, which preserves its invariants.

An API only affords its capabilities to client code the fulfils the required preconditions.

Don't design a Swiss Army knife, an API with many capabilities in one once may become a God class, which is an antipattern.

Design APIs so they're difficult to misuse. Mistake proof artefacts and processes. Active mistake-proofing inspects artefacts when written, such as done by TDD.
Passive mistake-proofing make illegal states unrepresentable, such as impossible cases being compile time errors.

Code is read more than written - write code for future readers.

Favour well-named code over comments - comments may deteriorate as the code evolves, and eventually become misleading. The code (that gets compiled) is only artefact you can trust. Can a comments be replaced with a well named variable or method? Names can grow stale as the behavoiur changes as well. Can the input parameters and return type of a method convey its purpose (along with the class or interface that is in) without knowing the name? Using the context of the object and only a few methods and particular types, they methods can convey information without the name. The name then doesn't need to repeat what the rest of the signature conveys, but can provide other info to the reader.

The purpose of encapsulation is to hide implementation details.

Command Query Separation (CQS)
- Commands - Methods with side-effects that should return no data. Their return type should be `void`.
- Queries - Methods with no side-effects, that should return data. They have a return type.
Favour Queries over commands, as queries are easier to reason about.

*How to solve adding a row to a database and return the generated ID using CQS*

> Don't say anything with a comment that you can say with a method (or variable) name.
> Don't say anything with a method name you can say with a type.

Guide the reader by:
1. Giving APIs distinct types - these are part of the compilation process
2. Giving methods helpful names - part of the compiled code, but don't cause compilation errors
3. Writing good comments - kept with the code
4. Providing illustrative examples as automated tests
5. Writing helpful commit messages in Git - information about the committed change (such as why)
6. Writing good documentation - Higher level information
Everything from 2 down can easily stagnate.

Write code for readers.

> *Any fool can write codde that a computer can understand. Good programmers write code that humans can understand.*
> - Martin Fowler

Encapsulation is important for making code that humans can understand. Encapsulation hides implementation details, which should sat irrelevant until you need to change them.


# Chapter 9 - Team Work

Understand the motivation for processes and that their outcome is what matters. Both in the short and long term.

## GIT

Use GIT tactically


### Commit Messages

These messages persist for you and future readers.

> Focus on *communication* over writing.

Git commit messages have a de-facto standard - the 50/72 rule - based on how various Git features work.
```
Summary in the imperative mood 50 characters or less

Extra information no longer than 72 charactres per line.
Write proper prose, that is, use proper spelling and grammar to improve
readability. It also aids in discoverability by having consistent
spelling.
```

The diff contains *what changed*, the commit message is often the best place to explain *why* the change was made.
The rationale behind code is a high-ranked problem in software engineering.


## Continuous Integration

Continuous Integration is a *practice*. The code you work on and the code of your colleagues is continually integrated (merged) with the trunk branch.
Continuous here isn't literal, it means frequently, such as every few hours.

By continuously integrating, the risk of having merge conflicts decreases, other wise merge hell can occur.


## Small Commits

Git allows for *manoeuvrability*, or code experimentation. Make a change, if it works commit it. If not, reset. Make micro-commits. Once the code change is in a good state tidy up the branch. This is done by squashing or rebasing commits to better group changes together (eg: keeping tests with the logic they test) with a good commit message. This forms a more coherent commit history. Then form a Pull Request.

The commit history should be a series of snapshots of *working* software. Do not commit code that doesn't work.


## Collective Code Ownership

Bus Factor - How many people can be 'hit by a bus' before development halts? Circumstances change, no single person should be indespensible.

*Does the team contain more than one person comfortable working with a particular piece of code?* The team must continually answer this as: yes.

Two active maintainers should approve code base changes (pair programming or code reviews).

Pair programming - two developers collaborating inreal time on the same problem. Helps avoid knowledge silos, especially by pair rotation. There are benefits, though independent coding at times may work better for individuals.

Mob programming - Three or more people programming together. There is a point of diminishing returns. Mob programming is great for knowledge transfer.

Code reviews help detect problems before code is deployed to production. Deployed defects can lead to unplanned work and firefighting, causing more time involvement (*shifting left* helps catch issues earlier). Code reviews should occcur quickly once a Pull Request has been formed. Changes should be small, less than half a days work, making it easier to review, more likely to be reviewed, give more specific feedback and detect defects. This all helps provide feedback and learning to the code submitter. Larger PRs take longer to review as there is more back and forth.

When performing a code review, saying *no* should be a real option. Eg: when a PR is too big and should be broken into smaller PRs. A code review is worthless if it is just rubber stamping. Do not give up and accept big PRs, reject them and request smaller PRs. This is known as *sunk cost fallacy*, while time has been spent on the PR, if not reviewed properly even more time could be spent in the future fixing or maintaining poor design.

A code review that takes more than an hour is not effective.

The fundamental question of a code review is: *Will I be okay maintaining this*.
(another valid and secondary question is *Does the change address a valid concern*).

The most important criterion in a code review is: is the code readable - does it fit in your brain. Can you trust the code? Ultimately it is the only artefact that matters.

The author should never guide the code review. The reviewer needs to be judge the code on its own merits. The author may cause the reviewer to overlook problematic practices.

The review should conduct the review at their own pace (without the author). The author isn't in a position to say whether code is readable by others.

Nitpicking should be minimal, does the code fit in your brain (eg: are methods too long or complex)? Can review aspects be automated with linters?

Non-exhaustive list of things to look for:
- Does the code work as intended?
- Is the intent clear?
- Is there needless duplication?
- Could existing code have solved this?
- Are the tests comprehensive and clear?

There will often be suggestions the review provides that ellicit dialogue, with improvements aggreed upon. This is an iterative process that will reach consensus and the changes integrated.

Everybody should be authors and reviewers.

Pull Requests (PR) - Somebody else review and approve the code before merging into main.

For PRs do the following and review for the following:
- Make the pull request as small as possible
- Atomic PRs. One thing per PR only.
  - Avoid reformatting - that should be its own PR
- Code builds
- All tests pass
- New behaviour includes the tests
- Write proper commit messages

When reviewing be extra polite in the way comments are written as tone and intent can be lost. Use [comment conventions](https://conventionalcomments.org/) and emojis to help. As a review do a proper review, and decline if too big to review rather than simply approving. Work with the author, offer alternatives, give praise, run the code on your machine.


---
layout: post
title: "Book Notes - Code That Fits in Your Head - Part II - Sustainability"
date: 2024-10-27
categories: ['jekyll']
tags: ['Book Notes']
---

Part II - Sustainability


# Chapter 10 - Augmenting Code

Most software development is with existing code.

Refactoring - changing existing code without changing its behaviour


**Feature Flags**

Feature flags hide incomplete code, allowing the behaviour being implemented to be deployed to production, but unavailable for use.

Feature flags also allow a feature to be disabled if it is causing issues that need to be immediately mitigated. Caution is needed for what impact turning a feature off might have for consumer capability and their data.

Feature flags should be removed after the feature is deemed complete and safe. Flags that remain for prolonged periods become unsafe to turn off as the code they wrap gets extended and further depended upon for other features. Also, the context about what the flag does  becomes forgotten or lost.

Deleting code is satisfying as there is less code to maintain.


**The Strangler Pattern**

Adding a new feature is often done by appending new code to the existing code base.

Heroism - This is not an engineering practice. It is too unpredicatble, and can stimuate sunk cost fallacies. Avoid.

> for each desired change, make the change easy (warning: this may be hard), then make the easy change.
> - Kent Beck

For any significant change, don't make it in-place; make it side-by-side

The stranger pattern (named after the strangler fig vine) is adding new code (either a method or class), shifting callers over, then deleting the old replaced code.

Maintenance Tax - When more code is added there is more code to maintain

When deleting a method from an interface, remember to remove it from the implementing classes. The compiler wont complain, but they will be a maintenance burden.


**Versioning**

Semantic Versioning _major.minor.patch_

The following is when to increment each value:
- _major_ - breaking change
- _minor_ - new feature
- _patch_ - bug fix

Versioning helps you think about the impact your changes have on other code that depends on your code, and how it should be conveyed to your callers. It is better to give advanced warning - such as using the `[Obsolete]` attribute, then after some time removing the code as a major change.


# Chapter 11 - Editing Unit Tests

How do you know your test suite is mistake free? You don't, but production code gives feedback about the tests.
Modify existing tests carefully, as tests don't have their own tests (though mutation testing helps with this). The more they are modified the more likely mistakes are introduced.

Adding new test classes, tests, Theory cases and assertions are fairly safe. Using IDE refactorings also generaly safe. Deleting tests or assertions can weaken the guarantees of the system - possibly allowing regression bugs and breaking changes to occur.

Refactor test and production code separately (if possible), and check into Git as separate commits. As test and production code are coupled, introducing a defect is more likely to have a test alert on it. Though this does depend on there being test cases that expose the defect.

Nevver trust a test you haven't seen fail. Follow Red, Green, Refactor, or temporarily modify production code to confirm the test fails.


# Chapter 12 - Troubleshooting

Troubleshooting skills are often based on "the shifting sands of individual experience", but there are techniques that can be applied.

Best advice: Try to understand what is going on.

Make understanding a priority. This should be the first reaction to a problem. If you have absolutely no idea, ask for help. Though if you have an inclination, follow a scientific method:
1. Make a prediction (hypothesis)
2. Perform the experiment
3. Compare outcome to prediction
4. Repeat till you have understanding

Simplify - consider if removing (simplifying) code will make a problem go away. It could be an implementation error.

Keeping things simple reduces or removes other problems that can occur. Though coming up with a simpler solution is _hard_.

https://www.infoq.com/presentations/Simple-Made-Easy/

You can draw a blank on a problem while trying to understand it. Try the following:
- Time-box the process (eg: 25 min), if no progress is made then take a break by leaving the computer and do something else (eg: for 5min).
- Try asking for help. Explain the problem tends to produce new insights
- If you have no colleague, try explaining to a 'rubber duck' (known as rubber ducking)

The ideal number of defects is zero.

In software development, _later_ is _never_. When a defect occurs, make it a priority to address it. Stop what you're doing and fix the defect.

Once the problem has potentially been identified, recreate it as a test. This is an experiment, the hypothesis is the test will fail. If it doees fail, then this test reproduces the defect and can serve as a regression test. Otherwise, revise and design a new experiment. Then fix the code to make the test pass.

When examining code and logs, slow down. Spend a moment to think through and understand what a field should be, the code should do, or what the log is saying. Is what is being said or set match your expectations?

Slows tests (integration tests) should be run as part of your deployment pipeline. They can also be created to try and recreate non-deterministic defects.

Bisection - detect (eg: an automated test) or reproduce a problem by repeatedly removing half the code until the code that produces the problem is small enough to understand what is going on. Producing a minimal working example of a problem is a super power as it can be illuminating in understanding the cause.

GIT Bisect - Git feature to do a binary search through commits to identify the one that introduces a defect.

Learn and use the above technique. Using a debugger has its place, but a debugger can't be used everywhere (eg: production).


# Chapter 13 - Separation of Concerns

> Things that change at the same rate belong together. Things that change at different rates belong apart.
> - Kent Beck

Decomposition is intimately related to Composition. Decompose so that you can compose working software from the resulting parts. Decomposition is only valuable if you can recompose the disparate parts.

Nested Composition - Software interacts with the real world (paints pixels on the screen, sends emails, saves data in the DB, etc). In the context of Command Query Separation (CQS), these *actions* are *side effects*. Composing these side effects into models results in side effects being nested inside other side effects. This violates CQS and makes it difficult to fit in your head what the code is doing.

> Abstraction is the elimination of the irrelevant and the amplification of the essential
> - Robert C. Martin

By hiding side effects in Queries, something essential (the side effect) is eliminated (hidden and more difficult to follow what the code is doing).

A constructor should be a 'Query' (in the context of CQS) as it *should not* have any side effects.

Sequential Composition - Can be performed as a series of Queries. Once the result is determined then the side effect action can be performed. But not as part of these queries. This eliminates the irrelevant and amplifies the essential.

Referential Transparency - Queries must be deterministic. That must not rely on anything random, eg: guid creation, random number generators, time of day, or any other data from the environment. A deterministic method without side effects is *referentially transparent*. It is also a *pure function*. Pure functions can always be sequentially composed. With respect to abstraction the result is what matters, how the function arrived at the result is an implementation detail.
Zooming in on a pure function, it can be assessed in isolation - the surrounding context is irrelevant - it operates exclusively on its inputs and immutable class fields.

Where does the non-deterministic behaviour and side effects go? Push these to the edge of the system: Controllers, message handlers, etc.
This style of programming is known as *functional core, imperative shell*.

There is a recommendation to learn a functional programming language (eg: F#).

Cross-Cutting Concerns - These are concerns that tend to cut across multiple disparate features, eg: Logging, Performance monitoring, auditing, etc. Logging should be added as a minimum to give insights into the running system.

Cross-cutting concerns tend to be best implemented using the Decorator design pattern (Refer to the book for a worked example). This pattern works well for most cross-cutting concerns, such as sending emails, or read from and writing to a cache. Security concerns can be addressed using this pattern, but most frameworks have built in security features, it is better to use those.

What to log - Only log what you need. Log what data is needed to just reproduce execution. This allows you to replay what happened when a problem manifests, and troubleshoot it.

> Log all impure actions, but no more.

Log everything you can't reproduce. This includes all nondeterministic code (eg: current date) and anything that has side effects.


# Chapter 14 - Rhythm

A loose rhythm or structure to the day, with the ability to continuously deploy can be beneficial for a team to move fast.

Have a loose structure to your day, as structure can help you get things done.

Time-boxing, work for 25min, take 5min break (like Pomodoro, but less involved). 25min of interrupted work helps make big tasks seem more manageable - the hardest part of most tasks is getting started, saying I'll look at it for 25min can make it less daunting. 
Use a visible timer, to know when your next break is. This will help counter the 'just' check Slack or emails.

For the 5min break, make it a proper break. Get out of your chair, walk around, leave the room, get away from the computer. This has physical health benefits, but also gives you a chance to reflect on the work you're doing, which may help. Good ideas often come when doing something else - the alternation between being in front of the computer and elsewhere seems to be the trick.

Also take longer breaks, for the same benefits. Also, working longer hours may lead to negative productivity, as mistakes can occur that have to be fixed.

Use your time deliberately. Spend dedicated time educating yourself - keep learning. You also learn a lot by teaching. Programming katas are another option.
Minimise the amount of meetings you attend. Meetings needs to have an agenda. Capture information that can be shared with others, if the meeting is a requst for information, then just point them to the already written information. Meetings don't scale, documentation does.

Touch Type - learn how to if you don't already do it. While you'll be able to type faster, it allows you to keep your attention on the screen, allowing mistakes to be picked up faster, or pick up suggestions from the IDE.

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



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

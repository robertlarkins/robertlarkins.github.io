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



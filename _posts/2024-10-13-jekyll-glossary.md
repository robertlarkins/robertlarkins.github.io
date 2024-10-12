---
layout: post
title: "Jekyll Glossary"
date: 2024-09-30 7:30:00 +1200
categories: ['glossary', 'jekyll']
tags: []
---

Jekyll is built using Ruby. Therefore, this glossary will include Ruby terms.

| Term        | Description     |
|    :---:    | :---            |
| **Gem**     | A Ruby package is called a Gem (like a NuGet in C#). Jekyll itself is a Gem. |
| **Gemfile** | A list of Gems that a project depends on. In Jekyll a Gemfile can also contain the list of Jekyll plugins. |
| **Bundler** | [Bundler](https://bundler.io/) is a program that reads the Gemfile and generates a Gemfile.lock file. Each Gem in the Gemfile.lock will have a version (even if not specified in the Gemfile). Bundler then downloads and installs the Gems defined in Gemfile.lock so the application can use them. |

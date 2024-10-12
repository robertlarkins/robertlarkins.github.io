---
layout: post
title: "Add Gem Packages"
date: 2024-10-13
categories: ['jekyll']
tags: ['setup', 'ruby', 'versions']
---

The Gemfile lists the Gem packages that a Ruby project depends on. 
A Gem can be added to a Gemfile using the CLI command `bundle add <gem-name>` (or manually). The version can be specified using the `-v` flag. The following [version constraints](https://guides.rubygems.org/patterns/#pessimistic-version-constraint) can also be added:

| Operator | Description |
| :---:    | :---        |
|   `~>`   | Pessimistic operator (aka twiddle-wakka). |
|   `!=`   | Not equal to. |
|   `<`    | Less than. |
|   `<=`   | Less than or equal to. |
|   `=`    | Equal to. |
|   `>=`   | Greater than or equal. |
|   `>`    | Greater than. |

Some examples:

| Command | Gemfile Result | Details |
| :---    | :---           | :---    |
| `bundle add jekyll -v 3.10.0` | `gem "jekyll", "= 3.10.0"` | Explicit version. |
| `bundle add jekyll -v "= 3.10.0"` | `gem "jekyll", "= 3.10.0"` | Explicit version. |
| `bundle add jekyll -v '~> 3.10.0'` | `gem "jekyll", "~> 3.10.0"` | Any patch version of 3.10. |
| `bundle add jekyll -v '> 3.9.0, <= 3.10.0'` | `gem "jekyll", "> 3.9.0", "<= 3.10.0"` | Any version greater than version 3.9 and less than or equal to version 3.10. |
---
layout: post
title: "Running Jekyll Locally"
date: 2024-09-30 7:30:00 +1200
categories: ['jekyll']
tags: ['local', 'github pages', 'hosting']
---

A Jekyll website can be hosted and viewed locally. This saves having to view changes via GitHub Pages, which would require committing changes to the GitHub repository.

To host the site locally, do the following ([based on these instructions](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)):

1. Install Ruby using [these instructions](https://jekyllrb.com/docs/installation/windows/).
1. Open the terminal and navigate to the root directory of the `<your-account-name>.github.io` repo.
1. Run the following commands:
   1. `bundle init` - [Creates the Gemfile](https://www.bundler.cn/man/bundle-init.1.html).
   1. `bundle add jekyll -v 3.10.0` - Adds Jekyll version 3.10.0 to the Gemfile ([github-pages depends on this version](https://pages.github.com/versions/)).
1. Add `gem "github-pages", group: :jekyll_plugins` to the Gemfile. This is based on the [instructions for the setting up the minimal theme](https://github.com/pages-themes/minimal?tab=readme-ov-file#usage).

    The Gemfile will look something like this:
    ```yml
    # frozen_string_literal: true

    source "https://rubygems.org"

    # gem "rails"
    gem "jekyll", "= 3.10.0"
    gem "github-pages", group: :jekyll_plugins
    ```
1. Run the command `bundle install`. This creates the Gemfile.lock file and installs any needed Gems.
1. Run the command `bundle exec jekyll serve` to start the webhost.
1. Browse to http://127.0.0.1:4000/.

More extensive setup instructions can be followed here: https://jekyllrb.com/docs/.


### Notes

- What is `group:`? [Reading this page](https://jekyllrb.com/docs/plugins/installation/#the-jekyll_plugins-group) it seems that the line `gem "github-pages", group: :jekyll_plugins` seems to include `github-pages` as part of the `jekyll_plugins` group.

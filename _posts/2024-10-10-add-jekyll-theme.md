---
layout: post
title: "Add Jekyll Theme"
date: 2024-09-17 7:30:00 +1200
categories: ['jekyll']
tags: ['theme', 'GitHub Pages']
---

GitHub Pages has a selection of [supported themes](https://pages.github.com/themes/), though any Jekyll theme can be used. The [Jekyll website links to different theme sites](https://jekyllrb.com/docs/themes/#pick-up-a-theme) with many themes available. A theme is applied using one of the following approaches:
- Via Ruby gem.
- Set using `remote_theme`.
- Adding all theme files.

The first two approaches are referenced in the [GitHub Pages documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll#adding-a-theme).


## Via Ruby Gem

A theme can be set by using its Ruby gem. The theme's gem is first added to the Gemfile like this:

```yml
gem "THEME-GEM"
```
where `THEME-GEM` is the name of the gem. Then the gem is referenced using the `theme` option in `_config.yml`:

```yml
theme: THEME-GEM
```

An example is the [*Type-on-Strap* theme](https://github.com/sylhare/Type-on-Strap). It is added to the Gemfile as:

```yml
gem "type-on-strap"
```

and referenced in the `_config.yml` as 

```yml
theme: type-on-strap
```

The name of a theme and its gem are usually found in the theme's README.md or associated documentation. Additionally, as the theme is a gem, a specific version can be set using the RubyGems version specifier syntax.

GitHub Pages supported themes only need the `theme` option. However, only the latest version of theme will be used. An example is the `minima` theme, which is simply added to the `_config.yml` as:

```yml
theme: minima
```


## Via `remote_theme`

The `remote_theme` option is specific to GitHub Pages. It goes in the `_config.yml` and replaces the `theme` option. Its usage looks like this:

```yaml
remote_theme: OWNER/REPO
```

Where `OWNER/REPO` is the path to the theme in GitHub. For example, the `minima` theme is found at https://github.com/jekyll/minima, so the value is:

```yaml
remote_theme: jekyll/minima
```

The [jekyll-remote-theme plugin](https://github.com/benbalter/jekyll-remote-theme) provides the `remote_theme` option. This plugin does not need to be specifically referenced as it bundled with the github-pages gem.


### Specific Version

Referencing the theme using the `OWNER/REPO` path will use its latest version. [A specific version can be set](https://github.com/benbalter/jekyll-remote-theme?tab=readme-ov-file#declaring-your-theme) by appending `@<git_ref>`, where `<git_ref>` is one of the following approaches:

| Approach | Example | Notes |
| :---     | :---    | :---  |
| Tag (version) | `jekyll/minima@v1.2.0` | |
| Branch   | `jekyll/minima@minima-next` | `minima-next` is the branch name. |
| Commit   | `jekyll/minima@9350a3b` | This example uses a short git commit hash (usually the first 7 characters of the full hash). The full git commit hash can be used. |


## Adding all theme files

Themes can also be applied by adding their files directly to the site. This is done by either:
- Fork the theme and building on top of it.
- Copy all the theme files into the site.

However, it is **not** recommended to add external theme files directly. This is because applying a newer version of the theme, or changing themes becomes much more challenging. Instead, use the above approaches to reference a theme. If theme customisation is desired, then [theme defaults can be overridden](https://jekyllrb.com/docs/themes/#overriding-theme-defaults).

Here's [somebody else's experience](https://www.richard-stanton.com/2023/01/20/github-pages-remote-theme.html) with forking the theme, and the related problems.

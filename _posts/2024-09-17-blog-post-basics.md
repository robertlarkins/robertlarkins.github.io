---
layout: post
title: "Adding Posts"
date: 2024-09-17 7:30:00 +1200
categories: [jekyll]
tags: ['front matter', 'posts', 'github pages']
---


# Blog Post Basics

Jekyll themes often support blog posts, as [blogging is baked into Jekyll](https://jekyllrb.com/docs/posts/). Posts are written in Markdown (or HTML), and generally live in a `_posts` directory at the root of the site. Though, `_posts` directories can be placed anywhere within the site's directory structure.


## Creating a Post File

To create a post, add a file to the `_posts` directory with the following naming convention:

```
yyyy-MM-dd-blog-post-title.MARKUP
```

Which is made up of the following parts:

- `yyyy-MM-dd` - The date in ISO 8601 format.
- `blog-post-title` - Replace with desired blog post title.
- `MARKUP` - The extension for the format used in the file. Typically `md` or `html`.

Here is an example of a valid file name:

```
2024-09-21-my-first-post.md
```


## Front Matter

[_Front matter_](https://jekyllrb.com/docs/front-matter/) is a YAML block of variables placed at the start of a post file. Jekyll processes files with front matter and makes [the variables (along with other data)](https://jekyllrb.com/docs/variables/) accessible via [Liquid tags](https://jekyllrb.com/docs/liquid/). _Layouts_ and _Includes_ that the post relies upon can also access these variables. These variable are generally predefined, but [custom variables can be created](https://jekyllrb.com/docs/front-matter/#custom-variables).

When forming front matter, it must meet the following:
- Placed at the top of the file.
- Be between triple-dashed (`---`) lines.
- Formed from valid YAML.

Here is a basic example:
```YAML
---
layout: post
title: Introduction to Front Matter
---
```


### Categories and Tags 

Posts can be organised using _categories_ and _tags_. While any category or tag can be used, the guidances is:

- [**Categories**](https://jekyllrb.com/docs/posts/#categories) - Represent a broad topic or group of topics that are related. Categories are hierarchical and enable structured content. They can also be [incorporated into the post's generated URL](https://jekyllrb.com/docs/permalinks/#global).
- [**Tags**](https://jekyllrb.com/docs/posts/#tags) - Are keywords that indicate the specific topic(s) a post is about. They do not have a hierarchy.

Categories and Tags are specified in a post's front matter:

```yaml
---
category: Recipes # Single category
categories: Breakfast Lunch Dinner # Multiple categories
tag: Teriyaki Chicken # Single tag
tags: Pancakes Salad Teriyaki-Chicken # Multiple tags
---
```

The `category` and `categories` fields can be used together. Though the `categories` field is used first for category hierarchy. If both `tag` and `tags` are present, Jekyll will only use the `tag` field. If any of these fields are doubled up, the last instance is used.

The following are the different ways that categories and tags can be listed:

```yaml
# Multi-word tags cannot have spaces in this form.
tags: Pancakes Salad Teriyaki-Chicken
# For the following forms all tags are treated the same (the quotation marks are ignored).
tags: ['Pancakes', "Salad", Teriyaki Chicken]
tags:
  - 'Pancakes'
  - "Salad"
  - Teriyaki Chicken
```

[Each directory above a post will also be read-in as a category](https://jekyllrb.com/docs/posts/#categories). For example, any post placed in the `Movie\Comedy\_posts` directory will also get `Movie` and `Comedy` as categories.

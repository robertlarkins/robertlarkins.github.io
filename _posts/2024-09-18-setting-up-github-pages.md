---
layout: post
title: "Setting up GitHub Pages"
date: 2024-09-18
categories: [jekyll, 'github pages']
tag: [setup]
---

# Setting up GitHub Pages

[GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) is a hosting service that hosts a website directly from a GitHub repository. GitHub Pages has three types of sites:
- **project** - site for a specific project (repo)
- **user** - site for a user account
- **organisation** - site for an organisation account
Jekyll is used for implementing these sites, which simplifies their setup. This post provides the steps for setting up a user (or organisation) site.


## Create the Repo

A user site has its own repo. So do the following to [create and initialise the repo](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site#creating-a-repository-for-your-site):
1. Create a new repo named *<your-account-name>.github.io*.
   1. Make the repo public.
   1. Initialise with a README.md.
1. Select the branch to publish from ([alternative instructions](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-from-a-branch)):
   1. Go to the Repo's **Settings**
   1. Go to **Code and automation > Pages** in the side menu.
   1. Set the **Build and deployment** branch to *main* and the folder to *root*.
1. Clone the repo to your local.
1. Create a .gitignore file.
   While a .gitignore file can be added as part of creating the repo, only one template can be selected. The website [toptal.com](https://www.toptal.com/developers/gitignore) allows multiple templates to be selected. Create the .gitignore with the following templates:
   - ruby
   - jekyll
   - rails
1. Commit the changes.
1. Browse to https://&lt;your-account-name>.github.io to see the hosted site (It website may take few minutes to update). At present, this will only display the contents of README.md.


# Add a GitHub Supported Pages Theme

GitHub has themes that it directly supports. A theme can be added to the GitHub page by doing the following:
1. Add a `_config.yml` file to the repo root with the following contents:
   ```yml
   theme: jekyll-theme-minimal
   title: Robert's homepage
   description: A blogging website?
   ```
1. Commit changes.
1. Reload https://&lt;your-account-name>.github.io to see the hosted site with the theme.

The theme `jekyll-theme-minimal` is the example provided in the GitHub pages documentation. This documentation also includes [instructions for getting the names of other supported themes](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll#adding-a-theme).

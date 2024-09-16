# Setting up GitHub Pages

GitHub allows a [GitHub pages sites to be created for an account](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site#creating-a-repository-for-your-site) (user or organisation).


## Create the Repo

The following instructions outline how to create a GitHub pages site for a user:
1. Create a new repo named *robertlarkins.github.io*.
   1. Make the repo public.
   1. Initialise with a README.md.
1. Select the branch to publish from ([further instructions found here](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-from-a-branch)):
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
1. Browse to https://robertlarkins.github.io to see the hosted site. At present, this will only display the contents of README.md.


# Add a GitHub Supported Pages Theme

GitHub has themes that it directly supports. A theme can be added to the GitHub page by doing the following:
1. Add a `_config.yml` file to the repo root with the following contents:
   ```yml
   theme: jekyll-theme-minimal
   title: Robert's homepage
   description: A blogging website?
   ```
1. Commit changes.
1. Reload https://robertlarkins.github.io to see the hosted site with the theme. (It make take a little bit for the website to update.)

The theme `jekyll-theme-minimal` is the example provided in the GitHub pages documentation. This documentation also includes [instructions for getting the names of other supported themes](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll#adding-a-theme).


## Running Locally

The above instructions get the website hosted through GitHub. However, locally hosting the website allows changes to be seen without committing. Before going further, here is a glossary of useful terms:

| Term        | Description     |
|    :---:    | :---            |
| **Gem**     | A Ruby package is called a Gem (like a NuGets in C#). Jekyll itself is a Gem. |
| **Gemfile** | A list of Gems that a project depends on. In Jekyll a Gemfile can also contain the list of Jekyll plugins. |
| **Bundler** | Bundler is a program that reads the Gemfile and generates a Gemfile.lock file. Each Gem in the Gemfile.lock will have a version (even if not specified in the Gemfile). Bundler then downloads and installs the Gems defined in Gemfile.lock so the application can use them. |

To get the GitHub Pages site hosted locally, do the following ([based on these instructions](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)):

1. Install Ruby using [these instructions](https://jekyllrb.com/docs/installation/windows/).
1. Open the terminal and navigate to the root directory of *robertlarkins.github.io* repo and run the following commands:
   1. `bundle init` - [Creates the Gemfile](https://www.bundler.cn/man/bundle-init.1.html).
   1. `bundle add jekyll -v 3.10.0` - Adds Jekyll version 3.10.0 to Gemfile, as [github-pages depends on this version](https://pages.github.com/versions/).
1. Add `gem "github-pages", group: :jekyll_plugins` to the Gemfile. This is based on the [instructions for the setting up the minimal theme](https://github.com/pages-themes/minimal?tab=readme-ov-file#usage).

    The Gemfile will look something like this:
    ```yml
    # frozen_string_literal: true

    source "https://rubygems.org"

    # gem "rails"
    gem "jekyll", "= 3.10.0"
    gem "github-pages", group: :jekyll_plugins
    ```
1. Run the command `bundle install` to create the Gemfile.lock file and install any needed Gems.
1. Run the command `bundle exec jekyll serve` to start the webhost.
1. Browse to `http://127.0.0.1:4000/`.

To get a more extensive setup, follow the instructions here: https://jekyllrb.com/docs/.


### Notes

- What is `group:`? [Reading this page](https://jekyllrb.com/docs/plugins/installation/#the-jekyll_plugins-group) it seems that the line `gem "github-pages", group: :jekyll_plugins` seems to include `github-pages` as part of the `jekyll_plugins` group.

- The CLI command `bundle add <gem>` will add the given Gem to the Gemfile. Adding the `-v` flag allows a version to be specified. The following [version constraints](https://guides.rubygems.org/patterns/#pessimistic-version-constraint) can also be added:

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


# Add Posts

https://jekyllrb.com/docs/posts/



# Add a Jekyll Theme

While GitHub Pages has supported themes, there are many other Jekyll themes that have been created and available for use.

What I'm interested in is a Jekyll theme that supports blog posts. The [Jekyll website links to different theme sites](https://jekyllrb.com/docs/themes/#pick-up-a-theme). There are different ways themes can be added:
- Set using `remote_theme`
- Add as a Ruby Gem
- Forking or directly copying all of the theme files into the project. While this approach allows more specific changes, it will not be explored as it is more difficult to update or change the theme.

Both adding as a Ruby Gem or using `remote_theme` approaches allow local overwrites to parts of the theme via the config and other files.

Using a gem would allow us to pin a particular version, so any changes to the remote-theme will not change our blog. The remote-theme option is specific to GitHub Pages and will keep the blog aligned with the latest version of the remote-theme.

Reference: https://www.richard-stanton.com/2023/01/20/github-pages-remote-theme.html


## Via `remote_theme`

To use the `remote_theme` setting, go to `_config.yml` and replace the `theme` setting with `remote_theme`. It will look like this:

```yaml
remote_theme: OWNER/REPO
```

Where the `OWNER/REPO` value is the path to the theme in GitHub. For example, the `minima` theme is found at https://github.com/jekyll/minima, so the value is:

```yaml
remote_theme: jekyll/minima
```

Note: The [jekyll-remote-theme plugin](https://github.com/benbalter/jekyll-remote-theme) comes bundled with the github-pages gem. Therefore, it does not need to be added separately.

This approach points to the theme in GitHub.

### Specific Version

Referencing the theme as `OWNER/REPO` will use its latest version. [A specific version can be set](https://github.com/benbalter/jekyll-remote-theme?tab=readme-ov-file#declaring-your-theme)) by appending `@<git_ref>`, where `<git_ref>` is one of the following approaches:

| Approach | Example | Notes |
| :---     | :---    | :---  |
| Tag (version) | `jekyll/minima@v1.2.0` | |
| Branch   | `jekyll/minima@minima-next` | `minima-next` is the branch name. |
| Commit   | `jekyll/minima@9350a3b` | This example uses a short git commit hash (usually the first 7 characters of the full hash). The full git commit hash can be used. |

  

remote_theme is specified as a config option here: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll#adding-a-theme
remote_theme plugin: https://github.com/benbalter/jekyll-remote-theme#declaring-your-theme

This approach points to the theme Gem, which might not be as up-to-date as the GitHub repo.


## Via Ruby Gem

A theme not supported by GitHub pages can still be set using the `theme` setting in `_config.yml`. The theme just needs to be added Gemfile:

```yml
gem "THEME-GEM"
```

An example would be the `minima` theme. In `_config.yml` it would be:

```yml
theme: minima
```

while the gem would be set in `Gemfile` as:

```yml
gem "minima"
```

As it is a gem, a specific version can be set using the RubyGems version specifier syntax.

The name of a theme and its gem are usually found in the theme's README.md or associated documentation.

https://jekyll-themes.com/riggraz/no-style-please

Note: Unknown yet if this approach will work for github pages.


# GitHub Actions

https://github.com/just-the-docs/just-the-docs/issues/1273#issuecomment-1594089958


# Future

- How to run Jekyll in a docker container.
- Custom themes. As none of the GitHub supported themes do what I want.

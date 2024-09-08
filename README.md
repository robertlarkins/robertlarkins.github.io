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


# Add a GitHub Pages Theme

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


### Notes

- What is `group:`? [Reading this page](https://jekyllrb.com/docs/plugins/installation/#the-jekyll_plugins-group) it seems that the line `gem "github-pages", group: :jekyll_plugins` seems to include `github-pages` as part of the `jekyll_plugins` group.

- The CLI command `bundle add <gem>` will add the given Gem to the Gemfile. Adding the `-v` flag allows a version to be specified. The following [version constraints](https://guides.rubygems.org/patterns/#pessimistic-version-constraint) can also be added:
  - `~>` - pessimistic operator (aka twiddle-wakka).
  - `!=` - not equal to
  - `<` - less than
  - `<=` - less than or equal to
  - `=` - equal to
  - `>=` - greater than or equal
  - `>` - greater than

Some examples:

| Command | Gemfile Result | Details |
| :---    | :---           | :---    |
| `bundle add jekyll -v 3.10.0` | `gem "jekyll", "= 3.10.0"` | Explicit version. |
| `bundle add jekyll -v "= 3.10.0"` | `gem "jekyll", "= 3.10.0"` | Explicit version. |
| `bundle add jekyll -v '~> 3.10.0'` | `gem "jekyll", "~> 3.10.0"` | Any patch version of 3.10. |
| `bundle add jekyll -v '> 3.9.0, <= 3.10.0'` | `gem "jekyll", "> 3.9.0", "<= 3.10.0"` | Any version greater than version 3.9 and less than or equal to version 3.10. |


# Future

- How to run Jekyll in a docker container.
- Custom themes. As none of the GitHub supported themes do what I want.

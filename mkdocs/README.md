# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Overview
MkDocs is a fast, simple and downright gorgeous static site generator that's geared towards building project documentation. Documentation source files are written in Markdown, and configured with a single YAML configuration file.

### Host anywhere
MkDocs builds completely static HTML sites that you can host on GitHub pages, Amazon S3, or anywhere else you choose.

### Great themes available
There's a stack of good looking themes available for MkDocs. Choose between the built in themes: mkdocs and readthedocs, select one of the 3rd party themes in the MkDocs wiki, or build your own.

### Preview your site as you work
The built-in dev-server allows you to preview your documentation as you're writing it. It will even auto-reload and refresh your browser whenever you save your changes.

### Easy to customize
Get your project documentation looking just the way you want it by customizing the theme.

## Installation

1. Installing Python  
    MkDocs supports Python versions 2.7, 3.4, 3.5, 3.6, 3.7 and pypy.
    ```
    $ python --version
    Python 2.7.2
    ```

1. Installing MkDocs  
    ```
    pip install mkdocs

    $ mkdocs --version
    mkdocs, version 1.0.4
    ```

## MkDocs Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs help` - Print this help message.
* [mkdocs gh-deploy](https://www.mkdocs.org/user-guide/deploying-your-docs/) - Behind the scenes, MkDocs will build your docs and use the `ghp-import` tool to commit them to the `gh-pages` branch and push the `gh-pages` branch to GitHub.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

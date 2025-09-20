# Contributing

## Content Authorship

I'm currently treating this site as a personal project and enjoying the solo authorship that comes with that.

Therefore, I am currently not accepting external contributions to authoring content.

## Technical overview

This website uses MkDocs and MkDocs plugins, which are configured using Python packages.

### Python Virtual Env

Since this site uses Mkdocs (see below), this requires Python.

I recommend using [mise-en-place](https://mise.jdx.dev/) for installing Python and configuring a Python virtual
environment.

### MkDocs

This site uses MkDocs for static site generation. For full documentation visit [mkdocs.org](https://www.mkdocs.org).

Basic commands:

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

### Material for MkDocs

This project also uses [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) for theming and additional
rendering options.

### Awesome Nav for MkDocs

This project also uses [Awesome Nav for MkDocs](https://lukasgeiter.github.io/mkdocs-awesome-nav/) for navigation
tweaks, mainly to do with how directory titles appear.

### Shipping the site

The project uses GitHub Actions for automating the shipping process.

The project is released as a GitHub Pages site. In `Settings > Pages`, this repo has been configured to deploy the
`gh-pages` branch, using the `/` (root) directory.

The custom domain `harmelodic.com` has been configured to be used for this project, and `www.harmelodic.com` as a CNAME
as well.

### Project layout

```
mkdocs.yaml   # The configuration file.
docs/
	index.md  # The documentation homepage.
	...       # Other markdown pages, images and other files.
```

# Contributing

This website uses MkDocs and MkDocs plugins, which are configured using Python packages.

## Python

Configure a Python Virtual Environment in the following way:

```shell
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Once in the Python Virtual Environment, deactivate it by doing:

```shell
deactivate
```

## Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

### Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

### Project layout

```
mkdocs.yaml    # The configuration file.
docs/
	index.md  # The documentation homepage.
	...       # Other markdown pages, images and other files.
```

## Material for MkDocs

This project also uses [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) for theming and additional
rendering options.

## Awesome Nav for MkDocs

This project also uses [Awesome Nav for MkDocs](https://lukasgeiter.github.io/mkdocs-awesome-nav/) for navigation
tweaks, mainly to do with how directory titles appear.

## Deploy as GitHub Pages

In `Settings > Pages`, this repo has been configured to deploy the `gh-pages` branch, using the `/` (root) directory.

The custom domain `harmelodic.com` has been configured to be used for this project.

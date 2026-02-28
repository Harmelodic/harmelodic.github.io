# harmelodic.github.io

My personal website, found at [harmelodic.com](https://harmelodic.com).

See [CONTRIBUTING.md](./CONTRIBUTING.md) for contribution policy.

## Technical overview

This website uses MkDocs and MkDocs plugins, which are configured using Python packages.

### Python Virtual Env

Since this site uses MkDocs (see below), this requires Python.

I recommend using [mise-en-place](https://mise.jdx.dev/) for installing Python and configuring a Python virtual
environment. A `mise.toml` file is provided to make this easier.

### MkDocs

This site uses MkDocs for static site generation. For full documentation visit [mkdocs.org](https://www.mkdocs.org).

Basic commands:

- `mkdocs serve --livereload` - Start the docs server locally, and reload content when files change.
- `mkdocs build` - Build the documentation site.
- `mkdocs --help` - Print help message.

This project also uses some MkDocs plugins:

- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) for theming and additional rendering options.
- [Awesome Nav for MkDocs](https://lukasgeiter.github.io/mkdocs-awesome-nav/) for navigation tweaks, mainly to do with
  how directory titles appear.

### Markdown linting

I'd love to sort out getting a formatter instead of a linter, but here we are. Lint the Markdown using `markdownlint`,
specifically I recommend using `markdownlint-cli2`:

```bash
markdownlint-cli2 **/*.md
```

TODO: Remove this and replace with just a CI check. Editor extensions exist to aid DX.

### Shipping (Build & Release)

The project uses GitHub Actions for automating the shipping process.

The project is released as a GitHub Pages site. In `Settings > Pages`, this repo has been configured to deploy the
`gh-pages` branch, using the `/` (root) directory.

The custom domain `harmelodic.com` has been configured to be used for this project, and `www.harmelodic.com` as a CNAME
as well.

### Project layout

```text
.github/         # GitHub Actions code to build & release the project.
docs/            # The site content.
docs/CNAME       # Configuration file for configuring CNAME for GitHub Pages site.
.gitignore       # Configuration file for Git to know which files and directories to ignore.
.python-version  # Configuration file to define which Python version to use for this project (MkDocs requires Python).
CODEOWNERS       # Configuration file for GitHub code ownership / access control.
CONTRIBUTING.md  # Docs on how to contribute to this project.
mise.toml        # Configuration file for mise-en-place.
mkdocs.yaml      # Configuration file for MkDocs.
README.md        # The README for project information.
renovate.json    # Configuration file for Renovate.
requirements.txt # Project dependency file for Python.
```

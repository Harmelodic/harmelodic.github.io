# What is... Homebrew (the macOS package manager)

> Originally published: 25 May 2019

Homebrew is a third party, open source [package manager](https://en.wikipedia.org/wiki/Package_manager) for macOS.

This adds a third, "standard" method of installing software onto machines running macOS.  
The other 2 methods being:

- Install from the App Store, which requires developers to pay for being part of the Apple Developer Program, as well as
  require developers to get their software verified by Apple.
- Install from a `.pkg` or `.dmg` file, which doesn't require any verification or payment, but does require the software
  to have an system within itself to be able to update itself; Otherwise the user has to manually download new versions
  of the software every time they want to update.

## How it works

Upon installing Homebrew, like with other package managers, Homebrew comes with an in-built list of repositories that it
can pull software from for users to easily install software onto their machine with a single command.

## Terminology

### Formulae

A formula (plural = formulae) is a package definition file containing the instructions for installing a piece of
software onto your machine.  
This software is _usually_ a program (i.e. a CLI tool).

### Tap

A tap is a repository that Homebrew can pull software from.

### Cask

A cask is a package definition file containing the instructions for installing a piece of software onto your machine.
This software is _usually_ an application (i.e. has a GUI)

## Installing Software

To install software using a formula:

```bash
brew install <formula>[@<version>]
```

To add a new tap:

```bash
brew tap <tap>
```

To install software using a cask:

```bash
brew cask install <cask>
```

## Upgrading

To update the list client-side list of repositories:

```bash
brew update
```

To upgrade formulae:

```bash
brew upgrade [<formuala>]
```

To upgrade casks:

```bash
brew cask upgrade [<cask>]
```

## Listing installed software

Formulae:

```bash
brew ls
```

Casks:

```bash
brew cask ls
```

## Removing Software

To remove software using a formula:

```bash
brew uninstall <formula>
```

To remove a tap:

```bash
brew untap <tap>
```

To remove software using a cask:

```bash
brew cask uninstall <cask>
```

## Advanced Commands

Fetch a non-shallow copy of all homebrew/core (homebrew/core is the core tap) formulae:

```bash
git -C "$(brew --repo homebrew/core)" fetch --unshallow
```

# Sublime Configurations

I use sublime to write codes and edit latex files.
I record some configurations in this repository.

## Basic Editing Settings

To enable vintage mode and set 'jk' key binding, follow [this](https://www.sublimetext.com/docs/3/vintage.html).

Translate all tabs into 4 spaces

```
"detect_indentation": true,
"tab_size": 4,
"translate_tabs_to_spaces": false
```

Enable spell check

```
"spell_check": true
```

## Latex

### Compilation

First, a latex compiler is required. On Linux, pdflatex is installed by default. On OS X, I usually install MacTex.

Before installing MacTex, install [Homebrew](https://brew.sh) first. After that, run

```
brew cask install mactex
```

which takes a bit time.

Now you can use `pdflatex` to compile `.tex` file in the command line (remember to open a new terminal).

I use [LaTexTools](https://latextools.readthedocs.io/en/latest/) to compile `.tex` files in sublime. Remember to write `%!TEX root = <master file name> ` at the beginning of every `.tex` file. After installation, change the builder to `simple`. Now you can use `cmd + b` to compile the `.tex` files.

### Completion

Install [LaTeX-cwl](https://packagecontrol.io/packages/LaTeX-cwl).

## Python

### Python Linter
- First, install [SublimeLinter](http://www.sublimelinter.com/en/stable/) and [SublimeLinter-flake8](https://github.com/SublimeLinter/SublimeLinter-flake8) in sublime via package control.
- Next, install [flake8](https://flake8.pycqa.org/en/latest/) in the system.
- Finally, configure the path of flake8 in SublimeLinter's setting.

You might need to *restart* sublime after the installation.
The linter works automatically whenever a file is saved.

## Some Awesome Plug-ins

[MarkdownPreview](https://packagecontrol.io/packages/MarkdownPreview)

[Terminus]


## Latex

### Compilation

First, a latex compiler is required.
On Linux, pdflatex is installed by default.
On OS X, I usually install MacTex.

Before installing MacTex, install [Homebrew](https://brew.sh) first.
After that, run
```
brew cask install mactex
```
which takes a bit time.

Now you can use `pdflatex` to compile `.tex` file in the command line (remember to open a new terminal).

I use [LaTexTools](https://latextools.readthedocs.io/en/latest/) to compile `.tex` files in sublime. Remember to write `%!TEX root = <master file name> ` at the beginning of every `.tex` file.
After installation, change the builder to `simple`. Now you can use `cmd + b` to compile the `.tex` files.

### Completion

Install [LaTeX-cwl](https://packagecontrol.io/packages/LaTeX-cwl).

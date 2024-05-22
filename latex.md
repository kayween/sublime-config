## Latex

### Compilation

A latex compiler is required.

#### Compilation in Terminals

- Linux. pdflatex is installed by default.
- OS X. MacTex can be installed by [Homebrew](https://brew.sh) using the command `brew cask install mactex`.

After the installation, `pdflatex` can be used to compile `.tex` file in the terminal.

#### Compilation in Sublime

Install [LaTexTools](https://latextools.readthedocs.io/en/latest/).
After the installation, change the builder to `simple`.
Use `cmd + b` to compile `.tex` files.

Remember to write `%!TEX root = <master file name> ` at the beginning of every `.tex` file.

### Completion

Install [LaTeX-cwl](https://packagecontrol.io/packages/LaTeX-cwl).
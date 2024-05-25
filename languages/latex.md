# Latex
We introduce how to compile latex documents in sublime.

## Requirements
A latex compiler is required---sublime plugins do not provide latex compilers.

- Linux. `pdflatex` is installed by default.
- Mac. Install MacTex by [Homebrew](https://brew.sh) using the command `brew cask install mactex`.

Test the installation by `pdflatex filename.tex` in the terminal.

## Compilation

Install [LaTexTools](https://latextools.readthedocs.io/en/latest/) using package control.
Compile `.tex` files by `ctrl + b` (linux) or `cmd + b` (mac).

If you have multiple latex source files and want to configure the root source file, there are two options:

- Add `%!TEX root = <path_to_root_tex_file> ` at the beginning of every `.tex` file.
- Create a `.sublime-project` file and add the following
```
"settings": {
    "TEXroot": "main.tex"
}
```
This solution option is attributed to this [post](https://tex.stackexchange.com/questions/174886/how-to-set-main-file-in-latextools-with-sublime-text-3-editor). Despite the [documentation](https://latextools.readthedocs.io/en/latest/settings/#builder-settings) claims that one should do `latextools.tex_root: "main.tex`, it does not work.

### Trouble Shooting
1. A python interpreter is required for post-compilation operations. If your python is installed in a non-standard location, you should add the python path in LatexTools' setting
```
"linux" : {
    // Command to invoke Python. Useful if you have Python installed in a
    // non-standard location or want to use a particular version of python.
    // Both Python2 and Python3 are supported, but must have the DBus bindings
    // installed.
    "python": "path/to/python",
```

## Completion

Install [LaTeX-cwl](https://packagecontrol.io/packages/LaTeX-cwl).
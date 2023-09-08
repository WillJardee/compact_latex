# Make

[GNU Make](https://www.gnu.org/software/make/) is a tool that helps automate command line usages. You can think of it as writing your own shell script (.sh), but allowing quality of life improvements like easier switching functions (this is the primary use of it here). 

You will find the relevant file in the Makefile and can be run with 
`make <OPTION>`
If `<OPTION>` is left blank, then we will use the Makefile's "default" value. In the Makefile given here, the default is pdf, which runs `latexmk -pdf`

## TODO
- [ ] Evaluate if we want to be using `latexmk` or `lualatex` or `xelatex`
- [ ] Expand the make file for more options; i.e., double compilation for bib files, format checking, spell checking, etc.
- [ ] Make that runs given file


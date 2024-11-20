## Compact Latex
Collection of starter sty, cls, tex, and bib examples.

## Adding to MikTex
Most of these styles should be usable out of the box. The TEXMF root directory
must be registered. If you are using MikTex, as I do on my Windows machine, you
need to [register it through the
console](https://tex.stackexchange.com/questions/1137/where-do-i-place-my-own-sty-or-cls-files-to-make-them-available-to-all-my-te).
You only want to register the `mytexmf` folder.

## Adding to Texlive
This is going to depend on your device specific distribution tree. A good guide
can be found through the [Arch Linux wiki](wiki.archlinux.org/title/TeX_Live),
and a ton of information can be gathered both from [TEX
FAQ](https://texfaq.org/) and [Reddit](https://www.reddit.com/r/LaTeX/). 

To get TeXLive to recognize your `.sty` file, you need to put them onto the path it searches. You can test for where that is with 
```sh
kpewhich -var-value TEXMFLOCAL
```
for system wide install and 
```sh
kpewhich -var-value TEXMFHOME
```
for user specific files. If these are not defined, you might need to manipulate your [variables](https://wiki.archlinux.org/title/TeX_Live#texmf_trees_and_Kpathsea) (I prefer to use [XDG Base Directories](https://wiki.archlinux.org/title/XDG_Base_Directory)).

As long as the files end up in this path, you're fine. You can copy the files
directly into this base directory or subdirectory, clone this repository
directly into this directory, or symlink it in. I personally prefer the last
option.
1. Clone this repo into somewhere convenient for you.
1. Link all the files into `TEXMFHOME`
```sh 
mkdir -p $TEXMFHOME/texmf/latex
ln -s /path/to/compact_latex/mytexmf/tex/latex/* $TEXMFHOME/texmf/latex/
```

If you only care about one project, you might want to copy the raw files over. The current directory should be added to the search path. So, if you copy the `.sty` file or directory into the working directory it should find it. 

The best solution I currently have for Overleaf is to take the last strategy 
  and copy it into the highest level directory of the Overleaf project.

## TODO:

- [X] Add Makefile direction
- [ ] Add Overleaf directions
- [ ] Add external links
  - [List of LaTeX mathematical symbols](https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols)
  - [Latex Tutorial](https://latex-tutorial.com/)
  - [A Not so Short Introduction to Latex](https://tobi.oetiker.ch/lshort/lshort.pdf)
- [ ] Add comprehensive "Get Latex Running" section
- [ ] Add math examples
- [ ] Scour the internet for more styles
- [ ] Add poster examples
  - [ ] [Better Poster](https://github.com/rafaelbailo/betterposter-latex-template)
- [ ] Develop file hierarchy visual

## LATEX notes
- add `\tracinglostchars=3` to preamble
- `draft` toggle to document

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
can be found through the [Arch Linux wiki](wiki.archlinux.org/title/TeX_Live). 

There are a couple different ways you can do this. 
- The first would be to copy all compact_latex directly into the texmf dir. 
  This approach is good if you do not update often or if you are trying things. 
  ```
  mkdir -p ~/texmf/tex/latex/compact_latex
  cp <path to the repo>/texmf/tex/latex/* ~/texmf/tex/latex/compact_latex/*
  ```
- The second would be to create symbolic link to the repo. This choice allows 
  `git pull` to update the repo and update your tex path.
  ```
  mkdir -p ~/texmf/tex/latex/
  ln -s <path to the repo>/texmf/tex/latex/ ~/texmf/tex/latex/compact_latex
  ```
  This creates a (soft) symbolic link from the GitHub repo to compact_latex. 
  Beware that if you move the repo, you will likely have to update where the 
  symbolic links to. The easiest way to do this is to remove the link
  ```
  rm ~/texmf/tex/latex/compact_latex
  ```
  then update it with the same process as you did the first time.
- The third way is good if you want to rip only a single file for a project. 
  Navigate to the .sty you wish to use from 
  `<path to the repo>/texmf/tex/latex/` and copy it to the current working dir
  of your tex project.
 
 
The best solution I currently have for Overleaf is to take the third strategy 
  and copy it into the highest level directory of the Overleaf project.

## TODO:

- [ ] Add Makefile direction
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

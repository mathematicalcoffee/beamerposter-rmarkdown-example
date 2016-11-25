# Sample beamerposter to compile with Rmarkdown

Based off the [Computational Physics and Biophysics Group, Jacobs University template with modifications by Nathaniel Johnston and Vel Gayevskiy](https://www.overleaf.com/latex/templates/landscape-beamer-poster-template/vjpmsxxdvtqk).

At the moment it consists of an example Rmd file, a pandoc template for beamer, and the beamer style file.
It compiles from R, but doesn't actually support the knitr yet (oops).

The idea is that it will be much more standalone - remove the custom colour definitions, custom boxes (well check - they might look worse) and basically everything except the custom headline? (Maybe I don't even need that?) - you could just use your favourite beamer theme, right? The main thing I need to do is mod the font sizes. So perhaps reduce the number of files needed?

TODO:

* horizontal and portrait posters, with extra colour themes:
  - CSIRO colours
  - default is something prettier than this.
* hierarchical? ie beamerthemebase, then an add-on for the orientation (or perhaps that's in the template not the theme), and colour themes for CSIRO/other.
* pure tex versions (rather than markdown)
* pure pandoc version (rather than *r*markdown)
* presentations

Separation of colour, font, inner, and outer themes: http://tex.stackexchange.com/questions/270112/designing-a-beamer-theme-from-scratch-what-to-put-in-inner-outer-color also Section 15.1 in beameruserguide.

.... too much to do!
* https://github.com/martinbjeldbak/ultimate-beamer-theme-list
* https://cran.r-project.org/web/packages/viridis/vignettes/intro-to-viridis.html
* quickstart to colour themes http://ramblingacademic.com/2015/12/how-to-quickly-overhaul-beamer-colors/

Specific todo

* biblio
* graphicspath
* logo

## YAML frontmatter

### Title, authors, institutions

```
title: Unnecessarily complicated research title
author:
    - name: The Student
      affiliation: [1, 2]
    - name: The Slavedriver
      affiliation: 1
    - name: Secondary Slavedriver
      affiliation: 2
affiliation:
    - key: 1
      name: Department of studying and research, University of PhD
    - key: 2
      name: Institute for grant money
```

Here is how you have multiple authors with their affiliations footnoted.
They appear in that order. The `key` is the symbol used for the institution, and has to match that given in `affiliation`.

Logos: **todo**.

### Template configuration

The following are available. May be ommitted to have the defaults.

```
beamerconfig:
    blocks:
        rounded: true # default true
        shadow: true # default false
papersize: a0 # default A0
columnwidth: '0.3\paperwidth' # default is 0.3\paperwidth
```

The macro `\colwid` is available for use within the template, and the `calc` package allows for further specifications like `2\colwid`.
It is set to `0.3\paperwidth` by default but you can override it in the YAML frontmatter if you like.

## Markdown

One problem is that when writing beamer, a lot of `\begin{}` and `\end{}` are used (block, column, ...) and pandoc will not parse text inside a block. Since the entire poster is usually in columns and blocks, this means you end up writing the whole poster in latex anyway, defeating the purpose of using pandoc. To get around this, we use the workaround in [jgm/pandoc #2453](https://github.com/jgm/pandoc/issues/2453#issuecomment-219233316) and provide aliases `\Begin{}` and `\End{}`, and the contents of these *will* be parsed as markdown/pandoc.

However if the begin/end block has an optional argument (e.g. `\Begin{figure}[t]`, this will not be properly treated; rather, the trailing `[t]` will be treated as literal text by Pandoc. In cases like these you need to use `\nopandoc` (which is just the identity command) as in `\nopandoc{\begin{figure}[t]}`, which (surprisingly) works.

A number of other unpleasant side-effects of parsing within LaTeX are that if you have comments using `%`, they'll be parsed as literal percent signs by pandoc; pandoc recognises the HTML-style comments only.

And also if you indent your latex/text for readability within blocks, if it's indented more than 4 spaces it'll probably get treated as a code block. So unindent it :/

## Troubleshooting

### Font problems (mathkerncmssi)

```
!pdfTeX error: pdflatex (file mathkerncmssi17): Font mathkerncmssi17 at 864 not
 found
 ==> Fatal error occurred, no output PDF file produced!

kpathsea: Running mktexpk --mfmode / --bdpi 600 --mag 1+264/600 --dpi 864 mathkerncmssi17
mktexpk: don't know how to create bitmap font for mathkerncmssi17.
mktexpk: perhaps mathkerncmssi17 is missing from the map file.
kpathsea: Appending font creation commands to missfont.log.
```

This worked:

```
./updmap-sys --enable Map sansmathaccent.map
```

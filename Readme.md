# Sample beamerposter to compile with Rmarkdown

Based off the [Computational Physics and Biophysics Group, Jacobs University template with modifications by Nathaniel Johnston and Vel Gayevskiy](https://www.overleaf.com/latex/templates/landscape-beamer-poster-template/vjpmsxxdvtqk).

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

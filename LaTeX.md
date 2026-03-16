## Introduction

LaTeX is a software system for typesetting documents. LaTeX is widely
used in academia for the communication and publication of scientific
documents and technical note-taking in many fields, owing partially to
its support for complex mathematical notation.[^1]

This page is for general info, how to install and use it.

## Installation

### Linux

#### Prerequisites

- Internet

#### Methods / Possibilities {#methods_possibilities}

It is possible to install TeX Live[^2] directly from the source or most
distributions have a packet in their package manager (recommended).

##### Installation from package manager {#installation_from_package_manager}

``` bash
# Install with your package manager (lite version)
## Example Ubuntu
sudo apt install texlive

## It is also possible to install texlive with all the latest packages (that your distro offers)
sudo apt install texlive-full

## If you realy need the latest packages from the TeX Live then use the "Installation from source" instructions otherwise it is recomended to use your package manager instead installing it from source
```

##### Installation from source {#installation_from_source}

``` bash
# How to install and run Latex on one node

## source: https://tug.org/texlive/quickinstall.html

sudo su -

cd /tmp
curl -L -o install-tl-unx.tar.gz https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
zcat < install-tl-unx.tar.gz | tar xf -
cd install-tl-*
perl ./install-tl --no-interaction

## change to normal user
exit

# Add to ~/.bashrc or ~/.zshrc
export PATH=/usr/local/texlive/2025/bin/x86_64-linux:$PATH
export MANPATH=/usr/local/texlive/2025/texmf-dist/doc/man:$MANPATH
export INFOPATH=/usr/local/texlive/2025/texmf-dist/doc/info:$INFOPATH

## Refresh shell or log out and back in again to take affect and check
source ~/.bashrc   # or ~/.zshrc

# Check installation
which pdflatex # should be /usr/local/texlive/2025/bin/x86_64-linux/pdflatex
which latex # should be /usr/local/texlive/2025/bin/x86_64-linux/latex
pdflatex --version

# Update packages only as root user
tlmgr init-usertree #check again!!!!
tlmgr update --self
tlmgr update --all
```

[^3]

##### Usage

``` bash
# Compile .tex file into PDF
pdflatex documentation.tex

# if glossaries are used in .tex file do this

pdflatex documentation.tex
makeglossaries documentation
pdflatex documentation.tex
```

## References

<references />

[index.php?title=Category:LaTeX](index.php?title=Category:LaTeX "index.php?title=Category:LaTeX"){.wikilink}
[index.php?title=Category:TeX
Live](index.php?title=Category:TeX_Live "index.php?title=Category:TeX Live"){.wikilink}

[^1]: <https://en.wikipedia.org/wiki/LaTeX>

[^2]: <https://www.tug.org/texlive/>

[^3]: <https://tug.org/texlive/quickinstall.html>

pandoc-novel
=============

This is a template project for writing a novel or story collection as markdown files and building it into both an ePub file and a printable PDF using [Pandoc](https://pandoc.org), [LaTeX](https://www.latex-project.org/get/), and [GNU Make](https://www.gnu.org/software/make/).   This project is a distillation of the code I used to build [my story collection](https://jp-fosterson.medium.com/the-plunge-pool-stories-by-me-8b0c5570df37), and was partly inspired by [Dan Grec's post on how he created his book for publication using LaTeX](http://theroadchoseme.com/how-i-self-published-a-professional-paperback-and-ebook-using-latex-and-pandoc).

You can see example output for[PDF](doc/example.pdf), and [ePub](doc/example.epub).

Set Up
-------

### Install prequisites

* [Pandoc](https://pandoc.org)
* [LaTeX](https://www.latex-project.org/get/) --- Installing LaTeX is beyond the scope of this document, but I use [MacTex](https://www.tug.org/mactex/) on macOS.
* the [`createspace` package](https://github.com/aginiewicz/createspace).  On the Mac, you can `git clone` that repo into `$HOME/Library/texmf/tex/latex/createspace/`. 

### Clone this repo

    git clone git@github.com:jp-fosterson/pandoc-novel.git

### Try it out!

    cd pandoc-novel
    make

This will build two targets `out/example.epub` and `out/example.pdf`.  Open them in your favorite viewers. 

If you're on macOS  `make view-epub` will attempt to build the `epub` target and open the file in [Calibre's](https://calibre-ebook.com) ebook viewer component (without actually adding the book to your library).  Likewise `make view-pdf` will build the `pdf` target and open it with the default PDF viewer (probably `Preview`).  If you're not on macOS, you can edit these targets in the `Makefile` to open the documents in your favorite viewers.

Writing!
--------

### `text/`

The most important part of the novel, the text, goes in markdown-format chapter files in the `text/` directory.  Aside from the `.md` extension, there are no particular requirements about the file names, chapter ordering will be defined in the [`Makefile`](/Makefile) (see below).  Chapter titles should be indicated with a `H1`-level header.  If you wish your chapters to be unnumbered, tag them with `{.unnumbered}` after the title.   Backmatter such as acknowledgements and an author bio should just be included as `{.unnumbered}` chapters; they will be indicated in the [`Makefile`](/Makefile).

### `Makefile`

Set the `SLUG` variable in the Makefile to change the prefix used on the output files

```
#
# Slug: filename prefix that will be used for the generated
# files. e.g. example.epub and example.pdf, plus intermediate
# files like example.tex
#
SLUG=example
```


To define the chapter ordering add the chapter files, in order, in the `CONTENTS` and `BACKMATTER` variables in [`Makefile`](/Makefile):

```
#
# The main text of the book. This sequence determines the
# chapter order.
#
CONTENTS = \
	text/beginning.md\
	text/middle.md\
	text/end.md\
	\
#
# The stuff in the back, not part of the story.
#
BACKMATTER = \
	text/ack.md \
	text/about.md\
	\
```

### `metadata.yaml`

The title, author, copyright notice, ePub cover image, and other important metadata are defined in [`metadata.yaml`](/metadata.yaml).

###  `make` targets

`make clean` --- cleans up everything, including the output and all the LaTeX shrapnel left in the directory after building.

`make tkcheck` --- searches for "TK" in the text and fails if it finds any.  [Editors use "TK" to indicate more "to come](https://en.wikipedia.org/wiki/To_come_(publishing)", i.e. unfinished writing.  You can do `make tkcheck all` to build the documents only once all TKs are removed.


Customization
--------------

TBD





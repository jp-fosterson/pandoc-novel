pandoc-novel
=============

This is a template project for writing a novel or story collection as markdown files and building it into both an ePub file and a printable PDF using [Pandoc](https://pandoc.org), [LaTeX](https://www.latex-project.org/get/), and [GNU Make](https://www.gnu.org/software/make/).   This project is a distillation of the code I used to build [my story collection](https://www.amazon.com/Plunge-Pool-Stories-JP-Fosterson/dp/B0939ZGC3G/), and was partly inspired by [Dan Grec's post on how he created his book for publication using LaTeX](http://theroadchoseme.com/how-i-self-published-a-professional-paperback-and-ebook-using-latex-and-pandoc).  

Well formatted ePub and PDF files should be enough for publishing ebooks and print-on-demand books with [Amazon KDP](https://kdp.amazon.com), [Barnes and Noble Press](https://press.barnesandnoble.com), [Smashwords.com](https://smashwords.com), and other self-publishing outlets.  (Well, that and a decent-looking cover, but cover design, especially for print books, is beyond the scope of this template.)

You can see example output for [PDF](doc/example.pdf), and [ePub](doc/example.epub).

Set Up
-------

### Install prequisites

* [Pandoc](https://pandoc.org)
* [LaTeX](https://www.latex-project.org/get/) --- Installing LaTeX is beyond the scope of this document, but I use [MacTex](https://www.tug.org/mactex/) on macOS.
* the [`createspace` package](https://github.com/aginiewicz/createspace).  On the Mac, you can `git clone` that repo into `$HOME/Library/texmf/tex/latex/createspace/`. 

### Clone this repo

    git clone git@github.com:jp-fosterson/pandoc-novel.git

### Try it out

    cd pandoc-novel
    make

This will build two targets `out/example.epub` and `out/example.pdf`.  Open them in your favorite viewers. 

If you're on macOS  `make view-epub` will attempt to build the `epub` target and open the file in [Calibre's](https://calibre-ebook.com) ebook viewer component (without actually adding the book to your library).  Likewise `make view-pdf` will build the `pdf` target and open it with the default PDF viewer (probably `Preview`).  If you're not on macOS, you can edit these targets in the `Makefile` to open the documents in your favorite viewers.

Writing
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

`make tkcheck` --- searches for "TK" in the text and fails if it finds any.  [Editors use "TK" to indicate more [to come](https://en.wikipedia.org/wiki/To_come_(publishing)), i.e. unfinished writing.  You can do `make tkcheck all` to build the documents only when all TKs are removed.]

`make unzip` --- Builds the ePub document, which really just a zip archive full of files, and then unzips it into `out/$(SLUG).unzip` so that you can examine the contents.  This is fun for the curious, or if you need to understand the style classes used in the document when modifying the stylesheet (see below).

Customization
--------------

You can customize the style of the generated ePub via CSS stylesheets, and the PDF via the LaTeX document template.

### ePub Stylesheets

The `STYLESHEET` variable in the [`Makefile`](/Makefile) selects the stylesheet to use when building the ePub document.  Two stylesheets are provided in the [`/css`](/css) directory, one with an indented paragraph style and one with a block-paragraph style.  Either can be used as a starting point for a new style.  If you're not sure what style classes you need to modify, do `make unzip` then examine the `chXXX.xhtml` files in `out/$(SLUG).unzip/EPUB/text`.


### LaTeX Template

The LaTeX file that generates the PDF is build from a template in `templates/book.tex` it uses [Pandoc template syntax](https://pandoc.org/MANUAL.html#templates).  The title page design was taken from [Peter Wilson's great collection of LaTeX title pages](http://tug.ctan.org/info/latex-samples/TitlePages/titlepages.pdf).   The page header and numbering are controlled by the [fancyhdr package](https://texblog.org/2007/11/07/headerfooter-in-latex-with-fancyhdr/).


Publishing
-----------

### Ebooks

[Amazon KDP](https://kdp.amazon.com) will take an ePub doc and covert it to a Kindle book, and it does a fairly good job.  If you're curious what it looks like, you can get [Kindle Previewer](https://www.amazon.com/gp/feature.html?docId=1000765261&ie=UTF8), and other ebook sites take ePubs directly.

### Print

I have only tried KDP, but the KDP site does a pretty good job of showing how your PDF will fit in different page trim sizes.  I found that despite using the "pocket" size parameter in the `createspace` package, it fits well in 6x9\" with good size margins (IMO). 

Cover Design
------------

For the artistically challenged like me, [Canva](https://canva.com) provides nice starting templates for eBook covers.  Amazon provides a [cover designer](https://kdp.amazon.com/en_US/help/topic/G201113520) for print books that takes care of computing the spine width properly.  Its editor leaves a lot to be desired, but I thought it was worth it to not have to do the sizing manually.



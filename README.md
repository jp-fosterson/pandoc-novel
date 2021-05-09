pandoc-novel
=============

This is a template project for generating a novel or story collection from markdown files and building both an ePub file and a printable PDF using [Pandoc](https://pandoc.org), [LaTeX](https://www.latex-project.org/get/), and [GNU Make](https://www.gnu.org/software/make/).   This project is a distillation of the code I used to build [my story collection](https://www.amazon.com/Plunge-Pool-Stories-JP-Fosterson/dp/B0939ZGC3G/), and was partly inspired by [Dan Grec's post on how he created his book for publication using LaTeX](http://theroadchoseme.com/how-i-self-published-a-professional-paperback-and-ebook-using-latex-and-pandoc).  

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

...or fork the repo (it's a template), and clone your own copy.

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

`make` or `make all` --- makes the main ePub and PDF targets in `out/`.

`make pdf`, `make epub` --- do what they say

`make unzip` --- Builds the ePub document, which really just a zip archive full of files, and then unzips it into `out/$(SLUG).unzip` so that you can examine the contents.  This is fun for the curious, or if you need to understand the style classes used in the document when modifying the stylesheet (see below).

`make clean` --- cleans up everything, including the output and all the LaTeX shrapnel left in the directory after building.

`make tkcheck` --- searches for "TK" in the text and fails if it finds any.  [Editors use "TK" to indicate more [to come](https://en.wikipedia.org/wiki/To_come_(publishing)), i.e. unfinished writing.  You can do `make tkcheck all` to build the documents only when all TKs are removed.]


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


A Gratuitous Note on Gratuitous Typography
---------------------------------------------

This example, especially for the print version, reflects a set of simple typographic choices designed to achieve a basic standard of readability: Pagella/Palatino font throughout, 1.5 line spacing for the text, centered chapter titles, simple centered page header and footer, etc. 

I arrived at these choices, to be honest, by pulling a bunch of novels off my shelf, looking at the pages, and trying to emulate what I saw there, but the choice to stay simple was one I made before I started.  As practical matter, of course, it makes sense for this example to shoot for a least-common-denominator from which users can branch off in different directions according to their tastes.  But, frankly, I think most such typographical branchings are a waste of effort.  I understand how a writer might want to do titles and headers in a futuristic sans-serif font for a sci-fi novel, or an old-fashioned font in historical fiction, but is it worth it?  For fiction---storytelling in written prose---the cost of _not_ making such embellishements is nil.  If your prose is doing its job then the reader won't notice the typography, as long as you've met a basic threshold of readability and it doesn't look weird.

Consider the great, classic novels. For me these would be things like _Pride and Predjudice_, _Moby Dick_, _The Adventures of Huckleberry Finn_, _The Awakening_, _Gatsby_, _The Left Hand of Darkness_, _Stranger in a Strange Land_, and others.  For you, maybe other works, but surely there are _some_.  These books, especially the older ones, have been reprinted and retypeset many times, even released as ebooks, where the typography is fluid, and yet their greatness remains.  The greatness is in the words, not the glyphs.  Some fiction is so great that its greatness survives not just being typeset differently, but even being translated into different languages that use different alphabets!  (Murakami and the great Russians come to mind)

For an indie author, the effort required to go beyond basic, good-looking, inoffensive readability is almost always better spent on writing.  This is my whole reason for writing in Markdown instead of using a WYSIWYG word processor: the formatting is irrelevant to the story. Pedants will find counterexamples, of course, where some typographical aspect of some book or other is crucial to the story, but those are exceptions.  Moreover, futzing with word processor settings (and the like) is one of the great sinks of procrastination for writers.  Just stop it and get back to writing.  

Meanwhile, if you do get fancy, you risk making bad typographic choices that distract the reader from your story.  Right now I'm reading a contemporary novel from a small press. It uses a traditional serif font for the main text, a typewriter font for some parts of the text that are composed as snippets of screenplay, a modern-looking sans-serif for the page header and footer and _some_ of the chapter titles, a caligraphic font for the remaining chapter and subchapter titles, and a different caligraphic font for the title pages of the major parts of the book.  The result feels to me as if the typesetter was trying to justify his salary.  There might be a meaning to the different chapter title fonts, but I'm damned if I can figure out what it is. Trying to deduce is just distracts me from staying immersed in the story.

Every rule has exceptions, of course.  Whoever typsets Tolkien will have to figure out how to deal with the [Tengwar](https://en.wikipedia.org/wiki/Tengwar).  If you make up a new alphabet for your story, then you'll have to to typeset it.  But if you're not doing something exceptional, you're better off thinking about your characters and what they'll be doing than your fonts, margins, spacing, and the like.


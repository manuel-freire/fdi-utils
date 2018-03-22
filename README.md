# fdi-utils

Scripts to improve QoL at FdI UCM

## Goal

Keep a series of useful scripts for [FdI UCM](https://informatica.ucm.es/) teachers. Currently oriented towards two workflows:

* wrangling with Moodle submissions, generating feedback for students in markdown files, and generating pretty pdfs from those files to upload as feedback into Moodle:

    1. [normalize-cv-subs](#normalize-cv-subs) handles uncompressing and, uh, normalizing the submissions; and can create a grading template for you.
    2. [split-by-headers](#split-by-headers) can split the grading template into individual files, so that you can easily convert them to individual feedback pdfs, one per student
    3. [markdown-to-pdf](#markdown-to-pdf) can generate those individual feedback pdfs for you

* generating nice-looking pdfs and slides from markdown, for use as class notes and slides:

    - [markdown-to-pdf](#markdown-to-pdf) generates nice class-notes style pdfs from markdown
    - [markdown-to-beamer](#markdown-to-pdf) generates beamer slides (as pdfs) from markdown

Why use all this markdown-centric workflow? Because many of the subjects and assignments at FdI UCM deal with writing code, and placing code into documents and slides is generally much harder (and looks uglier) than it should be. These scripts can generate beautiful, syntax-highlighted code from snippets:

``` {.md}
~~~ {.java}

    try {
        assertTrue("Tests basicos pasan", mainBasico.test("src/test/resources/basic"));
        assertTrue( "Tests avanzados pasan", mainBasico.test("src/test/resources/advanced"));
        mainBasico.test("src/test/resources/err");
        fail("no da error con una entrada mala");
    } // ...

~~~ 
```

Output:

![sample output](https://user-images.githubusercontent.com/2271676/37787542-81b3dc18-2dff-11e8-92ce-61a31b1bd2ea.png)

## normalize-cv-subs

Python 2.7 script that normalizes Moodle (Campus Virtual) submission names, uncompresses them where you choose, and optionally creates a grading template in Markdown to provide feedback to the students. For example, a submission named

`ALFREDO JAVIER BLÁZQUEZ LÓPEZ_2413645_assignsubmission_file_Entrega Práctica 1.7z`

would be extracted into a folder named

`alfredo_javier_blázquez_lópez-entrega-práctica-1/`

#### Requirements

python-magic, pyunpack, patool, unzip, unrar, p7zip-full

#### Usage

(output of `./normalize-cv-subs -h`)

~~~
usage: normalize-cv-subs [-h] [--md MD] [--template TEMPLATE]
                         source output_dir

Normalize names of Moodle submissions and, if possible, uncompress them into
folders

positional arguments:
  source               Either a zip-file OR a directory with compressed files
                       from Moodle
  output_dir           Output directory, for normalized, extracted submissions

optional arguments:
  -h, --help           show this help message and exit
  --md MD              Name of markdown file to add to output, with a top-
                       level #<name_of_submission> line per submission
  --template TEMPLATE  If --md specified, file with markdown snippet to append
                       after each submission-line
~~~

#### Example template file

~~~ {.md}

## entrega

## funcionalidad

## claridad

## diseño

## nota

~~~

#### Example output folder

~~~
alfredo_javier_blázquez_lópez-entrega-práctica-1/
héctor_gómez_ejémplez-p1/
~~~

with an extra markdown file thrown in, but only if `--md` specified  (eg.: `--md grading.md`).

#### Example grading output file

Only written if `--md` specified (eg.: `--md grading.md`).

~~~ {.md}
# alfredo_javier_blázquez_lópez-entrega-práctica-1
<template content goes here>

# héctor_gómez_ejémplez-p1
<template content goes here>

~~~

## split-by-headers

Python 2.7 script that splits a Markdown file by top-level headers (`# header text`), creating one file per such header, with all contents until either the next header or the end of the file. This is useful when splitting a grading output file such as the ones created by `normalize_cv_subs`

#### Use

as reported with the `-h` option:

~~~
usage: split-by-headers [-h] input_file

Split a markdown file into subfiles, one per top-level header

positional arguments:
  input_file  A markdown file to split

optional arguments:
  -h, --help  show this help message and exit
~~~

## markdown-to-pdf

Python 2.7 script that generates a plain pdf document from a markdown input file. You can call `pandoc` directly instead of using it.

#### Use

as reported with the `-h` option:

~~~
usage: markdown-to-pdf [-h] input_file

Make a call to pandoc to create a nice pdf file from markdown

positional arguments:
  input_file  A markdown file to convert to pdf

optional arguments:
  -h, --help  show this help message and exit
~~~

## markdown-to-beamer

Python 2.7 script that generates a plain pdf document from a markdown input file. You can call `pandoc` directly instead of using it, and then call `pdflatex` on the result. However, this script simplifies these steps, and generally takes care of housekeeping for you (deleting unwanted auxiliary pdflatex files, supressing pdflatex output, ...).

#### Use

as reported with the `-h` option:

~~~
usage: markdown-to-beamer [-h] [--v] input_file

Make a call to pandoc & pdflatex to create a nice beamer pdf file from
markdown

positional arguments:
  input_file  A markdown file to convert to pdf

optional arguments:
  -h, --help  show this help message and exit
  --v         Show full pdflatex output; otherwise silence it
~~~



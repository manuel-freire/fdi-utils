# fdi-utils

Scripts to improve QoL at FdI UCM

## Goal

Keep a series of useful scripts for [FdI UCM](https://informatica.ucm.es/) teachers.

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



---
title: "Quality Control for Next Generation Sequencing"
teaching: 5
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How to perform quality control of NGS raw data?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Assess short reads FASTQ quality using `FASTQE`
- Compare reads before and after quality filtering with `fastp`
::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

This is a lesson created via The Carpentries Workbench. It is written in
[Pandoc-flavored Markdown](https://pandoc.org/MANUAL.txt) for static files and
[R Markdown][r-markdown] for dynamic files that can render code into output. 
Please refer to the [Introduction to The Carpentries 
Workbench](https://carpentries.github.io/sandpaper-docs/) for full documentation.

What you need to know is that there are three sections required for a valid
Carpentries lesson:

 1. `questions` are displayed at the beginning of the episode to prime the
    learner for the content.
 2. `objectives` are the learning objectives for an episode displayed with
    the questions.
 3. `keypoints` are displayed at the end of the episode to reinforce the
    objectives.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Is FASTQE installed? What version?

What is the output of this command?

```bash
fastqe --version
```

:::::::::::::::::::::::: solution 

## Output
 
```output
fastqe 1.0
```

:::::::::::::::::::::::::::::::::


## Challenge 2: Can you do the same for `fastp`? Hint: try `-v` as well as `--version` 

:::::::::::::::::::::::: solution 
try@fastqe.com$ fastp --version
fastp 0.20.1

You should get the same result from using `-v` and `--version` which are known as short and long options respectivlely. Long options will typically (but not always) have a corresonidng short option.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Command line structure

We have mentioned commands and options so far. The syntax is typiclaly as follows:

![A command line](https://raw.githubusercontent.com/carpentries/logo/master/Badge_Carpentries.svg){alt='Blue Carpentries hex person logo with no text.'}

::::::::::::::::::::::::::::::::::::: keypoints 

- Options can be long and short
- We have confirmed `fastp` and `fastqe` are installed

::::::::::::::::::::::::::::::::::::::::::::::::


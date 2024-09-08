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

During sequencing, the nucleotide bases in a DNA or RNA sample (library) are determined by the sequencer. For each fragment in the library, a sequence is generated, also called a read, which is simply a succession of nucleotides.

Modern sequencing technologies can generate a massive number of sequence reads in a single experiment. However, no sequencing technology is perfect, and each instrument will generate different types and amount of errors, such as incorrect nucleotides being called. These wrongly called bases are due to the technical limitations of each sequencing platform.

Therefore, it is necessary to understand, identify and exclude error-types that may impact the interpretation of downstream analysis. Sequence quality control is therefore an essential first step in your analysis. Catching errors early saves time later on.

To take a look at sequence quality along all sequences, we can use FASTQE. It is an open-source tool that provides a simple and fun way to quality control raw sequence data and print them as emoji. You can use it to give a quick impression of whether your data has any problems of which you should be aware before doing any further analysis.

We will also use the tool `fastp` which can be used to filter sequence data to remove low quality reads.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Is FASTQE installed? What version?

What is the output of this command?

```bash
$ fastqe --version
```

:::::::::::::::::::::::: solution 

## Output
 
```output
fastqe 1.0
```

:::::::::::::::::::::::::::::::::


## Challenge 2: Can you do the same for `fastp`? Hint: try `-v` as well as `--version` 

:::::::::::::::::::::::: solution 
$ fastp --version
fastp 0.20.1

You should get the same result from using `-v` and `--version` which are known as short and long options respectivlely. Long options will typically (but not always) have a corresonidng short option.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Command line structure

We have mentioned commands and options so far. The syntax is typically as follows:

![A command line](https://raw.githubusercontent.com/lonsbio/workbench-template/main/shell_command_syntax.swc.svg){alt='Structure of a shell command.'}


::::::::::::::::::::::::::::::::::::: challenge

## Challenge 3: Run this command:

```bash
$ ls -F /
```

:::::::::::::::::::::::: solution

## Output

```output
dev/  home/  proc/  shared/  tmp/
```

This is the filesystem of our browser-based portal.
:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: keypoints 

- Options can be long and short
- Structure of a shell command
- We have confirmed `fastp` and `fastqe` are installed

::::::::::::::::::::::::::::::::::::::::::::::::


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

```bash
$ fastp --version # or use -v
```
::::::::::::::::::::::::::::::::::::: callout

When using the command line code, lines that start with the # sign are typically text descriptors or instructions, not lines of code.

The # character is used to indicate comments. A line starting with # line doesn’t do anything, in this case it’s just telling you what the next line of code does which is navigate to your computer’s desktop
- We will stick with this convention throughout the activity
- Comments can occur anywhere in a line, anything after the # will be ignored

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution 
```output
$ fastp --version
fastp 0.20.1
```

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

## Case Study

Here’s brief background on the in class metagenomics project that we’re collecting sequencing data for. Garter snakes excrete sexually dimorphic pheromones to attract a mate. The hypothesis of our experiment is that male and female garter snakes host unique microbial communities in their mouths, cloacae and musk glands that contribute to sexually dimorphic bioengineering of these pheromone molecules. Figure 1 provides an overview of our 16S metagenomics analysis pipeline. For this lesson though, all you need are the FASTQ files.

![Figure 1. Overview of the in class metagenomics project. Using a saline swabbing technique, microbial samples were collected from garter snake tissues in class (A). Swabs were placed in sterile tubes to release collected microbes and DNA was extracted for downstream analysis (B). Barcoded primers were used to PCR amplify the microbial 16S ribosomal DNA repeat genes for each sample followed by Illumina sequencing of PCR amplicons (C-D). The DNA Subway Purple Line web-based software can be used to analyze FASTQ data files generated from Illumina sequencing to reveal the microbial population of our swabs (E). Garter snakes were provided by Dr. Rocky Parker in the JMU Department of Biology (A; yellow shirt).](https://raw.githubusercontent.com/lonsbio/workbench-template/main/fig1.png){alt='Images of experiment and data workflow'}

The data for this experiment is located in the `/shared/fastqe/` folder.


::::::::::::::::::::::::::::::::::::: challenge

## Challenge 4: What are the files available from this experiment?

Hint: Use the `ls` command we have already seen


:::::::::::::::::::::::: solution

```bash
ls -F /shared/fastqe/
```


```output
female_musk2.fastq.gz  female_oral2.fastq.gz
```

We have two files, one for each of the snake tissues sampled.



::::::::::::::::::::::::::::::::::::: keypoints 

- Options can be long and short
- Structure of a shell command
- We have confirmed `fastp` and `fastqe` are installed
- The case study and location of the data

::::::::::::::::::::::::::::::::::::::::::::::::


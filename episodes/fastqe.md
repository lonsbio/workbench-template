---
title: "Introduction to FASTQE"
teaching: 3
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can you analyse the quality of NGS data from the command line?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Introduce students to writing basic command line scripts
- Analyze and assess the quality of FASTQ formatted short read NGS data
- Trim/filter low quality reads in FASTQ files

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction


The 1st step of any Next Generation Sequencing (NGS) analysis pipeline is checking the quality of the raw sequencing reads in each FASTQ formatted file. If the sequence quality is poor, then your resulting downstream analysis will be inaccurate and misleading. FastQC is a popular software used to provide an overview of basic quality metrics for NGS data. In this lesson, you will use an even more universal form of
communication to analyze FASTQ files, THE EMOJI. 

The tool we will use for this visualisation is `faste`

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Familairity with FASTQ format is helpful.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Can you run `fastqe`?

What is the output of this command from your terminal?

```bash
fastqe --help
```

:::::::::::::::::::::::: solution 

## Output

If you have `fastqe` installed correctly, you should get usage information:
 
```output
[1] "This new lesson looks good"
```

:::::::::::::::::::::::::::::::::






## Challenge 2: how do you nest solutions within challenge blocks?

:::::::::::::::::::::::: solution 

You can add a line with at least three colons and a `solution` tag.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Case Study 

Here’s brief background on the in class metagenomics project that we’re collecting sequencing data for. Garter snakes excrete sexually dimorphic pheromones to attract a mate. The hypothesis of our experiment is that male and female garter snakes host unique microbial communities in their mouths, cloacae and musk glands that contribute to sexually dimorphic bioengineering of these pheromone molecules. Figure 1 provides an overview of our 16S metagenomics analysis pipeline. For this lesson though, all you need are the FASTQ files.

![Figure 1. Overview of the in class metagenomics project. Using a saline swabbing technique, microbial samples were collected from garter snake tissues in class (A). Swabs were placed in sterile tubes to release collected microbes and DNA was extracted for downstream analysis (B). Barcoded primers were used to PCR amplify the microbial 16S ribosomal DNA repeat genes for each sample followed by Illumina sequencing of PCR amplicons (C-D). The DNA Subway Purple Line web-based software can be used to analyze FASTQ data files generated from Illumina sequencing to reveal the microbial population of our swabs (E). Garter snakes were provided by Dr. Rocky Parker in the JMU Department of Biology (A; yellow shirt).](https://raw.githubusercontent.com/lonsbio/workbench-template/main/fig1.png){alt='Images of experiment and data workflow'}


::::::::::::::::::::::::::::::::::::: callout

When using the command line code, lines that start with the # sign are typically text descriptors or instructions, not lines of code.

The # character is used to indicate comments. A line starting with # line doesn’t do anything, in this case it’s just telling you what the next line of code does which is navigate to your computer’s desktop
- We will stick with this convention throughout the activity
- Comments can occur anywhere in a line, anything after the # will be ignored

::::::::::::::::::::::::::::::::::::::::::::::::


## PHRED scores

THE FASTQ format encodes quality as ASCII values, which are mapped to quality scores in the PHRED format. The PHRED score is calculated from P, the probability of a base-calling error:

$Q = -10\log{P}$

Cool, right?

::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/

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


The first step of any Next Generation Sequencing (NGS) analysis pipeline is checking the quality of the raw sequencing reads in each FASTQ formatted file. If the sequence quality is poor, then your resulting downstream analysis will be inaccurate and misleading. FastQC is a popular software used to provide an overview of basic quality metrics for NGS data. In this lesson, you will use an even more universal form of
communication to analyze FASTQ files, THE EMOJI. 

The tool we will use for this visualisation is `fastqe`

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
$ fastqe --help
Read one or more FASTQ files, compute quality stats for each file, print as emoji... for some reason.😄 

🚨 Rust and WebAssembly beta: only command line options with a  ✅  are functional  
 rustc 1.75.0-beta.7 emcc 3.1.50

Usage: fastqe [OPTIONS] [FASTQ_FILE]...

Arguments:
  [FASTQ_FILE]...  Input FASTQ files

Options:
      --noheader              Hide the header before sample output ❌
      --output <OUTPUT_FILE>  Write output to OUTPUT_FILE instead of stdout
      --long <READ_BUFFER>    Buffer memory for long reads up to READ_BUFFER bp long ❌ [default: 500]
      --log <LOG_FILE>        Record program progress in LOG_FILE ✅
  -h, --help                  Print help
  -V, --version               Print version

Emoji options:
      --bin                   Use binned scores (🚫 💀 💩 🚨 😄 😆 😎 😍 ) ❌ 
      --custom <CUSTOM_DICT>  Use a mapping of custom emoji to quality in CUSTOM_DICT (🐍🌴) ❌
      --noemoji               Use mapping without emoji (▁▂▃▄▅▆▇█) ❌

Statistics options:
      --minlen <N>  Minimum length sequence to include in stats ✅ [default: 0]
      --scale       Show relevant scale in output ❌
      --nomean      Hide mean quality per position ❌
      --min         Show minimum quality per position  ❌
      --max         Show maximum quality per position ❌

HTML report options:
      --html         Output all data as html ❌
      --window <W>   Window length to summarise reads in HTML report ❌ [default: 1]
      --html_escape  ❌ Escape html within output, e.g. for Galaxy parsing

For more information, vist https://fastqe.com
```

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

## Case Study 

Here’s brief background on the in class metagenomics project that we’re collecting sequencing data for. Garter snakes excrete sexually dimorphic pheromones to attract a mate. The hypothesis of our experiment is that male and female garter snakes host unique microbial communities in their mouths, cloacae and musk glands that contribute to sexually dimorphic bioengineering of these pheromone molecules. Figure 1 provides an overview of our 16S metagenomics analysis pipeline. For this lesson though, all you need are the FASTQ files.

![Figure 1. Overview of the in class metagenomics project. Using a saline swabbing technique, microbial samples were collected from garter snake tissues in class (A). Swabs were placed in sterile tubes to release collected microbes and DNA was extracted for downstream analysis (B). Barcoded primers were used to PCR amplify the microbial 16S ribosomal DNA repeat genes for each sample followed by Illumina sequencing of PCR amplicons (C-D). The DNA Subway Purple Line web-based software can be used to analyze FASTQ data files generated from Illumina sequencing to reveal the microbial population of our swabs (E). Garter snakes were provided by Dr. Rocky Parker in the JMU Department of Biology (A; yellow shirt).](https://raw.githubusercontent.com/lonsbio/workbench-template/main/fig1.png){alt='Images of experiment and data workflow'}

The data for this experiment is located in the `/shared/fastqe/` folder. 


::::::::::::::::::::::::::::::::::::: challenge

## Challenge 4: What are the files available from this experiment?

Hint: Use the `ls` command we have alreayd seen 


:::::::::::::::::::::::: solution

```bash
ls -F /shared/fastqe/
```


```output
female_musk2.fastq.gz  female_oral2.fastq.gz
```

We have two files, one for each of the snake tissues sampled. 

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

When using the command line code, lines that start with the # sign are typically text descriptors or instructions, not lines of code.

The # character is used to indicate comments. A line starting with # line doesn’t do anything, in this case it’s just telling you what the next line of code does which is navigate to your computer’s desktop
- We will stick with this convention throughout the activity
- Comments can occur anywhere in a line, anything after the # will be ignored

::::::::::::::::::::::::::::::::::::::::::::::::

## The FASTQ format

Although it looks complicated (and maybe it is), the FASTQ format is easy to understand with a little decoding.

Each read, representing a fragment of the library, is encoded by 4 lines:

Line	Description
1	Always begins with @ followed by the information about the read
2	The actual nucleic sequence
3	Always begins with a + and contains sometimes the same info in line 1
4	Has a string of characters which represent the quality scores associated with each base of the nucleic sequence; must have the same number of characters as line 2
So for example, the first sequence in our file is:
```text
@M00970:337:000000000-BR5KF:1:1102:17745:1557 1:N:0:CGCAGAAC+ACAGAGTT
GTGCCAGCCGCCGCGGTAGTCCGACGTGGCTGTCTCTTATACACATCTCCGAGCCCACGAGACCGAAGAACATCTCGTATGCCGTCTTCTGCTTGAAAAAAAAAAAAAAAAAAAACAAAAAAAAAAAAAGAAGCAAATGACGATTCAAGAAAGAAAAAAACACAGAATACTAACAATAAGTCATAAACATCATCAACATAAAAAAGGAAATACACTTACAACACATATCAATATCTAAAATAAATGATCAGCACACAACATGACGATTACCACACATGTGTACTACAAGTCAACTA
+
GGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGFGGGFGGGGGGAFFGGFGGGGGGGGFGGGGGGGGGGGGGGFGGG+38+35*311*6,,31=******441+++0+0++0+*1*2++2++0*+*2*02*/***1*+++0+0++38++00++++++++++0+0+2++*+*+*+*+*****+0**+0**+***+)*.***1**//*)***)/)*)))*)))*),)0(((-((((-.(4(,,))).,(())))))).)))))))-))-(
```

It means that the fragment named `@M00970` corresponds to the DNA sequence `GTGCCAGCCGCCGCGGTAGTCCGACGTGGCTGTCTCTTATACACATCTCCGAGCCCACGAGACCGAAGAACATCTCGTATGCCGTCTTCTGCTTGAAAAAAAAAAAAAAAAAAAACAAAAAAAAAAAAAGAAGCAAATGACGATTCAAGAAAGAAAAAAACACAGAATACTAACAATAAGTCATAAACATCATCAACATAAAAAAGGAAATACACTTACAACACATATCAATATCTAAAATAAATGATCAGCACACAACATGACGATTACCACACATGTGTACTACAAGTCAACTA` and this sequence has been sequenced with a quality `GGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGFGGGFGGGGGGAFFGGFGGGGGGGGFGGGGGGGGGGGGGGFGGG+38+35*311*6,,31=******441+++0+0++0+*1*2++2++0*+*2*02*/***1*+++0+0++38++00++++++++++0+0+2++*+*+*+*+*****+0**+0**+***+)*.***1**//*)***)/)*)))*)))*),)0(((-((((-.(4(,,))).,(())))))).)))))))-))-(`.

## PHRED scores

THE FASTQ format encodes quality as ASCII values, which are mapped to quality scores in the PHRED format. The PHRED score is calculated from P, the probability of a base-calling error:

$Q = -10\log{P}$

```text
Phred Quality Score	Probability of incorrect base call	Base call accuracy
10	1 in 10	90%
20	1 in 100	99%
30	1 in 1000	99.9%
40	1 in 10,000	99.99%
50	1 in 100,000	99.999%
60	1 in 1,000,000	99.9999%
```


```
Phred Quality Score	ASCII code	Emoji
0	!	🚫
1	”	❌
2	#	👺
3	$	💔
4	%	🙅
5	&	👾
6	’	👿
7	(	💀
8	)	👻
9	*	🙈
10	+	🙉
11	,	🙊
12	-	🐵
13	.	😿
14	/	😾
15	0	🙀
16	1	💣
17	2	🔥
18	3	😡
19	4	💩
```
::::::::::::::::::::::::::::::::::::: keypoints 

- The options available for `fastqe`
- The case study and location of the data
- The formula for Q scores

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/

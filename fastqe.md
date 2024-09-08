---
title: "The FASTQ format and FASTQE"
teaching: 3
exercises: 2
---


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

FASTQE has a lot of options! Note that this version does not implement all of them, however we will be using it in the default mode.
:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


## The FASTQ format

Although it looks complicated (and maybe it is), the FASTQ format is easy to understand with a little decoding.

Each read, representing a fragment of the library, is encoded by 4 lines:

1.	Always begins with @ followed by the information about the read
2.	The actual nucleic sequence
3.	Always begins with a + and contains sometimes the same info in line 1
4.	Has a string of characters which represent the quality scores associated with each base of the nucleic sequence; must have the same number of characters as line 2

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

These values are linked by the Phred Quality Score, Probability of incorrect base call, and Base call accuracy. We can summarise these:

```text
PHRED	Probability	Accuracy
10	1 in 10		90%
20	1 in 100	99%
30	1 in 1000	99.9%
40	1 in 10,000	99.99%
50	1 in 100,000	99.999%
60	1 in 1,000,000	99.9999%
```

FASTQE aids in the interpretation of these by mapping the Phred Quality Score and 	ASCII code to an Emoji:

```
PHRED	ASCII	Emoji
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

Much easier to understand! Here we show the emoji for Q scores below 20, which represents a 1 in 100 probability of a base error. Given there can be many thousands of reads and bases in even a small sequeuncing sample, the impact of these errors on downstream analysis and conclusions is significant. Steps we will see later in the lesson can be used to remove low quality reads.   


## Case Study 


Let's look at the output of FASTQE on our case study data:

```bash
$ fastqe /shared/fastqe/female_musk2.fastq.gz
```

which should give the following:

```output
$ fastqe /shared/fastqe/female_musk2.fastq.gz
Filename        Statistic       Quality
/shared/fastqe/female_musk2.fastq.gz    mean    😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 
😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁
😁 😁 😁 😁 😁 😉 😉 😁 😁 😁 😁 😁 😉 😁 😁 😁 😁 😁 😁 😉 😁 😉 😁 😁 😉 😁 😁 😁
😁 😁 😛 😜 😉 😁 😉 😉 😁 😁 😁 😁 😁 😉 😁 😁 😁 😁 😁 😁 😁 😁 😉 😉 😉 😉 😉 😁
😁 😉 😁 😁 😉 😉 😉 😛 😋 😄 😄 😆 😄 😆 😆 😆 😆 😆 😘 😆 😆 😆 😆 😘 😘 😘 😘 😘
😘 😘 😘 😘 😘 😘 😘 😘 😘 😘 😘 😘 😃 😃 😘 😘 😘 😘 😘 😃 😃 😃 😃 😃 😃 😃 😘 😘
😃 😘 😃 😃 😘 😃 😃 😃 😃 😃 😃 😃 😃 😃 😃 😚 😃 😃 😃 😃 😃 😃 😃 😃 😃 😚 😃 😃
😃 😃 😃 😃 😃 😚 😚 😚 😚 😚 😃 😚 😚 😚 😚 😃 😚 😚 😃 😃 😚 😚 😚 😃 😚 😚 😚 😚 
😚 😚 😚 😚 😚 😚 😚 😚 😚 😗 😚 😚 😚 😗 😗 😗 😗 😗 😚 😚 😗 😗 😗 😗 😗 😗 😙 😗
😙 😙 😙 😙 😗 😙 😏 😊 😙 😙 😙 😊 😊 😏 😊 😊 😊 😏 😏 😅 😏 😅 😏 😊 😊 😊 😅 😅
😅 😅 😏 😅 😏 😅 😊 😏 😅 😏 😏 😅 😅 😅 😅 😏 😅 😅 😏 😅 😅 😀 😀 😀 😅 😀 😅 😀
😅 😀 🚨 😡
```


::::::::::::::::::::::::::::::::::::: challenge

## Challenge 5: What about the other sample?

Can you use `fastqe` on the other tissue sample? Which sample has better overall quality? Why?

:::::::::::::::::::::::: solution

You should run `fastqe` with a different  filename:
```bash
$ fastqe /shared/fastqe/female_oral2.fastq.gz 
```
and the output will be:

```output
Filename        Statistic       Quality
/shared/fastqe/female_oral2.fastq.gz    mean    😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁
😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁
😁 😁 😁 😁 😁 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😁 😁
😁 😁 😉 😜 😛 😜 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😉 😁 😁 😉 😁 😁 😉 😉 😉 😉 😉
😉 😉 😉 😁 😉 😉 😉 😉 😛 😄 😘 😃 😃 😃 😚 😚 😚 😚 😚 😚 😗 😗 😚 😗 😗 😗 😗 😗
😙 😗 😗 😗 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙
😙 😙 😙 😙 😙 😙 😊 😙 😙 😙 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊
😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😏 😊 😊 😊 😏 😊 😏 😏 😏 😏 😊 😊 😊 😊 😊 😏 😏
😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😅 😅 😅 😅 😅 😅 😅
😅 😅 😅 😅 😅 😅 😀 😀 😀 😅 😀 😀 🚨 😀 😀 😀 😀 😀 😀 🚨 🚨 💩 🚨 🚨 😀 🚨 💩 💩
💩 🚨 🚨 🚨 🚨 💩 🚨 🚨 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩
💩 😡 💩 😾
```

The mouth (oral) sample has inferirior quality, on average, towards the end of the reads. 

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

To look at the quality of these files in more detail, we will next use the emoji-less command line tool `fastp`.

::::::::::::::::::::::::::::::::::::: keypoints 

- The options available for `fastqe`
- The formula for Q scores
- FASTQE maps scores to emoji
- FASTQE can be used to quickly asses the quality of sequencing data

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/

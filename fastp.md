---
title: "Filtering with fastp"
teaching: 10
exercises: 2
---


## Filtering

The quality drop off observed could cause bias in downstream analyses with these potentially incorrectly called nucleotides. Sequences must be treated to reduce bias in downstream analysis. Trimming can help to increase the number of reads the aligner or assembler are able to succesfully use, reducing the number of reads that are unmapped or unassembled. In general, quality treatments include:

- Trimming/cutting/masking sequences:
	- from low quality score regions
	- beginning/end of sequence
	- removing adapters

- Filtering of sequences
	- with low mean quality score
	- too short
	- with too many ambiguous (N) bases

`fastp` is a command line tool that performs many of these steps. Let's run it with our oral2 sample, and direct output of the new, filtered, data to a new file with `-o` opoption: 


```bash
fastp -i /shared/fastqe/female_oral2.fastq.gz -o female_oral_filtered.fastq.gz

```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 6: What is filtered? 

How many reads are in this FASTQ file before and after filtering? 

After filtering, what is the % of reads with a Q score of 20 or higher?

:::::::::::::::::::::::: solution

429 reads are filtered, and 93.7328% of the remaining have a Q score of at leats 20. 

```output

Read1 after filtering:
total reads: 383
total bases: 113368
Q20 bases: 106263(93.7328%)
Q30 bases: 98853(87.1966%)

Filtering result:
reads passed filter: 383
reads failed due to low quality: 428
reads failed due to too many N: 1
reads failed due to too short: 0
reads with adapter trimmed: 0
bases trimmed due to adapters: 0
```



::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::


In addition to command line information, `fastp` produces an interactive HTML report. We can use our browser platform  to view it:

```bash
open fastp.html
```

Those reports help, but what about our old friend FASTQE?
Recall the output on the original file:

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

Lets now run FASTQE on our filtered data from the oral smaple:

```bash
$ fastqe female_oral_filtered.fastq.gz
```

We can see easily the effect of the filtering and trimming done by `fastp`:

```bash
Filename        Statistic       Quality
female_oral_filtered.fastq.gz   mean    😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😁 😁 😁 😁
😁 😁 😉 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😁 😉
😉 😉 😉 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😉 😉 😁 😁 😉 😉 😉 😉 😁 😉 😁 😁
😁 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉
😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😜 😜 😜 😜 😉 😉 😜 😜 😜
😜 😜 😉 😜 😜 😉 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜
😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😛 😜 😜 😜 😜 😜 😜 😜 😜 😛 😛 😜 😜
😛 😛 😛 😛 😛 😛 😛 😛 😛 😛 😝 😛 😛 😛 😛 😛 😛 😝 😝 😝 😝 😝 ☺️ 😝 😝 😝 😝 😝
😝 ☺️ ☺️ 😋 😋 ☺️ ☺️ ☺️ 😋 😄 😋 😋 😋 😋 😄 😋 😄 😄 😘 😄 😄 😋 😋 😘 😘 😆 😄 😆 😆 
😆 😆 😄 😆 😘 😘 😆 😘 😘 😘 😘 😆 😘 😘 😘 😘 😘 😘 😘 😘 😘 😃 😘 😘 😃 😃 😃 😡
```

Things are looking better!

## Conclusion

To sum up, you just looked at Illumina FASTQ data quality using Emoji output. You then filtered the low quality sequences in your FASTQ file and output before and after QC plots. Following filtering, you created a new Emoji output of your filter FASTQ file.

You did all of that on the command line, congrats!


::::::::::::::::::::::::::::::::::::: keypoints

- `fastp` can be used to filter low quality reads from sequencing data
- Filtering can improve the quality of data and downstream analysis
- Emoji can be useful!

::::::::::::::::::::::::::::::::::::::::::::::::





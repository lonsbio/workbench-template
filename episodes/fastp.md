---
title: "Filtering with fastp"
teaching: 10
exercises: 2
---


## Filtering


```bash
fastp -i /shared/fastqe/female_oral2.fastq.gz -o female_oral_filtered.fastq.gz

```

`fastp` produces an interactive HTML report. We can use our browser platform 
to view it:

```bash
open fastp.html
```

Those reports help, but what about our old friend FASTQE?
Recall the output on the original file:

```output
Filename        Statistic       Quality
/shared/fastqe/female_oral2.fastq.gz    mean    😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 >😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 >😁 😁 😁 😁 😁 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😁 😁 >😁 😁 😉 😜 😛 😜 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😉 😁 😁 😉 😁 😁 😉 😉 😉 😉 😉 >😉 😉 😉 😁 😉 😉 😉 😉 😛 😄 😘 😃 😃 😃 😚 😚 😚 😚 😚 😚 😗 😗 😚 😗 😗 😗 😗 😗 >😙 😗 😗 😗 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 😙 >😙 😙 😙 😙 😙 😙 😊 😙 😙 😙 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 >😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😊 😏 😊 😊 😊 😏 😊 😏 😏 😏 😏 😊 😊 😊 😊 😊 😏 😏 >😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😏 😅 😅 😅 😅 😅 😅 😅 >😅 😅 😅 😅 😅 😅 😀 😀 😀 😅 😀 😀 🚨 😀 😀 😀 😀 😀 😀 🚨 🚨 💩 🚨 🚨 😀 🚨 💩 💩 >💩 🚨 🚨 🚨 🚨 💩 🚨 🚨 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 💩 >💩 😡 💩 😾
```

Lets now run FASTQE on our filtered data from the oral smaple:

```bash
$ fastqe female_oral_filtered.fastq.gz
```

We can see easily the effect of the filtering and trimming done by `fastp`:

```bash
Filename        Statistic       Quality
female_oral_filtered.fastq.gz   mean    😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😁 😁 😁 😁 😁 😁 😉 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😁 😉 😉 😉 😉 😉 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😁 😉 😉 😉 😁 😁 😉 😉 😉 😉 😁 😉 😁 😁 😁 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😉 😜 😜 😜 😜 😉 😉 😜 😜 😜 😜 😜 😉 😜 😜 😉 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😜 😛 😜 😜 😜 😜 😜 😜 😜 😜 😛 😛 😜 😜 😛 😛 😛 😛 😛 😛 😛 😛 😛 😛 😝 😛 😛 😛 😛 😛 😛 😝 😝 😝 😝 😝 ☺️ 😝 😝 😝 😝 😝 😝 ☺️ ☺️ 😋 😋 ☺️ ☺️ ☺️ 😋 😄 😋 😋 😋 😋 😄 😋 😄 😄 😘 😄 😄 😋 😋 😘 😘 😆 😄 😆 😆 😆 😆 😄 😆 😘 😘 😆 😘 😘 😘 😘 😆 😘 😘 😘 😘 😘 😘 😘 😘 😘 😃 😘 😘 😃 😃 😃 😡
```

Things are looking better!

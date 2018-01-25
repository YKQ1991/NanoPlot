# NanoPlot
Plotting tool for long read sequencing data and alignments.  

[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/wouter_decoster.svg?style=social&label=Follow%20%40wouter_decoster)](https://twitter.com/wouter_decoster)
[![conda badge](https://anaconda.org/bioconda/nanoplot/badges/installer/conda.svg)](https://anaconda.org/bioconda/nanoplot)
[![Build Status](https://travis-ci.org/wdecoster/NanoPlot.svg?branch=master)](https://travis-ci.org/wdecoster/NanoPlot)
[![Code Health](https://landscape.io/github/wdecoster/NanoPlot/master/landscape.svg?style=flat)](https://landscape.io/github/wdecoster/NanoPlot/master)



![Example plot](https://github.com/wdecoster/NanoPlot/blob/master/examples/scaled_Log_Downsampled_LengthvsQualityScatterPlot_kde.png)

The example plot above shows a bivariate plot comparing log transformed read length with average basecall Phred quality score. More examples can be found in the [gallery on my blog 'Gigabase Or Gigabyte'.](https://gigabaseorgigabyte.wordpress.com/2017/06/01/example-gallery-of-nanoplot/)

In addition to various plots also a NanoStats file is created summarizing key features of the dataset.

This script performs data extraction from Oxford Nanopore sequencing data in the following formats:  
- fastq files  
(can be bgzip, bzip2 or gzip compressed)  
- fastq files generated by albacore or MinKNOW containing additional information  
(can be bgzip, bzip2 or gzip compressed)  
- sorted bam files  
- sequencing_summary.txt output table generated by albacore  

### INSTALLATION

`pip install NanoPlot`  

Upgrade to a newer version using:  
`pip install NanoPlot --upgrade`

or

[![conda badge](https://anaconda.org/bioconda/nanoplot/badges/installer/conda.svg)](https://anaconda.org/bioconda/nanoplot)   
`conda install -c bioconda nanoplot`

The script is written for python3.

### OUTPUT
NanoPlot creates:
- a statistical summary
- a number of plots
- a html summary file



### USAGE
```
NanoPlot [-h] [-v] [-t THREADS] [--verbose] [--store] [--raw]
                [-o OUTDIR] [-p PREFIX] [--maxlength N] [--minlength N]
                [--drop_outliers] [--downsample N] [--loglength]
                [--percentqual] [--alength] [--minqual N]
                [--readtype {1D,2D,1D2}] [--barcoded] [-c COLOR]
                [-f {eps,jpeg,jpg,pdf,pgf,png,ps,raw,rgba,svg,svgz,tif,tiff}]
                [--plots [{kde,hex,dot,pauvre} [{kde,hex,dot,pauvre} ...]]]
                [--listcolors] [--no-N50] [--N50] [--title TITLE]
                (--fastq file [file ...] | --fastq_rich file [file ...] | --fastq_minimal file [file ...] | --summary file [file ...] | --bam file [file ...] | --cram file [file ...] | --pickle pickle)


General options:
  -h, --help            show the help and exit
  -v, --version         Print version and exit.
  -t, --threads THREADS Set the allowed number of threads to be used by the script
  --verbose             Write log messages also to terminal.
  --store               Store the extracted data in a pickle file for future plotting.
  --raw                 Store the extracted data in tab separated file.
  -o, --outdir OUTDIR   Specify directory in which output has to be created.
  -p, --prefix PREFIX   Specify an optional prefix to be used for the output files.

Options for filtering or transforming input prior to plotting:
  --maxlength N         Drop reads longer than length specified.
  --minlength N         Drop reads shorter than length specified.
  --drop_outliers       Drop outlier reads with extreme long length.
  --downsample N        Reduce dataset to N reads by random sampling.
  --loglength           Logarithmic scaling of lengths in plots.
  --percentqual         Use qualities as theoretical percent identities.
  --alength             Use aligned read lengths rather than sequenced length (bam mode)
  --minqual N           Drop reads with an average quality lower than specified.
  --readtype            Which read type to extract information about from a summary file.
                        One of 1D (default), 2D, 1D2
  --barcoded            Use if you want to split the summary file by barcode

Options for customizing the plots created:
  -c, --color COLOR     Specify a color for the plots, must be a valid matplotlib color
  -f, --format          Specify the output format of the plots.
                        One of png [default], eps,jpeg,jpg,pdf,pgf,ps,raw,rgba,svg,svgz,tif,tiff
  --plots               Specify which bivariate plots have to be made.
                        One or more of kde (default), hex (default), dot (default), pauvre
  --listcolors          List the colors which are available for plotting and exit.
  --no-N50              Hide the N50 mark in the read length histogram
  --N50                 Show the N50 mark in the read length histogram
  --title TITLE         Add a title to all plots, requires quoting if using spaces

Input data sources, one of these is required.:
  --fastq file [file ...]
                        Data is in one or more default fastq file(s).
  --fastq_rich file [file ...]
                        Data is in one or more fastq file(s) generated by albacore or MinKNOW with
                        additional information concerning channel and time.
  --fastq_minimal file [file ...]
                        Data is in one or more fastq file(s) generated by albacore or MinKNOW with
                        additional information concerning channel and time. Minimal data is extracted
                        swiftly without elaborate checks.
  --summary file [file ...]
                        Data is in one or more summary file(s) generated by albacore.
  --bam file [file ...]
                        Data is in one or more sorted bam file(s).
  --cram file [file ...]
                        Data is in one or more sorted cram file(s).
  --pickle pickle       Data is a pickle file stored earlier.
```

### EXAMPLE USAGE
```bash
Nanoplot --summary sequencing_summary.txt --loglength -o summary-plots-log-transformed  
NanoPlot -t 2 --fastq reads1.fastq.gz reads2.fastq.gz --maxlength 40000 --plots hex dot
NanoPlot -t 12 --color yellow --bam alignment1.bam alignment2.bam alignment3.bam --downsample 10000 -o bamplots_downsampled
```
This script now also provides read length vs mean quality plots in the '[pauvre](https://github.com/conchoecia/pauvre)'-style from [@conchoecia](https://github.com/conchoecia).

## [ACKNOWLEDGMENTS](https://github.com/wdecoster/NanoPlot/blob/master/ACKNOWLEDGMENTS.MD)

I welcome all suggestions, bug reports, feature requests and contributions. Please leave an [issue](https://github.com/wdecoster/NanoPlot/issues) or open a pull request. I will usually respond within a day, or rarely within a few days.

## PLOTS GENERATED
Plot|Fastq|Fastq_rich|Fastq_minimal|Bam|Summary|Options|Style
----|----|----|----|----|----|----|----
Histogram of read length|x|x|x|x|x|N50|
Histogram of (log transformed) read length|x|x|x|x|x|N50|
Bivariate plot of length against base call quality|x|x||x|x|log transformation|dot, hex, kde, pauvre
Heatmap of reads per channel||x|||x||
Cumulative yield plot||x|x||x||
Violin plot of read length over time||x|x||x||
Violin plot of base call quality over time||x|||x||
Bivariate plot of aligned read length against sequenced read length||||x|||dot, hex, kde
Bivariate plot of percent reference identity against read length||||x||log transformation|dot, hex, kde
Bivariate plot of percent reference identity against base call quality||||x|||dot, hex, kde
Bivariate plot of mapping quality against read length||||x||log transformation|dot, hex, kde
Bivariate plot of mapping quality against basecall quality||||x|||dot, hex, kde


## COMPANION SCRIPTS
- [NanoComp](https://github.com/wdecoster/nanocomp): comparing multiple runs  
- [NanoStat](https://github.com/wdecoster/nanostat): statistic summary report of reads or alignments  
- [NanoFilt](https://github.com/wdecoster/nanofilt): filtering and trimming of reads  
- [NanoLyse](https://github.com/wdecoster/nanolyse): removing contaminant reads (e.g. lambda control DNA) from fastq

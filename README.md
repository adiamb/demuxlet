# demuxlet
Genetic multiplexing of barcoded single cell RNA-seq

### Introduction

**_demuxlet_** is a software tool to deconvolute sample identity and identify multiplets when multiple samples are pooled by barcoded single cell sequencing.
**_demuxlet_** takes (1) a SAM/BAM/CRAM file produced by the standard 10x sequencing platform, or any other barcoded single cell RNA-seq (with proper --tag-UMI and --tag-group) options (2) a VCF/BCF file containing the genotype (GT), posterior probability (GP), or genotype likelihood (GL) to assign each barcode to a specific sample (or a pair of samples) in the VCF file. 

### Installing demuxlet

Before installing demuxlet, you need to install [htslib](https://github.com/samtools/htslib) in the same directory you want to install demuxlet (i.e. demuxlet and htslib should be siblings).

After installing htslib, you can clone the current snapshot of this repository to install as well

<pre>
$ git clone https://github.com/statgen/demuxlet.git
$ cd demuxlet
$ autoreconf -vfi
$ ./configure  (with additional options such as --prefix)
$ make
$ make install (may require root privilege)
</pre>

### Using demuxlet

demuxlet uses a self-documentation utility. You can run each utility with -man or -help option to see the command line usages.

<pre>
$ ./demuxlet          (for short usage)
$ ./demuxlet -help    (for detailed usage)
</pre>

The detailed usage is also pasted below.

<pre>
Options for input SAM/BAM/CRAM
  --sam           [STR: ]             : Input SAM/BAM/CRAM file. Must be sorted by coordinates and indexed
  --tag-group     [STR: CB]           : Tag representing readgroup or cell barcodes, in the case to partition the BAM file into multiple groups. For 10x genomics, use CB
  --tag-UMI       [STR: UB]           : Tag representing UMIs. For 10x genomiucs, use UB

Options for input VCF/BCF
  --vcf           [STR: ]             : Input VCF/BCF file, containing the individual genotypes (GT), posterior probability (GP), or genotype likelihood (PL)
  --field         [STR: GP]           : FORMAT field to extract the genotype, likelihood, or posterior from
  --geno-error    [FLT: 0.01]         : Genotype error rate (must be used with --field GT)
  --min-mac       [INT: 1]            : Minimum minor allele frequency
  --min-callrate  [FLT: 0.50]         : Minimum call rate
  --sm            [V_STR: ]           : List of sample IDs to compare to (default: use all)
  --sm-list       [STR: ]             : File containing the list of sample IDs to compare

Output Options
  --out           [STR: ]             : Output file prefix
  --alpha         [V_FLT: ]           : Grid of alpha to search for (default is 0.1, 0.2, 0.3, 0.4, 0.5)
  --write-pair    [FLG: OFF]          : Writing the (HUGE) pair file
  --doublet-prior [FLT: 0.50]         : Prior of doublet
  --sam-verbose   [INT: 1000000]      : Verbose message frequency for SAM/BAM/CRAM
  --vcf-verbose   [INT: 10000]        : Verbose message frequency for VCF/BCF

Read filtering Options
  --cap-BQ        [INT: 40]           : Maximum base quality (higher BQ will be capped)
  --min-BQ        [INT: 13]           : Minimum base quality to consider (lower BQ will be skipped)
  --min-MQ        [INT: 20]           : Minimum mapping quality to consider (lower MQ will be ignored)
  --min-TD        [INT: 0]            : Minimum distance to the tail (lower will be ignored)
  --excl-flag     [INT: 3844]         : SAM/BAM FLAGs to be excluded

Cell/droplet filtering options
  --group-list    [STR: ]             : List of tag readgroup/cell barcode to consider in this run. All other barcodes will be ignored. This is useful for parallelized run
  --min-total     [INT: 0]            : Minimum number of total reads for a droplet/cell to be considered
  --min-uniq      [INT: 0]            : Minimum number of unique reads (determined by UMI/SNP pair) for a droplet/cell to be considered
  --min-snp       [INT: 0]            : Minimum number of SNPs with coverage for a droplet/cell to be considered
</pre>
# Genome assembly

## Before we start

Lets see how our directory structure looks so far:

```
$ cd ~/analysis
$ ls -1F
```

## Creating a genome assembly

We want to create a genome assembly for our ancestor. We are going to
use the quality trimmed forward and backward DNA sequences and use a
program called [SPAdes](https://github.com/ablab/spades) to build a genome
assembly.

**Questions**

1. Discuss briefly why we are using the ancestral sequences to create a reference genome as opposed to the evolved line.

### Installing the software

We are going to use a program
called [SPAdes](https://github.com/ablab/spades) fo assembling our genome.
In a recent evaluation of assembly
software, [SPAdes](https://github.com/ablab/spades) was found to be a good
choice for fungal
genomes [\[ABBAS2014\]](https://genomics.sschmeier.com/ngs-assembly/index.html#abbas2014).
It is also simple to install and use.

```
$ mamba create -y -q -n assembly spades quast
$ mamba activate assembly
```

### SPAdes usage

```console
# change to your analysis root folder
$ cd ~/analysis

# Make a directory for our assemblies.
$ mkdir assembly

# to get a help for spades and an overview of the parameter type:
$ spades.py -h
```

Generally, paired-end data is submitted in the following way
to [SPAdes](https://cab.spbu.ru/software/spades/):

```console
$ spades.py -t 1 -m 32 -o assembly/spades-default/ -1 read1.fastq.gz -2 read2.fastq.gz
```

| Arguments        | Meaning                                           |
| :--------------- | :------------------------------------------------ |
| `-t 1`           | Use 1 thread, because we're on a shared machine   |
| `-m 32`          | Use up to 32 GB of RAM (this machine has 750GB)   |
| `-o assembly/spades-default/`   | Output the results to a folder called `assembly/` |
| `-1 read1.fq.gz` | Our forward read                                  |
| `-2 read2.fq.gz` | Our reverse read                                  |

**Exercises**

1. Run [SPAdes](https://github.com/ablab/spades) with default parameters on the **ancestor**'s **trimmed** **reads**

2. Read in the [SPAdes](https://github.com/ablab/spades) manual about about assembling with 2x150bp reads

3. Run [SPAdes](https://github.com/ablab/spades) a second time but use the options suggested at the [SPAdes](https://github.com/ablab/spades) manual for assembling 2x150bp paired-end reads. Use a different output directory `assembly/spades-150` for this run.

**Hint**

Should you not get it right, try the commands in [Code: SPAdes assembly (trimmed data)](https://genomics.sschmeier.com/code.html#code-assembly1).

## Assembly quality assessment

### Assembly statistics

[Quast](https://github.com/ablab/quast) (QUality ASsessment Tool) [\[GUREVICH2013\]](https://genomics.sschmeier.com/ngs-assembly/index.html#gurevich2013), evaluates genome assemblies by computing various metrics, including:

- N50: length for which the collection of all contigs of that length
  or longer covers at least 50% of assembly length
- NG50: where length of the reference genome is being covered
- NA50 and NGA50: where aligned blocks instead of contigs are taken
- miss-assemblies: miss-assembled and unaligned contigs or contigs
  bases
- genes and operons covered

It is easy with [Quast](https://github.com/ablab/quast) to compare these
measures among several assemblies. The program can be used via `conda` as we did at the start of this lesson.


Run [Quast](https://github.com/ablab/quast) with both assembly
scaffolds.fasta files to compare the results.

```
$ quast -o assembly/quast assembly/spades-default/scaffolds.fasta assembly/spades-150/scaffolds.fasta
```

**Exercise**

1. Compare the results of [Quast](https://github.com/ablab/quast) with regards to the two different assemblies.
2. Which one do you prefer and why?

## Compare the untrimmed data

**Exercise**

1. To see if our trimming procedure has an influence on our assembly,
   run the same command you used on the trimmed data on the original
   untrimmed data.

2. Run [Quast](https://github.com/ablab/quast) on the assembly and
   compare the statistics to the one derived for the trimmed data set.
   Write down your observations.

**Hint**

Should you not get it right, try the commands in [Code: SPAdes assembly (original data)](https://genomics.sschmeier.com/code.html#code-assembly2).

## Further reading

### Background on Genome Assemblies

- How to apply de Bruijn graphs to genome
  assembly. [COMPEAU2011]

- Sequence assembly demystified. [NAGARAJAN2013]

### Evaluation of Genome Assembly Software

- GAGE: A critical evaluation of genome assemblies and assembly
  algorithms. [SALZBERG2012]

- Assessment of de novo assemblers for draft genomes: a case study
  with fungal
  genomes. [ABBAS2014]

## Web links

- Lectures for this topic: [Genome Assembly: An
  Introduction](https://dx.doi.org/10.6084/m9.figshare.2972323.v1)

- [SPAdes](https://github.com/ablab/spades)

- [Quast](https://github.com/ablab/quast)

- [Bandage](https://rrwick.github.io/Bandage/) (Bioinformatics
  Application for Navigating De novo Assembly Graphs Easily) is a
  program that visualizes a genome assembly as a
  graph [\[WICK2015\]](https://genomics.sschmeier.com/ngs-assembly/index.html#wick2015).

## References

ABBAS2014([1](https://genomics.sschmeier.com/ngs-assembly/index.html#id2),[2](https://genomics.sschmeier.com/ngs-assembly/index.html#id10))

:   Abbas MM, Malluhi QM, Balakrishnan P. Assessment of de novo
    assemblers for draft genomes: a case study with fungal genomes. [BMC
    Genomics. 2014;15 Suppl
    9:S10.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4290589/) doi:
    10.1186/1471-2164-15-S9-S10. Epub 2014 Dec 8.

[COMPEAU2011](https://genomics.sschmeier.com/ngs-assembly/index.html#id7)

:   Compeau PE, Pevzner PA, Tesler G. How to apply de Bruijn graphs to
    genome assembly. [Nat Biotechnol. 2011 Nov
    8;29(11):987-91](https://dx.doi.org/10.1038/nbt.2023)

[GUREVICH2013](https://genomics.sschmeier.com/ngs-assembly/index.html#id4)

:   Gurevich A, Saveliev V, Vyahhi N and Tesler G. QUAST: quality
    assessment tool for genome assemblies. [Bioinformatics 2013, 29(8),
    1072-1075](https://bioinformatics.oxfordjournals.org/content/29/8/1072)

[NAGARAJAN2013](https://genomics.sschmeier.com/ngs-assembly/index.html#id8)

:   Nagarajan N, Pop M. Sequence assembly demystified. [Nat Rev Genet.
    2013 Mar;14(3):157-67](https://dx.doi.org/10.1038/nrg3367)

[SALZBERG2012](https://genomics.sschmeier.com/ngs-assembly/index.html#id9)

:   Salzberg SL, Phillippy AM, Zimin A, Puiu D, Magoc T, Koren S,
    Treangen TJ, Schatz MC, Delcher AL, Roberts M, Marçais G, Pop M,
    Yorke JA. GAGE: A critical evaluation of genome assemblies and
    assembly algorithms. [Genome Res. 2012
    Mar;22(3):557-67](http://genome.cshlp.org/content/22/3/557.full?sid=59ea80f7-b408-4a38-9888-3737bc670876)

[WICK2015](https://genomics.sschmeier.com/ngs-assembly/index.html#id11)

:   Wick RR, Schultz MB, Zobel J and Holt KE. Bandage: interactive
    visualization of de novo genome assemblies. [Bioinformatics 2015,
    10.1093/bioinformatics/btv383](https://bioinformatics.oxfordjournals.org/content/early/2015/07/11/bioinformatics.btv383.long)

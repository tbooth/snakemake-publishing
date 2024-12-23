---
title: Making a toy dataset
teaching: 30
exercises: 30
---

## Publishing and sharing your workflows

In this and the following episodes, we'll look at the requirements and considerations to make your
workflow a re-usable resource that you can publish and share. If all you want is to archive your
code alongside your publication then you may find that some of this is overkill, but if you
envision your workflow being actively used in future then the time invested to follow these steps
will be well worth it.

We will start with the assembly workflow from [linky] episode X of the introductory lessons. We
will take the sample answer and imagine this is a workflow that we are looking to publish. If you
have your own working solution you may want to work with that, but for demonstration purposes
we will use [linky] the standard sample answer.

In this first episode, we'll consider how and why to make a toy dataset for our workflow.

## Integration testing and regression testing

We are starting with a working workflow, so it may seem strange that we are now talking about
testing it, but we want to assure ourselves that it will behave when set to work
on a new analysis task or in a new environment. A non-portable workflow may depend on:

   * Specific software packages, including Snakemake itself
   * Features of a particular operating system version
   * Specific filesystem paths or local databases being available
   * Network resources or internet access

A software test consists of some sample input data, a command to run, and an expected
result. The test may run through the whole workflow, which would be termed an "integration test",
or you could make individual tests that run specific rules in the workflow and these would be
called "unit tests". Either approach is valid here. Such tests can be re-run at any time to
check that the whole workflow is operational. When setting up the workflow on a new system,
this provides reassurance that everything is working before any attempt to analyse real data.

Further, if you modify your workflow in future, or want to use updated versions of tools, you want
to be sure that your changes do not cause a new bug in something that already worked. Such bugs
in code are called "regressions" and "regression tests" aim to show them up. In practise, both
"unit tests" and "integration tests" can be effective regression tests as long as you have an easy
way to go back and re-run them.

A "test suite" is a collection of unit tests and/or integration tests that can be run quickly,
ideally with a single command. For this simple workflow, our "suite" can be an integration test
that runs the whole workflow on a *toy dataset*.

## What is a toy dataset?

A toy dataset is a small sample dataset that can be used to run the pipeline. You may want to
include other test datasets with your pipeline, but having one which is as small as possible has
many uses.

1. The small toy files can be stored directly in Git with the code.
1. The whole pipeline can be run very quickly even on a small computer, to check that all the
   tools, conda envs, containers, etc. are behaving themselves.
1. In Snakemake, you can use the toy dataset as a basis for generating a sample DAG (ie. a
   diagram to represent your workflow).
1. Even a small FASTQ file nowadays will have thousands of lines, but a user or developer should
   be able to directly examine the toy dataset and the intermediate and final results on their
   screen.
1. If you developed the pipeline on your own research data, this may not be freely redistributable.
   The toy dataset should be at least as re-usable as the pipeline itself.

## A strategy for making a toy dataset

We're working with RNA reads in FASTQ files, so an obvious idea is to down-sample these files to,
say, a couple of hundred reads. However, these reads are going to be from different parts of the
yeast transcriptome and are not going to assemble together. We could map reads back to the draft
assembly and look to extract an assembly-friendly set, but in practise it's easier to go
back to the transcriptome and make up some synthetic reads.

################# code

$ zcat Saccharomyces_cerevisiae.R64-1-1.cdna.all.fa.gz | grep '^>' | wc -l
6612

######

In this code, `zcat` deals with uncompressing the gzipped data, `grep` is selecting just the FASTA
header lines, and this reveals that there are...

6612 transcripts in our yeast transcriptome. Let's take the first five.

################# code

$ zcat Saccharomyces_cerevisiae.R64-1-1.cdna.all.fa.gz | \
   fasta_formatter | \
   head -n 10 > first5.fa

######

In the code above, the `fasta_formatter` is part of the standard fastx_toolkit (the same set of
tools that includes `fastx_trimmer`) and this joins the sequences onto a single line each, meaning
that the first ten lines, as saved from the `head` command, contain the first five transcripts.

We will now use `wgsim`, a simple short read simulator. It can be installed standalone but is
normally packaged with samtools [linky] (check it really is in the conda package).

############### code

$ wgsim -e0.01 -d50 -N100 -175 -275 top5.fa toy_1_1.fq  toy_1_2.fq
$ wgsim -e0.01 -d50 -N100 -175 -275 top5.fa toy_2_1.fq  toy_2_2.fq

######

With the above commands, we generate a total of 200 read pairs (`-N100`) with a read length of 75
(`-175 -275`). The `-d` parameter sets the virtual fragment length and the `-e0.01` add a few
errors in the reads. We can confirm that these reads do assemble (to some extent) with velvet.

############# code

$ velveth velvet_toy 21 -shortPaired -fastq -separate toy_1_1.fq toy_1_2.fq toy_2_1.fq toy_2_2.fq
$ velvetg velvet_toy
$ tail velvet_toy/contigs.fa

########

The exact result will vary since there are random variables in both the `wgsim` read generation
and the `velveth` graph construction, but you should expect to see around 60 contigs. By no means
a good assembly, but this is not the point. The longest contig is a few hundred bases and this
is enough. We now have a suitable toy dataset for our workflow.

############## callout

Finding the right flags to `wgsim` to make the commands above took a mixture of guesswork and
trial and error. Different options were tried and Velvet was repeatedly re-run to see the result.
There is no correct answer here, we just need something that works, and as the commands run in a
matter of seconds the tinkering process can be pretty quick.

We can also see in the *wgsim* log messages that one of the transcripts is too short to use, so we
are actually simulating reads from just four sequences. If we were really sequencing such short
fragments on a physical sequencer then the read pairs would overlap and we might even sequence
right through the whole fragment into the adapter. *wgsim* cannot simulate this so it just skips
the short sequence.

####

## Making the test suite

These toy reads will be the basis of the test suite.

** Get them to adapt the Snakefile so that it processes these reads

It's easiest to decide if a test has passed of failed if the expected output is identical every
time.
Get them to work out if this is the case here.

Failing that, ask yourself what should always be true. Let's have a multiple guess:

1) There will always be a contig longer than 300bp
2) The output file will always have 4 lines
3) The last contig will always be "contig_57"
4) Other stuff

And finally we'll just have a script that runs the test and makes some cursory checks.

$ snakemake -Fn ...
$ check that the final output has 4 lines in there

## Callout - continuous integration

No technical details, but mention that once we have Git we can consider GitHub CI
or whatever.

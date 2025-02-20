---
title: Separating code and configuration
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Separate the code of our workflow from the configuration
- Remove hard-coded paths and machine-specific settings

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What makes for tidy code, and why do we care?
- What features of Snakemake can help?
- How does this fit in with testing?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Making a configurable workflow

In the last episode, we edited the assembly workflow to run on the toy dataset. This involved
editing the Snakefile directly so we now have two versions of the Snakefile. We want to separate
the code from the configuration, and have a single Snakefile that assembles either the original
reads, or the toy reads, or any other reads, according to the *config* settings.

We have already learned about the use of the `--config` and `--configfile` options to Snakemake,
and we will now rearrange our workflow into three files:

* `Snakefile` which will have all the rules
* `yeast_config.yaml` which will assemble the original test reads
* `toy_config.yaml` which will assemble the toy reads

Remember the three changes we had to make to the Snakefile (or use the Linux `diff` command to
remind you what changed).

```bash
$ diff  sample_answer.Snakefile Snakefile
3c3
< CONDITIONS = ["ref", "etoh60", "temp33"]
---
> CONDITIONS = ["toy"]
16,17c16,17
<         read1 = "reads/{sample}_1.fq",
<         read2 = "reads/{sample}_2.fq"
---
>         read1 = "tests/integration/toy_reads/{sample}_1.fq",
>         read2 = "tests/integration/toy_reads/{sample}_2.fq"
32,33c32,33
<         read1s = ["cutadapt/{condition}_1_1.fq", "cutadapt/{condition}_2_1.fq", "cutadapt/{condition}_3_1.fq"],
<         read2s = ["cutadapt/{condition}_1_2.fq", "cutadapt/{condition}_2_2.fq", "cutadapt/{condition}_3_2.fq"],
---
>         read1s = ["cutadapt/{condition}_1_1.fq", "cutadapt/{condition}_2_1.fq"],
>         read2s = ["cutadapt/{condition}_1_2.fq", "cutadapt/{condition}_2_2.fq"],
```

Here is a potential version of `yeast_config.yaml`. There are other things we could configure,
like the list of kmer lengths, but this is the minimum to support being able to use the same
Snakefile for both the toy and original data.

```
conditions: ["ref", "etoh60", "temp33"]
reads_dir: "reads"
replicates: ["1", "2", "3"]
```

::::::::::::::::: challenge

1. Make a config file for the toy data
2. Modify the code to use the settings in the config file
3. Modify the `run.sh` file and ensure the test still passes

Hint: to vary the number of replicates in the *concatenate* rule, you can use an `expand()`
expression, but you will need to put extra brackets around `{{condition}}` to avoid getting a
`WildcardError`.

::::::::


Note sure this is a whole episode? It's basically a big code tidy-up, and we have to explain what
it means for code to be "tidy" and why we care. Most tutorials exhort you to be tidy from the
start, but in practise everyone making a new workflow will start by just "getting it done" and
worry about tidying it up later. So we'll not deny this reality. You must be prepared to
substantially re-write and re-organise your code to make it re-usable, even if you got it working
for your initial analysis.

Suggested file locations as per Snakemake. Where's the link?? Now we have a Snakefile and a toy
dataset we can start to split things out.

https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html

Snakefile into workflow/Snakefile
Sample data into .tests/integration (explain the term integration testing. Maybe in the previous ep?)
Sample config into ???

## What makes for tidy workflow code?

The goal is that:

1) We can run the unmodified Snakefile on different input data
   - configurable items
   - hard-coded strings or paths (eg. location of reference files)
   - optional steps (not sure if it belongs here but it's relevant)
   - optional/additional prameters to rules (as a general strategy)
2) We can run the unmodified Snakefile on different computers
   - hard-coded strings or paths eg. scratch or home or working directories
3) We do not have everything in the same directory. Specifically:
   - code
   - configuration
   - input data
   - intermediate files
   - output results
   - logs and metrics

At what point do I want to talk about "snakedeploy"? I still don't get what it does. Need to do
some testing of that on the sample workflow.

:::::::::::::: callout

## What if the different conditions have a different number of replicates?

This situation can be handled. The configuration could look like this:

```
conditions:
    ref:    ["1", "2", "3"]
    etoh60: ["1", "2"]
    temp33: ["2", "3"]
reads_dir: "reads"
```

The input for the *concatenate* rule now gets a bit more complex.

```
input:
    read1s = lambda wc: expand( "cutadapt/{condition}_{rep}_1.fq",
                                condition=wc.condition,
                                rep=config["conditions"][wc.condition]),
    read2s = lambda wc: expand( "cutadapt/{condition}_{rep}_2.fq",
                                condition=wc.condition,
                                rep=config["conditions"][wc.condition]),
```

To understand this, you need to read about [input functions](
https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#input-functions)
in the Snakemake docs. A `lambda:` expression in Python defines an anonymous function.

::::::::::

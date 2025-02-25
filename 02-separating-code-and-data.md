---
title: Separating code and configuration
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Separate the code of our workflow from the configuration
- Have a single workflow that runs on our original and test data

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What needs to be configurable in our workflow?
- How do we express these settings using the Snakemake config mechanism?
- How does this fit in with testing?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Making a configurable workflow

In the last episode, we edited the assembly workflow to run on the toy dataset. This involved
editing the Snakefile directly so we now have two versions of the Snakefile. We want to separate
the code from the configuration, and have a single Snakefile that assembles either the original
reads, or the toy reads, or any other reads, according to the *config* settings.

We have already learned about the use of the `--config` and `--configfile` options to Snakemake,
and we will now rearrange our workflow and configuration into three files:

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

:::::::::: solution

This is an equivalent config file, which could go under `tests/integration/toy_config.yaml`:

```
conditions: ["toy"]
reads_dir: "tests/integration/toy_reads"
replicates: ["1", "2"]
```

In the Snakefile, at the top:

```
CONDITIONS = config["conditions"]
```

In the *cutadapt* rule, there are other ways to write this but this is probably the simplest:

```
input:
    read1 = config["reads_dir"] + "/{sample}_1.fq",
    read2 = config["reads_dir"] + "/{sample}_2.fq",
```

In the *concatenate* rule, we need to change not only the input patterns but the number
of inputs based upon the configured replicates. Again, there are different ways to do this
but this will work. The `{rep}` gets replaced with replicate numbers by the `expand()` function
but then the `{{condition}}` keyword remains as `{condition}` so it still acts as a wildcard.

```
input:
    read1s = expand("cutadapt/{{condition}}_{rep}_1.fq", rep=config["replicates"]),
    read2s = expand("cutadapt/{{condition}}_{rep}_2.fq", rep=config["replicates"]),
```

In the `tests/integration/run.sh` script we just need an extra `--configfile` parameter
when calling snakemake.

```
snakemake -F --use-conda -j1 --configfile tests/integration/toy_config.yaml
```

:::::::

:::::::::::

If you have not done so already, move the `toy_config.yaml` file to the tests/integration
directory and run the `run.sh` script to reassure yourself the test still works.

:::::::::::::: callout

## What if the different conditions have a different number of replicates?

This situation can be handled. The configuration could look like this.

```
reads_dir: "reads"
conditions:
    ref:
      replicates: ["1", "2", "3"]
    etoh60:
      replicates: ["1", "2"]
    temp33:
      replicates: ["2", "3"]
```

The input for the *concatenate* rule now gets a bit more complex.

```
input:
    read1s = lambda wc: expand( "cutadapt/{condition}_{rep}_1.fq",
                                condition=wc.condition,
                                rep=config["conditions"][wc.condition]["replicates"]),
    read2s = lambda wc: expand( "cutadapt/{condition}_{rep}_2.fq",
                                condition=wc.condition,
                                rep=config["conditions"][wc.condition]["replicates"]),
```

To understand this, you need to read about [input functions](
https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#input-functions)
in the Snakemake docs. Using input functions allows you to go beyond the basic wildcard
replacement logic for rule inputs. The `lambda:` keyword in Python defines an anonymous function.

::::::::::

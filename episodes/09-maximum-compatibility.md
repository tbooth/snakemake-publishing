---
title: Maximum compatibility
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Add resources to our rules
- Consider containers

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What helps a workflow to run on different environments.

::::::::::::::::::::::::::::::::::::::::::::::::::

This episode is a work in progress may be dropped. It has been temporarily removed from
the lesson list in `setup.py`.

## Setting resources for rules

When Snakemake runs a workflow using the default *local* executor, it's really no different than
running the program yourself from the command line or a script. You can tell Snakemake how many
things to run at once using the `--cores` or `--jobs` options, along with the *threads:* setting
on your rules. This was covered previously.

When a workflow is run on a cluster or cloud system, Snakemake packages each job into a cluster
job. Cluster managers like *SLURM*, *SGE*, and the various *Kubernetes* batch schedulers, will
try to assign very specific resources to jobs in order to make efficient use of the hardware.
The settings to do this vary from system to system, but Snakemake handles these details. You
jost need to define the job-local resources for your rules.

Here's an example, modifying the exiting *cutadapt* rule with both a max number of threads and
a memory usage requirement of 10 megabytes.

```
rule cutadapt:
    output:
        read1 = "cutadapt/{sample}_1.fq",
        read2 = "cutadapt/{sample}_2.fq"
    input:
        read1 = READS_DIR + "/{sample}_1.fq",
        read2 = READS_DIR + "/{sample}_2.fq",
    log: "logs/cutadapt_{sample}.log"
    params:
        adapter = config.get(adapter, "AGATCGGAAGAGC")
    threads: 4
    resources:
        mem = "10GB"
    conda: "envs/assembly_conda_env.yaml"
    shell:
        """exec >{log} 2>&1
           cutadapt -j {threads} -a {params.adapter} -A {params.adapter} \
            -o {output.read1} -p {output.read2} \
               {input.read1}     {input.read2}
        """
```

If you are dealing with sofware that needs GPU cores, or requires temprary disk space outside
of the working directory (aka. *scratch space*) you can also specify this as a resource
requirement. It's also possible, in the latest versions of Snakemake, to scale the resource
requested according to the input data size.

## Cross-platform compatibility

Snakemake is designed to run on Linux, Mac and Windows systems, or indeed any system that supports
Python. However, your workflow is unlikely to run on a different system, or not without some
extra work.

We already mentioned Conda.

What about making your workflow run on Mac/Linux/x86/arm. Do we even care?

## Apps in containers

Conda is already covered in the main tutorial but maybe there is a bit more to say.

Containers are a whole new topic.

* Describe a container as being somewhere between a conda environment and a VM
* This brings some advantages
  * Faster to start and stop than a VM
  * Use less resource than a VM
  * May work in environments where a VM is not possible (eg. on HPC systems)
  * Runs on more platforms than Conda
  * You can add software that's not in, or for some reason cannot be in, conda
  * In some cases, more secure than the other options
  * Distrubutable as a single "container image" file
* But also disdavantages
  * If you can't re-use an existing container image, building one is harder than making a conda env
  * Not supported on some environments
  * Harder to debug than software in Conda environents
  * Harder to update or fix specific issues ("build once" mentality)
  * There are compatibility issues between competing container applications
  * Cannot overcome CPU/GPU incompatibility problems

Snakemake uses [Apptainer](https://apptainer.org/) to run jobs within containers, be they local
jobs or cluster jobs. You can use a pre-existing container from, say, [BioContainers](
https://biocontainers.pro/tools/cutadapt), or make your own manually using [apptainer build](
https://apptainer.org/docs/user/latest/quick_start.html#building-images-from-scratch), or use
the built-in `--containerize` [option of Snakemake](
https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#containerization-of-conda-based-workflows)
to generate container images based off a conda environment specification.

*Side note. I'm not sure why `--containerize` has been built into Snakemake. It seems to me that
this should be a separate general purpose tool??*

## Callout - HPC systems and cloud compatibility

Also, what about running on an HPC system? Do I want to go into any details about the profile setup
for that? Maybe just link to the docs as a callout.

That's probably it. Plenty to be getting on with.

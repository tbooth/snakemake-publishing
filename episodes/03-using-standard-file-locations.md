---
title: Using standard file locations
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand the Snakemake recommended file layout
- Reorganise our sample code to comply with this standard

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What tools exist to check that the workflow files are all present and correct?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Moving files to appropriate locations

There is a recommended layout for files in a Snakemake workflow. Following this convention will
help other users to understand your workflow when you share it.

The following listing is taken from the [Snakemake reference documenation](
https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html)

```
├── .gitignore
├── README.md
├── LICENSE.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── .tests
│   ├── integration
│   └── unit
├── images
│   └── rulegraph.svg
├── workflow
│   ├── rules
│   │   ├── module1.smk
│   │   └── module2.smk
│   ├── envs
│   │   ├── tool1.yaml
│   │   └── tool2.yaml
│   ├── scripts
│   │   ├── script1.py
│   │   └── script2.R
│   ├── notebooks
│   │   ├── notebook1.py.ipynb
│   │   └── notebook2.r.ipynb
│   ├── report
│   │   ├── plot1.rst
│   │   └── plot2.rst
│   ├── Snakefile
│   └── documentation.md
├── config
│   ├── config.yaml
│   └── some-sheet.tsv
├── results
└── resources
```

What should we change in the light of this?

* The `.gitignore` file is related to using *Git*, which we will come to in the next chapter.
* The `README.md` should be a text file briefly describing the workflow. We'll add this now.
* We'll need a license before we share the code, but we'll consider this later.
* We'll skip the `CODE_OF_CONDUCT.md` and `CONTRIBUTING.md` files for now.
* We have a `tests` directory, but the official layout says it should be `.tests`
* We'll generate a rulegraph using the `--dag` option of Snakemake
* We'll move our `Snakefile` and conda environment file as suggested
* We'll leave all our rules in the main Snakefile for now, but notice that we have the option
  to break out rules or groups of rules into modules.
* We have no scripts, notebooks, or reports in our simple workflow just now.
* We should put our `yeast_config.yaml` into a `config` subdirectory.
* The Snakemake workflow should put results into a `results` subdirectory.

We'll tidy our code to comply with the above points, and then keep to using standard file
locations from now on. Let's do the simpler stuff first.

## Making a rulegraph.svg

We could make the rulegraph from the yeast transcriptome data or the toy data. We'll use the
toy dataset.

```bash
$ snakemake -F --dag --configfile tests/integration/toy_config.yaml > rulegraph.dag
```

This file made by Snakemake is in *GraphViz* formay, not *SVG* format, but we can use the `dot`
command line tool to generate the *SVG*.

```bash
$ dot -Tsvg rulegraph.dag -o rulegraph.svg
```

*SVG* is the Scalable Vector Graphics format. If you want to view the SVG directly you can try
loading it in a web browser.

## Moving the files around

First, we'll make the required subdirectories.

```bash
$ mkdir -p images workflow/envs config results
```

Now move the existing files

```bash
$ mv rulegraph.svg images/
$ mv Snakefile workflow/
$ mv mv assembly_conda_env.yaml workflow/envs/
$ mv yeast_config.yaml config/
```

## Renaming the tests directory

The official layout guidance says that that tests should be in a directory called `.tests` which
would be a hidden directory. Do we really need to hide our tests away in order to comply with
the standard? A compromise is to rename the directory, but then make a symlink to
keep it visible.

```bash
$ mv tests .tests
$ ln -s .tests tests
```

## Adding a README.md

The idea of this *README* is to provide a top-level summary and introduction to your workflow.
Some people will just add a brief description, while others will add comprehensive documentation
in this file. We'll just add a brief description.

The `.md`/*Markdown* format, is an [enhanced text format](
https://github.com/adam-p/markdown-here/wiki/markdown-cheatsheet) that supports all sorts of
formatting but for now we just need to know that lines beginning `#` will be shown as headings.

Paste this into the file `README.md`

```markdown
# Demo de-novo assembly workflow

This workflow assembles short reads by running the obsolete Velvet assembler. It performs
the same assembly on every sample with different k-mer length settings, and reports the longest
contig in each case.
```

## Output to the results directory

All of our output files currently go into subdirectories within the current working directiory,
but the standard says they need to be under `results`. On the face of it, it seems that we need to
re-write all of our rules and add `results/` to all the paths. Fortunately there is a trick to
avoid this.

```
READS_DIR = os.path.join("..", config["reads_dir"])
workdir:
    config.get("results", "results")
```

Then in the *cutadapt* rule, modify the *input* part to reference the new variable.

```
input:
    read1 = READS_DIR + "/{sample}_1.fq",
    read2 = READS_DIR + "/{sample}_2.fq",
```

The use of `os.path.join("..", ...)` will work regardless if the reads directory is given as a
relative or absolute path.

The *workdir* will now default to `results`, as per the standard layout, but we can override it.

:::::::::::::::: challenge

Edit the `toy_config.yaml` file so that the results for test runs go into a directory named
`test_results`. See if the workflow is still working after all these changes. What do we still
need to fix?


:::::::

Do I lint the code at this point?

At what point do I want to talk about "snakedeploy"? I still don't get what it does. Need to do
some testing of that on the sample workflow.




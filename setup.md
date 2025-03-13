---
title: Setup
---

## Data Sets

<!--
FIXME: place any data you want learners to use in `episodes/data` and then use
       a relative link ( [data zip file](data/lesson-data.zip) ) to provide a
       link to it, replacing the example.com link.
-->
We'll need to start with a simple example Snakefile, which in this case is the sample
answer given to the [sequence assembly challenge](
https://carpentries-incubator.github.io/snakemake-novice-bioinformatics/12-assembly_challenge.html).

Download [the Snakefile](files/setup/Snakefile) and save it.

If you do not have the yeast files from the intro course, unpack the sample dataset tarball from
[https://figshare.com/ndownloader/files/42467370](https://figshare.com/ndownloader/files/42467370)

You may do this in the shell with the command:

```bash
$ wget --content-disposition https://figshare.com/ndownloader/files/42467370
```

The [tar file](https://www.gnu.org/software/tar/manual/html_node/Tutorial.html)
needs to be unpacked to yield the directory of files used in the course. In the shell you may
do this with:

```bash
$ tar -xvaf data-for-snakemake-novice-bioinformatics.tar.xz
```

You will also need to rename the files as is normally done at the start of [episode 06](
https://carpentries-incubator.github.io/snakemake-novice-bioinformatics/06-expansion.html).

```bash
$ cd yeast/reads
$ rename -v -s ref ref_ *
```

[See this link](https://figshare.com/articles/dataset/data-for-snakemake-novice-bioinformatics_tar_xz/19733338/1)
for details about this dataset and the redistribution license.

## Software Setup

::::::::::::::::::::::::::::::::::::::: discussion

### Details

You will need the Snakemake software installed and working with conda. One way to do this is to
follow the same setup instructions as for the [Snakemake for Bioinformatics lesson](
https://carpentries-incubator.github.io/snakemake-novice-bioinformatics/#software-installation).

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: spoiler

### Windows

This lesson is currently not tested to work on Windows. You may use a WSL Linux environment,
or else connect to a Linux system.

::::::::::::::::::::::::

:::::::::::::::: spoiler

### MacOS

Use Terminal.app

::::::::::::::::::::::::


:::::::::::::::: spoiler

### Linux

You are good.

::::::::::::::::::::::::


---
title: RO-Crates
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Download the RO-crate for our workflow
- Understand the structure of an RO-Crate
- Put the metadata file into Git

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are RO-crates in general?
- What are the specific types of RO-Crate?
- What can I do with an RO-Crate?

::::::::::::::::::::::::::::::::::::::::::::::::::

## A general introduction to RO-Crates

An [RO-Crate](https://www.researchobject.org/ro-crate/about_ro_crate) is simply a *zip* file that
contains a collection of data files, and includes a structured description of what those files are.

The base RO-Crate specification says that you can include any types of data files you like, but
it does stipulate that the description must be in a file named `ro-crate-metadata.json` and that
the format of this file is [JSON LD](https://json-ld.org/).

We will not talk about the details of the JSON LD format, since it's pretty technical and also
because WorkflowHub is going to make the file for us. But we do need to understand that it
contains:

* Information about the data collection as a whole
  * who created it
  * who may use it (the license)
* A description of what each of the individual data files are
  * the formats of the files
  * the content of the files
  * how the specific files relate to each other

The *JSON LD* file is not designed to be read directly by end users. Rather, software like that
running on WorkflowHub will read and summarise the file. User-readable documentation is best
written in a format such as Markdown, and from the RO-Crate perspective, is regarded as part of
the dataset.

## Putting workflows into RO-Crates

The *RO-Crate* standard was not specifically designed for sharing workflows, but the base standard
has been extended by several *profiles*.

* [Workflow RO-Crate](https://about.workflowhub.eu/Workflow-RO-Crate/)
  * [Workflow Run Crate](https://www.researchobject.org/workflow-run-crate/profiles/workflow_run_crate/)
    * [Provenence Run Crate](https://www.researchobject.org/workflow-run-crate/profiles/provenance_run_crate/)
  * [Workflow Testing RO-Crate](https://crs4.github.io/workflow-testing-ro-crate/)

The most comprehensive of the profiles is the *Provenance Run Crate*. The goal of this standard is
to cater for users who want to share their workflow outputs, but desire to very carefully record
exactly how they obtained those output files, including all the *input data*, *software versions*,
*parameters*, *platform* and *procedure (workflow)* used. Imagine for example a forensics
laboratory where the whole analysis process could be audited and questioned, even several years
after the analysis. Work is ongoing to add reporting plugins to Snakemake that provide for
automated recording at this level of detail in a single archive.

The *Workflow Run Crate* is a less stringent version, recording that a particular analysis was
made with a particular workflow, and the results of that analysis.

The *Workflow Testing RO-Crate* would be suitable to capture information about our toy dataset and
any other tests included with the workflow. (But we'll not look further into this topic within
this course).

Here, will will just look at the basic *Workflow RO-Crate*. As suggested in [the WorkflowHub docs](
https://about.workflowhub.eu/developer/how-to-make-a-workflow-ro-crate/):

> The most convenient way to make a workflow RO-crate at this moment is by making use of
> WorkflowHub capabilities.

## Getting the RO-Crate matadata from WorkflowHub

So we will download our workflow as an *RO-Crate* from WorkflowHub.  Use the
*Download RO-Crate* button on the website. Assuming you save the resulting `.crate.zip` file in
your `Downloads` directory, you can look at the contents on the command line, or you can open
the file in GUI.

```bash
$ ls ~/Downloads/*.crate.zip
...see the actual filename...
$ unzip -tv ~/Downloads/workflow-XXXX.crate.zip
```

Aside from the files we made ourselves, we see:

```output
    testing: ro-crate-metadata.json   OK
    testing: ro-crate-preview.html    OK
```

The HTML file is just a visual preview of the JSON file. We extract the JSON file into our source
directory.

```bash
$ unzip ~/Downloads/workflow-XXXX.crate.zip ro-crate-metadata.json
```

And commit it back to GIT.

```bash
$ git add ro-crate-metadata.json
$ git commit -m "Add RO-Crate metadata"
```

Now you have a backup of all the information that WorkflowHub stores about the workflow, saved
along with your code.

Why do we do this, again? I feel like I'm losing myself a little.

---

When your primary interest is to share a workflow so that other people can use it for their own
data, you are talking about a workflow crate.

These two requirements are very closely related. If you have a good test dataset for your workflow,
as we have carefully created for this example, then the idea is to run your full workflow on the
toy data and capture all the provenance information from that, so that the crate will capture
everything needed to validate and re-use the workflow. (re-word this. what can we do practically?)

Q1 - What does WorkflowHub put into the RO-Crate (we can take a look!).

Q2 - What else might we want to include (if we go into the provenance stuff). Can I actually demo
anything in this regard?

---

This is where I give a general intro to RO-Crates.

What do they do, in general.

What do they do, for you.

What do you actually need to know:

1) If you are just looking to share your workflow, WFH will deal with the RO-Crate stuff for you

2) If you need to make an RO-Crate yourself, you can ? use the plugin ???

3) And then?

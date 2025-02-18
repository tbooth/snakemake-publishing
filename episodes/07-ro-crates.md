---
title: RO-Crates
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Download the RO-crate for our workflow
- Understand the structure of an RO-Crate
- ???

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are RO-crates in general?
- What are the specific types of RO-Crate?
- What can I do with an RO-Crate?

::::::::::::::::::::::::::::::::::::::::::::::::::

# A general introduction to RO-Crates

An [RO-Crate](https://www.researchobject.org/ro-crate/about_ro_crate) is simply a zip file that
contains a collection of data files, and includes a description of what those files are.

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

The description is not designed to be read directly by end users. Rather, software like that
running on WorkflowHub will read and summarise the file. User-readble documentation is best
written in a format such as Markdown, and from the RO-Crate perspective, is regarded as part of
the dataset.

# Putting workflows into RO-Crates

Describe the related testing crate and provenance crate.

When your primary interest is in sharng your results, but you want to very carefully record
exactly how you obtained those results, including all the input data, software, parameters,
platform and procedure (workflow) used. This a job for a provenance crate.

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

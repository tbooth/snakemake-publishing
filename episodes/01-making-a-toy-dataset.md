---
title: Making a toy dataset
teaching: 30
exercises: 30
---

In this episode, we'll consider how and why to make a toy dataset for your workflow.

## Integration testing and regression testing (and unit testing)

TODO - Give some practical examples.

We already have a working workflow, so why are we now talking about testing it? Things may
not work so well when the workflow is set to work on a new analysis task on a
new environment. Specifically...

1) A specific step may fail with a new input file. For example, [add a better example]
You can't anticipate every possible case but it's good practise to look through your rules and see
if there are obvious cases that could be tested. Such checks would be called "unit tests" as they
test a single unit (in this case a rule) in your workflow.

2) A workflow that runs on one system (like your local compute server) may not run on another system.
Maybe you depend on particular software versions, including Snakemake itself? Maybe you assume
that certain paths or local databases are available. Maybe you assume that there is always network
access. Maybe you have bugs that only manifest when multiple jobs are run in parallel.
Tests that check that the whole workflow is operational are "integration tests" as they
ensure all the components are working together. Integration tests are great to have when you are
setting up the workflow on a new system, as they reassure you everything is working before you
try to analyse real data.

3) If you modify your workflow in future, how do you know that changes to add functionality or to
fix one problem do not cause a new problem elsewhere? Such bugs in code are called "regressions"
and "Regression tests" are checks you can make to reassure yourself you did not introduce any.
In practise, both "unit tests" and "integration tests" can act as regression tests as long as you
have the ability to go back and re-run them.

So, a "test suite" is a collection of unit tests and/or integration tests that can be run, ideally
with a single script, whenever you modify the code or install the workflow on a new system. At
minimum, your test suite will have an integration test that involves running the whole workflow
based on your toy dataset.

Right. Now make that relevant to the example at hand.

## What is a toy dataset?

A toy dataset is a small sample dataset that can be used to run the pipeline. You may want to
include other test datasets with your pipeline, but having one which is as small as possible has
many uses.

1. The small toy files can be stored directly in Git with the code.
1. The whole pipeline can be run very quickly even on a small computer, to check that all the
   tools, conda envs, containers, etc. are behaving themselves.
1. In Snakemake, you can use the toy dataset as a basis for generating the sample DAG (ie. a
   diagram to represent your workflow).
1. Even a small FASTQ file nowadays will have thousands of lines, but a user or developer should
   be able to directly examine the toy dataset and the intermediate and final results.
1. If you developed the pipeline on your own research data, this may not be freely redistributable.
   The toy dataset should be at least as re-usable as the pipeline itself.

## Strategy for making a toy dataset

Here we need a practical example. We can base it on the existing sample data.

Do I want to say anything about making a sample dataset for genome assembly?
- Maybe using https://github.com/lh3/wgsim

I'm not sure if this belongs here, but it is cool, and worth a look.

Yes, I think it does. Making a toy "genome" and then generating some reads from it is a good
idea.

So I could add the stuff I tried out in ~/wgsim_test and make it be a practical, then show it
working on the assembly workflow.



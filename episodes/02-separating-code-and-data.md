---
title: Separating code and data
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

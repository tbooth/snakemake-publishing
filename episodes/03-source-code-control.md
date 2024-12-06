---
title: Source code control
teaching: 30
exercises: 30
---

I cannot introduce GIT in a single module. So what do I need to say about it?

- Git records the history of your files, but you control that history by making commits, branches
  and tags.

Then define:

Commit = a snapshot of all code files at a certain point it time. Git gives each commit a GUUID so
you can always refer to it unambiguously and retrieve the code as long as it is stored in some
git database.

Branch = a series of commits recording code development. In the simplest case we work on a single
branch and the commits just form a timeline. You can make new branches for various reasons, but
we'll not worry about that here

Tag = a name you attach to a commit. We want to use tags to indicate versions of the code. The
match between the tag in git and the version in Workflowhub wil tie everything together.

## Git + GitHub + WorkflowHub

Explain how these components interact. I need a diagram with paths going between them.

## Sematic versioning

This probably should live here. I think most people have seen this. It bears repeating:

v1.2.3 = major + minor + patch

Explain what they do. Changing to a new patch version should not affect the output at all, other
than in the case of fixing something which is clearly broken.

The decision over whether to make a major or minor release is more subjective. For some software
(bowtie and bwa are a case in point) the new major release is a re-write such that the new version
is essentially a whole new program in itself, so it's reasonable to say that making a major release
is on the same level as giving the software a new name.

v0.x releases - used for development. The minor and patch numbers are understood to be less
meaningful

v24.12.p format - a variation where you still use the major+minor+patch format but rather than just
adding the the major and minor versions sequentially you use the 2-digit year (24) and the month.
The patch number is still just a sequential number. Used in a few popular projects, but an obvious
isse is that you can never have more than one minor release in a month.



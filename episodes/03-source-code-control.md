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



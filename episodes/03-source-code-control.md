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

Branch = a series of commits recording code development. In the simple case we work on a single
branch and the commits will form a timeline. You can make new branches for various reasons, but
we'll not worry about that here, so just think of a branch as a timeline of work.

Tag = git allows you to give a name to any commit. We want to use tags to indicate versions of the
code. The match between the tag in git and the version in Workflowhub will tie everything together.

## Git + GitHub + WorkflowHub

Explain how these components interact. I need a diagram with paths going between them.

GitHub is the most popular service for hosting code managed in Git. It's by no means the only one,
as there are other public Git repository hosts, and you may well have one hosted by your
institution. It's also possible to use Git without a repository, but this negates many of the
advantages of using Git.

Note that moving code, with the entire commit history, between different Git hosting services is
very easy, so you will not be locked in to your first choice.

## Callout on Git dangers

Many people worry about sharing their un-finished code in a public repository. In practise, there
is not much to fear but there are some specific dangers.

The main one is leaking passwords, authentication tokens, and private data,

*** TODO - maybe say something about credential management. Does that belong here?


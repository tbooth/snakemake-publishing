---
title: Source code control
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Put our code under version control with GIT
- Record our changes and edits in the GIT commit history
- Push our code to GitHub

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are the key ideas of source control?
- What are the essential GIT commands we need to manage our simple code?
- How can git tags help to manage our code updates?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Source code control with Git

Covering anything but the bare basics of Git here is not possible, but we need to know a few
fundamentals:

- *Git* is a program that takes snapshots of all the files in a directory, and saves a log of
  every snapshot taken.
- These snapshots are known as *commits*, and a sequential timeline of commits is known as a
  *branch*. In the simple case, we have a linear series of *commits* over time, so just one
  *branch*.
- You decide when to make a commit, and enter a comment to summarize what changed.
- Git can then show you a *history* of you commits, or tell you exactly what changed (a *diff*)
  between two version.

All this happens on your local system, by saving special files into a `.git` subdirectory, but
you have the option to *push* that information to a *remote server*. The most popular such system,
and the one we will use today, is GitHub.

- GitHub is a free-to-use service owned by Microsoft.
- It allows you to view your code on-line, and *pull* it to another computer.

## Git + GitHub

Explain how these components interact. I need a diagram with paths going between them. Maybe have
a version now and then a second version later that adds WorkflowHub? Yes!

### I think this probably goes in the version episode.
Tag = git allows you to give a name to any commit. Ultimataly we want to use tags to indicate versions of the
code. The match between the tag in git and the version in Workflowhub will tie everything together.

Here's what we need to do, practically:

1) Git init (see this creates ./.git) and "git add . && git commit" (adding a message)

"git log" to reassure ourselves we are good.

2) Ensure everyone has a GitHub account. Many will have one aready. How is the sign-up process
nowadays? Should we do this in advance?

As of March 2023, GitHub requires 2FA, so you will need either an authenticator app or to be
able to receive a code via SMS. We'll assume you use SMS. Once you are logged in on the browser
you will stay logged in.

Need to generate and add an SSH key. Step them through that.

Now push the code to a new personal repo. Again we need to do that.

??? Should I do the signups for GitHub and Workflowhub at the same time?

3) We're nto doing Workflowhub yet.

:::::::::::::::::::: callout

## GitHub dangers

Many people worry about sharing their un-finished code in a public repository, or relying on the
GitHub service. In practise, there is not much to fear, but there is one significant risk:

Avoid leaking passwords, authentication tokens, and private data into Git. If you ever
add such an item to a commit then removing it from the Git history is nearly impossible. Git is
designed to never forget! Having good habits about separating your code, configuration and data is
the best way to avoid this. For example, if your workflow needs to retrieve data from a server
with a user name and password, put this into a configuration file outside of the workflow source
directory.

You may also worry that data on GitHub might be corrupted or lost, but this is less of a worry.
The chain of commits on Git is cryptographically signed, so it's unlikely that code can ever be
tampered with. Also, moving code to another Git hosting service, along with the full commit
history, is very easy. If you start out using GitHub you can move to an alternative host at any
time.

:::::::::::::

*** TODO - maybe say something about credential management. Does that belong here?


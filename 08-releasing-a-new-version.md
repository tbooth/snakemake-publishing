---
title: Releasing a new version
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand sematic versioning
- Push a new workflow version to Workflowhub and GitHub
- Automate new releases

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What should I do to update the workflow?

::::::::::::::::::::::::::::::::::::::::::::::::::

## New versions of the workflow

It may well be that your workflow is used for one study, archived on WorkflowHub, and never
changed again. On the other had, you may use the workflow on some new data and find that you need
to add new features or options. Or maybe someone alse uses your workflow and writes to suggest a
bug fix or enhancement that you are happy to incorporate.

Releasing a new version is essentially the same process as submitting the workflow in the first
place:

* Make and test the code changes
* Commit the changes in Git
* Push to GitHub
* Register a new update on WorkflowHub.com

Having said this, there are some considerations that will help you and others to understand
the nature of the update.

## Sematic versioning

[Semantic versioning](https://semver.org/) is the established way to assign a version number to
any piece of software. You should use this scheme when releasing any updates to your workflow. A
sematic version looks like this:

:::::::::::::::::::: code

v1.12.3

::::::::::::::

The `v` is not part of the version, but is normally included to indicate what the numbers are
referring to.

Then `1` is called the major version, `12` is the minor version, and `3` is the patch version.
The choice of version number indicates to the user not only which release is the latest but also
how two releases relate to each other.

Guidance for sematic versioning tends to focus on the task of writing software libraries for which
there is a "public API". Workflows are a little different but analogous guidelines can still be
followed.

### Incrementing the patch version

This is for bug fixes and documentation changes only. If you add any new features or options you
should increment the minor version.

Changing to a new patch version should not affect the output of the workflow at all, other
than in the case of fixing something which is clearly broken. The versions of any software tools
used by the workflow should not change, other than to switch up to a new patch version of a tool
which addresses some bug.

In many cases there will be no need for a patch version, and so this number is normally 0 for
most pipeline releases.

### Incrementing the minor version

In the most common case, if you release an update to your workflow you will increment the minor
version. You can add new features, re-write parts of the process, and bring in new software. In
general it should be possible to re-do any previous analysis with the updated workflow, but it
is not necessary that you obtain an identical result, as changes to tool versions or default
parameters may affect that.

In our sample workflow, adding the ability to configure the adapter used by *cutadapt*,
for example, would warrant a new minor version.

### Incrementing the major version

The decision over whether to make a major or minor release is more subjective. For some software
(the *bowtie* and *bwa* read aligners are cases in point) the new major release is a whole
re-write such that the new version is more like a whole new program. It's reasonable
to say that making a major release is on the same level of significance as giving the software
or pipeline a new name.

A change in major version indicates to any users that they cannot assume the workflow will operate
as before, and they should re-read the documentation before using it. Most workflows will never
get a change so fundamental that a major version increment is necessary.

If we changed the assembler in our workflow from *Velvet* to something more modern this would
warrant a major new major version.

### Pre-releases

If you consider your workflow to be a work in progress but still wish to track the
version number you may version it as "0.1.0". The major version number of 0 indicates a
development pre-release. If you update the code you should increment the minor or patch releases
as above, but the distinction between these is understood to be less meaningful in the context
of development releases.

### Variants on sematic versioning

For some well-known software, you will see the versions like `24.12.0` - a variation where
major and minor version numbers encode the year and month of release. The patch number is still
just a sequential number. This works will in projects like Ubuntu Linux which have periodic
releases, but in the case of workflows or other software you write you are best to avoid it.

You will see that in WorkflowHub, since we did not specify a version, the versions look like
this:

> main @ 08522a2

That hexadecimal number comes from the start of the Git commit identifier
(as seen in the `git log` command) and serves as a unique identifier, but it say nothing about
the ordering and relationship between workflow versions.

## Updating our workflow

Let's make a small change to the workflow, allowing the user to set the adapter sequence for
*cutadapt* in the config file.

In the cutadapt rule, change this part of the *cutadapt* rule.

```
    params:
        adapter = "AGATCGGAAGAGC"
```

to

```
    params:
        adapter = config.get(adapter, "AGATCGGAAGAGC")
```

And commit to Git:

```bash
$ git add workflow/Snakefile
$ git commit -m 'Allow the adapter to be configured for Cutadapt'
$ git push
```

As noted above, this warrants an increase in the minor release version. As we didn't actually set
the version in the initial upload we'll say that the initial upload was *1.0.0* and this new
one will be *1.1.0*.

You can change the version for the workflow that's already in WorkflowHub, so do this now.
Use the *Edit* button in the *Version History* section at the bottom of your workflow page,
and change the version to *v1.0.0* (including the `v` here is a good idea as we'll see shortly).

## Making a versioned GitHub release

We'll make the release on the GitHub web interface. There are other ways to do it, but this
is the simplest.

1. On *github.com*, when viewing the *yeast_demo_assembly* repository, make sure the latest
   commit is showing up as the change you just pushed.
1. Click the **Create a new release** link on the right.
1. In the **Choose a tag** dropdown, enter the new tag *v1.1.0* and click the option that
   appears to **Create new tag: v1.1.0 on publish**.
1. You may enter a release title and description or hit **Generate release notes** to auto-fill
   these options.
1. Now **Publish release**

Back on *dev.workflowhub.eu*, we can tell the site about the new release.

1. Use the **Actions** > **New Version** option on the web interface
1. Confirm that we are fetching from the existing repository
1. This time, select the **v1.1.0** option from the **Tags** target
1. Accept all the changes and **Register** the new version.

:::::::::::::::::::::::::: callout

## Automated update from GitHub to WorkflowHub

It's overkill for most workflows, but if you want to be able to add new versions to WorkflowHub
without the chore of going to the site and manually importing then this is possible.

Instructions on the following page outline how to set up an automated submission action:

https://github.com/workflowhub-eu/submission-action

We already have a WorkflowHub account and a WorkflowHub team. We do need an API token. Get this
from the *dev.workflowhub.eu* website under
*Top-Right Menu* > *My Profile* > *Actions* > *API Tokens* > *New API Token*. Give the title
*github_release* and **Create**. Then leave the next page open so you can copy the token.

Back on *github.com*, make sure you are viewing the *yeast_demo_assembly* project and click the
*Settings* button in the top bar. On the left, select *Secrets and variables* and then select
*Actions* from the sub-options. Then click the button for **New repository secret**.

Give the name as *DEV_WORKFLOWHUB_API_TOKEN* (you must use this exact name) and paste in the
token that is shown in *dev.workflowhub.eu*. Then click **Add secret**.

Now you need to make sure there are two files in your repository. One is the
`ro-crate-metadata.json` obtained in the previous episode. You can edit this directly if you
want to change the information about your workflow when you make a new release. The other is
a file named `.github/workflows/submit_to_workflowhub.yml`. It must contain a version of the
text found at [the bottom of the instructions page](
https://github.com/workflowhub-eu/submission-action#user-content-test-workflow-submission-using-devworkflowhubeu
).

```
name: Workflow publishing on dev.WorkflowHub.eu

on:
  release:
    types: [published]

jobs:
  wfh-submit:
    name: dev.WorkflowHub.eu submission
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Submit workflows
        uses: workflowhub-eu/submission-action@v0
        env:
          API_TOKEN: ${{ secrets.DEV_WORKFLOWHUB_API_TOKEN }}
        with:
          team_id: 58
          instance: https://dev.workflowhub.eu
```

The *team_id* is *58* (which corresponds to the *Submission Tutorial* team), and the submission
happens only when a release is published, not on every `git push`.

Add and commit the files as before.

```
$ git add ro-crate-metadata.json .github
$ git commit -m 'Enable auto-submit'
$ git push
```

And finally, make a new release on GitHub as above. Since the workflow has not changed, call this
a patch release, and set the version tag to *v1.1.1*.

In GitHub, under the *Actions* tab, you should see the action triggered by the new release, and
any errors with the submission.

:::::::::::::::::

---
title: Releasing a new version
teaching: 30
exercises: 30
---

I need to come up with a change that is plausible.

Sematic versioning.

Manual upate process.

Auto update process.

Validation and re-testing.


## Sematic versioning

[Semantic versioning](https://semver.org/) is the established way to assign a version number to
a software product. You should use this scheme when releasing any updates to your workflow. A
sematic version looks like this:

############### code

1.2.3

############

Where "1" is called the major version, "2" is the minor version, and "3" is the patch version. The
choice of version number indicates to the user not only which release is the latest but also how
two releases relate to each other.

Guidance for sematic versioning tends to focus on the task of writing software libraries for which
there is a "public API". Workflows are a little different but analogous guidelines can still be
followed.

### Incrementing the patch version

This is for bug fixes only. If you add any new features or options you should increment the minor
version.

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

### Incrementing the major version

The decision over whether to make a major or minor release is more subjective. For some software
(bowtie and bwa are cases in point) the new major release is a whole re-write such that the new
version is essentially a whole new program. In this case it's reasonable to say that
making a major release is on the same level of significance as giving the software a new name.

A change in major version indicates to any users that they cannot assume the workflow will operate
as before, and they should re-read the documentation before using it. Most workflows will never
get a change so fundamental that a major version increment is nevessary.

### Pre-releases

If you do not consider your workflow to be ready for real use but still wish to track the version
number you may version it as, say, "0.5.0". The major version number of 0 indicates a development
release. Use the major and minro releases as above, but the distinction between these is understood
to be less meaningful in the context of development releases.

### Variants on sematic versioning

For some well-known software, you will see the versions like 24.12.0 - a variation where
major and minor version numbers encode the year and month of release. The patch number is still
just a sequential number. This works will in projects like Ubuntu which have periodic releases,
but for workflow updates you are best to avoid it.



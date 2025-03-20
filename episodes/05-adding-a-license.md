---
title: Adding a license
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand the difference between license and copyright
- Understand what might inform, or limit, our choice of license
- Add a license to our own code

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What OSI licenses are available?
- How do I record ownership and authorship of my code?

::::::::::::::::::::::::::::::::::::::::::::::::::

## The short answer: add the MIT license

The [MIT license](https://opensource.org/license/mit) is a simple permissive license that...

* asserts your right to be identified as the creator/owner of the code
* permits others to re-use, modify and share it
* disclaims any liability for bugs or errors in the code

The GitHub web interface has an option to add the license file, but we'll just do it manually.

In your workflow source directory, download the MIT license text template and add your own name
at the top, along with the date.

```bash
$ wget https://raw.githubusercontent.com/licenses/license-templates/refs/heads/master/templates/mit.txt
$ mv mit.txt LICENSE.txt
$ nano LICENSE.txt
```

You may now *commit* and *push* the change to GitHub. The `-m` option to `git commit` allows you
to set a short commit message without going through the text editor. We'll use this in the examples
from now on as it is easier to write things out this way in the code snippets.

```bash
$ git add LICENSE.txt
$ git commit -m 'Add a license'
$ git push
```

## Licence, copyright, and other stipulations

### Copyright ownership

License and copyright are two different things. Copyright means identifying the legal owner of the
code and related files. Often this is obvious, but you need to get it right.

* If you wrote the code as part of your paid work, your employer is generally the owner.
* Code written in your own time, outside of work projects, belongs to you.
* Copyright ownership can be assigned to another owner by formal agreement.
* Ownership can be disclaimed entirely, but this is generally a bad idea as noted below.
* If you incorporated pieces of code from other work, the owners of that work share ownership.
* Code that you produce with AI tools is not owned by you (but to some degree this is an open
  legal question).

Questions of ownership have led to costly and acrimonious legal actions, including ones between
academic institutions and between employees and employers. Where the ownership of code is not
specified, many organisations and projects will simply refuse to touch it, so even if you do not
intend to restrict the copying of your code, you need to explicitly identify who had the right to
make that stipulation.

::::::::::: callout

## Copyright on the sample workflow

Given the above, putting your own name on the `LICENSE.txt` file in your Git repository is
looking problematic, since the Snakefile was created by the authors of this course. However, the
[terms of use](LICENSE.html) for the course material explicitly say that you can do that.

:::::::

### Licensing the code

Having identified the owner of the code, you need to say what others may legally do with it. In
the case of software products that you plan to sell or otherwise monetize you would want to make
careful and explicit restrictions, but in this context we assume that you want your workflow to
be shared and re-used by whoever may need it. There are many [OSI Approved Licenses](
https://opensource.org/licenses) but most are for niche applications, or concerned with source code
versus compiled code, or concerned with patents. These probably do not apply to your workflow. As
stated above, you should use the MIT license unless you have a good reason not to.

If you have incorporated any code that was shared with you under a different license then that
license may stipulate (the [GPL](https://opensource.org/license/gpl-3-0) is a notable example)
that your new derived work *must* use that same license. However, this does not apply to tools
that are *called by* your workflow, only code you are directly *copying into* your workflow.

If you are distributing tutorials, presentations or other substantial documentation along with the
code then you likely want to consider a separate license for these. The OSI Licenses are all geared
to the sharing of code, whereas the [Creative Commons](
https://creativecommons.org/share-your-work/cclicenses/) licenses are designed for other creative
works. The equivalent of the MIT license here is the least restrictive *CC-BY*, which is used for
these lessons and all the Carpentries materials, but you may prefer to opt for an alternative.

### Other stipulations

You would probably like your workflow to be cited by anyone who uses it for their research. You
may like the idea that users should send you a postcard. Maybe you are concerned that your
workflow should not be used for nefarious purposes, like developing bio-weapons. In the past,
extra stipulations like this were added to many licenses and it causes a big headache, less
for the end users but more for the projects and sites that archive and redistribute code. For this
reason, we give you this edict:

**Choose an established Open Source license, and do not edit the legal text in the LICENSE file**

Having said this, you can add any further requests that you like. Just do not give them legal
weight unless you or your employer would be prepared to take legal action over them. You could ask
your users to send you a postcard. You may vent your anger at those who use science for the
disbenefit of humanity, though they are unlikely to care.

We'll assume here that, like most researchers, you would like your work to be cited.

## Requesting a citation

While it may not be a legal requirement, the expectation of citation is enforced by academic
publishers and the norms of the scientific community. Different publications use different
citation formats, and there are also several open citation formats. Fortunately in this case there
is one *best* way, using [CFF](https://citation-file-format.github.io/). This standard
is promoted by GitHub, but it's widely supported and can be auto-converted to whatever is
actually needed.

Save the following text to a file `CITATION.cff`. You can edit the text if you like (the underlying
format is YAML, as used for the workflow configuration files), but the
[official on-line CFF editor](https://citation-file-format.github.io/cff-initializer-javascript/#/)
provides a better way to create and amend these files.

```
# This CITATION.cff file was generated with cffinit.
# Visit https://bit.ly/cffinit to generate yours today!

cff-version: 1.2.0
title: Demo assembly workflow
message: >-
  If you use this software, please cite it using the
  metadata from this file.
type: software
authors:
  - given-names: Timothy
    family-names: Booth
    affiliation: University of Edinburgh
  - name: BioFAIR
    website: 'https://biofair.uk'
url: 'https://tbooth.github.io/snakemake-publishing'
license: MIT
```

Note there is some redundancy with `LICENSE.txt`, in that the license name and the authors are
specified, but the authors for the purpose of citation are not necessarily the legal copyright
owners of the code. Also, the `LICENSE.txt` statement is generally understood to hold legal weight
while this `CITATION.cff` file is informative and may be updated as needed. If you publish the
workflow in a journal, or mint a new DOI for it, then add the info here.

Add the new `CITATION.cff` file to Git.

```bash
$ git add CITATION.cff
$ git commit -m 'add citation CFF'
$ git push
```

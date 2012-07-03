Sign the OCLA
=============

This page is the step-by-step guide to signing the Obvious
Contributors License Agreement. It's easy and pretty painless!

1. First and foremost, read [the current version of the
   OCLA](ocla-1.0.md). It is written to be as close to plain
   English as possible.

2. Make an account on [GitHub](https://github.com/) if you don't already
   have one.

3. File a pull request on this project (the Obvious Open Source
   Umbrella Project), as outlined below.

4. Wait for an Obvious team member to merge your pull request.

5. Contribute!

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

Filing the Pull Request
-----------------------

If you don't yet know how to file a pull request, read [GitHub's
document about it](https://help.github.com/articles/using-pull-requests).

Make your pull request be the addition of a single file to the
[contributors](contributors) directory of this project. Name the file
with the same name as your GitHub userid, with `.md` appended to the
end. For example, for the user `danfuzz`, the full path to the file
would be `contributors/danfuzz.md`.

Put the following in the file:

```
[date]

I hereby agree to the terms of the Obvious Contributors License
Agreement, version 1.0, with MD5 checksum
fcd4077c7cac68dce23ba424d7651125.

I furthermore declare that I am free and able to make this agreement
and sign this declaration.

Signed,

[your name]
https://github.com/[your github userid]
```

Replace the bracketed text as follows:

* `[date]` with today's date, in the unambiguous numeric form `YYYY-MM-DD`.
* `[your name]` with your name.
* `[your github userid]` with your GitHub userid.

That's it!

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

Rationale
---------

In an older time, this sort of agreement might have been collected
in paper form. You might have been asked to physically sign a piece
of paper and then send it into an organization (perhaps physically
or as a fax), which would in turn keep it in a real filing cabinet.

We no longer live in that time.

The real point of all this is to have a document of someone's statement,
such that they can't credibly claim later that they didn't make that
statement. That is, what we want is a *non-repudiatable* record of
someone's statement. This provides the company and the open source
project protection against malicious would-be contributors.

One wonderful thing about the world we live in today is that we can
achieve this non-reputiatability without having to have a physical
document. In the case of the OCLA, we bootstrap this ability off of
the infrastructure provided by GitHub. In particular, we treat GitHub
as a neutral third party to witness the transactions between a
would-be contributor and The Obvious Corporation. GitHub ends up
acting sort of like a notary, in that its records of the actions
&mdash; such as in particular the pull requests &mdash; of people
using it can be taken as authoritative and unbiased.

So, when a contributor forks this project, commits a change indicating
agreement to the OCLA, and files a pull request back with this project,
GitHub knows that all that happened, knows when it happened, and knows
the identity of the entity taking all that action. Should there ever
be a dispute about whether any of that took place, it's not just
he-said she-said, because GitHub has unbiased information which it can
use to shed light on the situation.

The upshot is that filing a pull request containing a statement of
agreement to the OCLA is close enough to having submitted a physical
document saying the same thing to be a workable solution to the
problem. And this scheme fits in naturally with how actual
contributions get made, what we do here ends up being the
least-intrusive way of getting the assurance we &mdash; and the open
source community in general &mdash; need to operate in safety.

OCLA Rationale
==============

Why do this at all?
-------------------

The point of the OCLA signing process is to have a credible record of
a developer stating that they really intend to contribute to an open
source project. Technically speaking, the thing we are aiming for is a
*non-repudiable* statement from a contributor, that is, a statement
that would be a blatant falsehood to later deny.

This is an important step in assuring that an open source project
&mdash; any open source project &mdash; truly is open source. More
specifically, it helps guard against bad actors who contribute to a
project only *apparently* in good faith, and then later make trouble
by claiming they weren't really contributing under the project's open
source license. For some projects "trouble" has historically come, for
example, in the form of patent lawsuits.

The Obvious Corporation wants to do our part to guard against this
potential trouble, and we believe so should you. We aren't innovating
here by asking you to sign an agreement, but we *are* trying to
innovate by making the agreement and process nearly-transparent,
natural to do for folks already active in open source, and (we hope)
extremely understandable.

For comparison, here are a few other open source projects and
organizations that use contributor license agreements or have similar
processes:

* [ANTLR](http://www.antlr.org/):
  <http://www.antlr.org/doc/ANTLR-contributor-agreement.pdf>
* [Apache](http://www.apache.org/): <http://www.apache.org/licenses/icla.txt>
* [GNU](http://www.gnu.org/):
  <http://www.gnu.org/prep/maintain/html_node/Copyright-Papers.html>
* [Google](http://code.google.com/) (Android, Chrome / ChromeOS, and more):
  <http://code.google.com/legal/individual-cla-v1.0.html>
* [Linux kernel](http://kernel.org/):
  <http://elinux.org/Developer_Certificate_Of_Origin>
* [Node](http://nodejs.org/): <http://nodejs.org/cla.html>
* [10Gen](http://www.10gen.com/) (MongoDB):
  <http://www.10gen.com/contributor>

In addition, there is unsurprisingly a
[Wikipedia page about CLAs](http://en.wikipedia.org/wiki/Contributor_License_Agreement). It's brief but has some good information and useful links.


Why do it this way?
-------------------

In an older time, this sort of agreement might have been collected in
paper form. You might have been asked to sign a piece of paper and
then send it into an organization (perhaps physically or as a fax),
which would in turn keep it in a real filing cabinet.

We no longer live in that time.

One wonderful thing about the world we live in today is that we can
achieve the necessary non-reputiability without having to have a
physical document. In the case of the OCLA, we bootstrap this ability
off of the infrastructure provided by GitHub: More specifically, we
treat GitHub as a neutral third party to witness the transactions
between a would-be contributor and The Obvious Corporation. GitHub
ends up acting sort of like a notary, in that its records of the
actions &mdash; such as in particular the pull requests &mdash; of
people using it can be taken as authoritative and unbiased.

So, when a contributor forks this project, commits a change indicating
agreement to the OCLA, and files a pull request back with this project,
GitHub knows that all that happened, knows when it happened, and knows
the identity of the entity taking all that action. Should there ever
be a dispute about whether any of that took place, it's not just
he-said she-said, because GitHub has unbiased information which it can
use to shed light on the situation.

In addition to what's in the pull request, we need to have a little
bit of extra information, which can help verify that you are who you
say you are, should the need arise. In particular, we need to have
some information that can link you to your contributions, even if you
later delete your GitHub account. Since people rightly desire privacy
about their addresses and phone numbers, we don't ask for this
information to be made public in the pull request, instead going for a
traditional email. We promise never to use this information for
any purpose other than resolving authorship disputes.

The upshot is that filing a pull request containing a statement of
agreement to the OCLA, along with the supplementary email, is close
enough to having submitted a signed physical document saying the same
things. That is, this tactic is a workable solution to the problem. And
since fits in naturally with how actual contributions get made, what
we do here ends up being the least-intrusive way of getting the
assurance that we all need to operate in safety.

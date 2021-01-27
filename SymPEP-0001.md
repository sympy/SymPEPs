# SymPEP 1 --- SymPEP Purpose and Process

**Author** Aaron Meurer
**Status** Draft
**Type** Process
**Created** 2021-01-27
**Resolution**

## Abstract

SymPEPs are a formal process whereby important changes are proposed and
discussed. Corresponding to each SymPEP is a document that outlines the
proposed change(s), the discussion around those changes, and motivation for
that change. This document outlines the details of the SymPEP process and
corresponding documents.

## What is a SymPEP?

SymPEP stands for "SymPy Enhancement Proposal". SymPEPs represent a formal
process whereby important changes are proposed and discussed in the SymPy
community. Corresponding to each SymPEP is a SymPEP document which outlines
the proposal and corresponding discussion. SymPEPs are based on
[PEP](https://www.python.org/dev/peps/) process used by the Python language
community to propose changes to the Python language. It also takes motivations
from other similar processes in communities adjacent to SymPy, such as the
[NEP](https://numpy.org/neps/nep-0000.html) process for NumPy and the
[MEP](https://matplotlib.org/devel/MEP/index.html) process for Matplotlib.
However, the SymPEP process differs from these processes in many ways, so
those who may already be familiar with similar processes from other
communities should read this document to understand how it works for SymPEPs.

In particular, SymPEPs are significantly less formal than Python PEPs.

## Types

There are three kinds of SymPEPs:

A **Standards Track** SymPEP describes a new feature or important change for
SymPy.

An **Informational** SymPEP describes a SymPy design issue, or provides general
guidelines or information to the Python community, but does not propose a new
feature. Informational SymPEPs do not necessarily represent a SymPy community
consensus or recommendation, so users and implementers are free to ignore
Informational SymPEPs or follow their advice.

A **Process** SymPEP describes a process surrounding SymPy, or proposes a
change to (or an event in) a process. Process SymPEPs are like Standards Track
SymPEPs but apply to areas other than the SymPy library itself. They may
propose an implementation, but not to SymPy’s codebase; they require community
consensus. Examples include procedures, guidelines, changes to the
decision-making process, and changes to the tools or environment used in SymPy
development. Any meta-SymPEP is also considered a Process SymPEP.

## Purpose

The SymPEP process and corresponding documents serve several purposes:

- To decide, as a community, whether a proposed change should be made.
- To decide on the process by which a change will be implemented, for example,
  whether the change should be implemented in several stages, whether
  intermediate releases are required for things like deprecations, and so on.
- To document the above discussions and decisions for future reference.
- To document the motivations for a change from the perspective of end users
  who may be affected by it. Here "end users" means both users of the SymPy
  library, as well as developers working on parts of SymPy itself which may be
  affected by the change.

Importantly, **a SymPEP is not documentation** for the proposed change. End
user documentation should be included with the implementation of the feature
in the corresponding SymPy documentation. This also means that other
documentation should not cross-reference a SymPEP as if it were documentation
for a feature. Even technical discussion of a feature should be documented
separately from a SymPEP. The reason is that SymPEPs will necessarily include
details that are irrelevant to the final implementation, such as discussions
of alternate implementations which were rejected and discussions of
implementation of the change.

## SymPEP Workflow

Discussions around a proposed change may begin informally on the mailing list
or SymPy issue tracker. However, once it is decided that the formal SymPEP
process is desired for a change, the discussion should move to the
[SymPEP repository](https://github.com/sympy/SymPEPs) on GitHub.

Every SymPEP must have a champion. This is a person who is responsible for
writing the SymPEP, leads the discussion around it, and tries to create
consensus around it.

The author of a SymPEP should fork the repository and create a pull request
with a new SymPEP document based on the [SymPEP template](SymPEP-template).
The SymPEP document may be named `SymPEP-XXXX.md` until a number is assigned.
One of the core SymPy developers will then assign a number to the SymPEP, in
which case `XXXX` should be replaced with the number with leading 0s. The
person who assigns the number should also update the
[README](https://github.com/sympy/SymPEPs/blob/main/README.md) of the main
[SymPEP repository](https://github.com/sympy/SymPEPs) to list that number.
Numbers should be assigned to SymPEPs as soon as it is determined that they
are a legitimate proposal. If a SymPEP ends up being rejected or postponed, it
keeps its number, as rejection or postponement status is still a discussion
that should be documented in the SymPEP. SymPEP numbers should generally be
assigned in increasing numeric order.

Discussion on the SymPEP should then continue on the SymPEP pull request.
Discussions may also take place in other places, such as [GitHub
discussions](https://github.com/sympy/SymPEPs/discussions) or the [mailing
list](http://groups.google.com/group/sympy). All discussions should be
cross-referenced in the "Discussions" section of the SymPEP document.

For each SymPEP, the community should decide whether a draft implementation is
needed before acceptance or not. For instance, if a SymPEP concerns the
details of how a feature is implemented, it should be accepted before that
happens. On the other hand, the community may decide that it cannot come to a
consensus about a SymPEP until a draft implementation is proposed.

Once the community reaches a consensus about a SymPEP, the status of a SymPEP
should be updated (see below). This consensus may be to accept or to reject
the SymPEP, or to defer it. Here the "community" refers to the broader
community that has a stake in the SymPEP, not just the core SymPy developers.
The purpose of the SymPEP process is not to create a cabal of decision makers,
but rather to enhance the involvement of the broader SymPy community in the
decision making process.

### Status

The **status** section at the top of the SymPEP document (see the
[template](SymPEP-template)) should be updated according to the current status
of the SymPEP.

All SymPEPs should be created with the **Draft** status. **Draft** status
SymPEPs generally live in a pull request.

Eventually, after discussion, there may be a consensus that the SymPEP should
be accepted–see the next section for details. At this point the status
becomes **Accepted**.

Once a SymPEP has been **Accepted**, the reference implementation must be
completed. When the reference implementation is complete and incorporated into
the main source code repository, the status will be changed to **Final**.

A SymPEP can also be assigned status **Deferred**. The SymPEP author or a core
developer can assign the SymPEP this status when no progress is being made on
the SymPEP.

A SymPEP can also be **Rejected**. Perhaps after all is said and done it was
not a good idea. It is still important to have a record of this fact. The
**Withdrawn** status is similar—it means that the SymPEP author themselves has
decided that the SymPEP is actually a bad idea, or has accepted that a
competing proposal is a better alternative.

SymPEPs can also be **Superseded** by a different SymPEP, rendering the
original obsolete.

Process SymPEPs may also have a status of **Active** if they are never meant
to be completed, e.g. SymPEP 1 (this SymPEP).

### Merging the SymPEP Document Pull Request

Whenever a SymPEP moves from the **Draft** status to one of the other above
statuses, the header should be updated, and the corresponding pull request
should be merged. This way the document lives inside of the SymPEP repository
proper. SymPEPs that are merged are not set in stone, and may be updated by
future pull requests (although SymPEPs that are **Accepted** should generally
not be significantly modified once they have reached that status). The purpose
of merging is simply to make the SymPEP visible in the repository, even if it
isn't yet accepted or rejected. SymPEP discussions that have stalled should
also be merged, so that the SymPEP becomes visible in the SymPEP repository
proper—again, discussion may be picked up again with a new pull request.

## Accepting a SymPEP

Once a SymPEP is Accepted by consensus of all interested contributors, an
email should be sent to the [SymPy mailing
list](http://groups.google.com/group/sympy) with a subject like:

    Proposal to accept SymPEP #<number>: <title>

In the body of your email, you should:

- link to the latest version of the SymPEP,

- briefly describe any major points of contention and how they were resolved,

- include a sentence like: “If there are no substantive objections within 7
  days from this email, then the SymPEP will be accepted; see SymPEP 1 for
  more details.”

After you send the email, add the the email thread to the Discussion section
of the SymPEP, so that people can find it later.

Generally the SymPEP author will be the one to send this email, but anyone can
do it – the important thing is to make sure that everyone knows when a SymPEP
is on the verge of acceptance, and give them a final chance to respond. If
there’s some special reason to extend this final comment period beyond 7 days,
then that’s fine, just say so in the email. It shouldn’t be less than 7 days,
because sometimes people are traveling or similar and need some time to
respond.

In general, the goal is to make sure that the community has consensus, not
provide a rigid policy for people to try to game. When in doubt, err on the
side of asking for more feedback and looking for opportunities to compromise.

If the final comment period passes without any substantive objections, then
the SymPEP can officially be marked **Accepted**. A followup email should then
be sent notifying the list. Update the SymPEP by setting its **Status** to
**Accepted**, and its **Resolution** header to a link to your followup email.
The SymPEP pull request should be merged at this time, if it hasn't been
already, so that it is visible in the SymPEP repository proper.

If there are substantive objections, then the SymPEP remains in Draft state,
discussion continues as normal, and it can be proposed for acceptance again
later once the objections are resolved.

## Discussion

## Copyright

This document has been placed in the public domain.

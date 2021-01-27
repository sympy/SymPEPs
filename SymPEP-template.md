SymPEP X — Template and Instructions
====================================

**Author** list of authors' real names and optionally, email addresses
**Status** Draft | Active | Accepted | Deferred | Rejected | Withdrawn | Final | Superseded
**Type** Standards Track | Informational | Process
**Created** date created on, in yyyy-mm-dd format
**Resolution** url to discussion (required for Accepted | Rejected | Withdrawn)

## Abstract

The abstract should be a short description of what the SymPEP will achieve.

Note that the — in the title is an em dash, not -.

## Motivation and Scope

This section describes the need for the proposed change. It should describe
the existing problem, who it affects, what it is trying to solve, and why.
This section should explicitly address the scope of and key requirements for
the proposed change.

## Usage and Impact

This section describes how users of SymPy will use features described in this
SymPEP. It should be comprised mainly of code examples that wouldn't be possible
without acceptance and implementation of this SymPEP, as well as the impact the
proposed changes would have on the ecosystem. This section should be written
from the perspective of the users of SymPy, and the benefits it will provide
them; and as such, it should include implementation details only if
necessary to explain the functionality.

Note that this section should *not* be treated as documentation of the
proposed functionality. Documentation should be included in the implementation
itself. The purpose of this section is to motivate why the change should be
made in the first place. See [SymPEP 1](SymPEP-0001) for more information.

## Backwards compatibility

This section describes the ways in which the SymPEP breaks backward compatibility.

The mailing list post will contain the SymPEP up to and including this section.
Its purpose is to provide a high-level summary to users who are not interested
in detailed technical discussion, but may have opinions around, e.g., usage and
impact.

## Detailed description

This section should provide a detailed description of the proposed change.
It should include examples of how the new functionality would be used,
intended use-cases and pseudocode illustrating its use.

## Related Work

This section should list relevant and/or similar technologies, possibly in other
libraries. It does not need to be comprehensive, just list the major examples of
prior and relevant art.

## Implementation

This section lists the major steps required to implement the SymPEP.  Where
possible, it should be noted where one step is dependent on another, and which
steps may be optionally omitted.  Where it makes sense, each step should
include a link to related pull requests as the implementation progresses.

Any pull requests or development branches containing work on this SymPEP should
be linked to from here.  (A SymPEP does not need to be implemented in a single
pull request if it makes sense to implement it in discrete phases).

## Alternatives

If there were any alternative solutions to solving the same problem, they should
be discussed here, along with a justification for the chosen approach.

## Discussion

This section may just be a bullet list including links to any discussions
regarding the SymPEP:

- This includes links to mailing list threads or relevant GitHub issues.

## References

Any references should be listed here. Note this does not include links to
discussions about the SymPEP, which are listed in the previous section. For
online resources, freely accessible and stable online resources such as
Wikipedia, Wolfram MathWorld, and the NIST Digital Library of Mathematical
Functions (DLMF), which are unlikely to suffer from hyperlink rot, should be
preferred. Non-online references such as textbook or literature references may
also be used here.

## Copyright

Each SymPEP must be explicitly labeled as placed in the public domain, using
the below sentence (the below sentence also applies to this template).

This document has been placed in the public domain.

## Extra Notes About SymPEPs

NOTE: This section is not part of the template. It should not be included in
any SymPEPs. It is a list of notes of things that apply to all parts of the
above template.

Some extra notes about writing a SymPEP:

- All SymPEPs live in the [SymPEPs
  repository](https://github.com/sympy/SymPEPs/) on GitHub. All proposed
  SymPEPs should be made as pull requests to that repo (see [SymPEP
  1](SymPEP-0001)).
- The number corresponding to a SymPEP will be assigned when it is first
  proposed to the community. You may use `XXXX` as a placeholder number until
  this is done.
- The file for a SymPEPs should be named `SymPEP-XXXX.md` where `XXXX` is a
  four digit number, preceded with leading 0s. It should be placed at the root
  of the [SymPEPs repository](https://github.com/sympy/SymPEPs/).
- All SymPEP documents should be written in Markdown, following this template.
  The Markdown should be compatible with GitHub Flavored Markdown, so that the
  SymPEPs are viewable on GitHub. This means that Markdown features
  not supported by GitHub, such as footnotes, should be avoided.
  <!-- XXX: Perhaps we should abandon this and only require them to be
  readable in some rendered format. That would allow us to use MathJAX, which
  could be useful. -->
- Any references to other SymPEPs should be written as internal links, e.g.,
  [SymPEP 1](SymPEP-0001).
- Images or diagrams may be included in the SymPEP if they improve the
  understanding of the discussion. Code examples should always be included as
  plain text. Any image or diagram should be accompanied by corresponding text
  describing what is in it. Images should be stored in an `images` directory
  at the root of the SymPEPs repository. Images should be titled like
  `XXXX-image-name.svg` where `XXXX` is the four digit number of the SymPEP
  and `image-name` is the name of the image. For example,
  `images/0001-process-diagram.svg`. Vector images are preferred when
  possible. Note that images should only be included when they significantly
  improve the discussion in the SymPEP. The vast majority of SymPEPs should
  not include images.

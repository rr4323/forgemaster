# Product Enhancement Proposal Process

## Table of Contents

<!-- toc -->
- [Summary](#summary)
- [Motivation](#motivation)
- [Stewardship](#stewardship)
- [Reference-Level Explanation](#reference-level-explanation)
  - [What Type of Work Should Be Tracked by a PEP](#what-type-of-work-should-be-tracked-by-a-pep)
  - [PEP Template](#pep-template)
  - [PEP Metadata](#pep-metadata)
  - [PEP Workflow](#pep-workflow)
  - [Git and GitHub Implementation](#git-and-github-implementation)
  - [PEP Editor Role](#pep-editor-role)
  - [Important Metrics](#important-metrics)
  - [Prior Art](#prior-art)
- [Drawbacks](#drawbacks)
- [Alternatives](#alternatives)
  - [GitHub Issues vs. PEPs](#github-issues-vs-peps)
- [Unresolved Questions](#unresolved-questions)
<!-- /toc -->

## Summary

A standardized development process for this project is proposed, in order to:

- provide a common structure for proposing changes to this project
- ensure that the motivation for a change is clear
- allow for the enumeration of stability milestones and stability graduation
  criteria
- persist project information in a Version Control System (VCS) for future
  contributor
- support the creation of _high-value, user-facing_ information such as:
  - an overall project development roadmap
  - motivation for impactful user-facing changes
- reserve GitHub issues for tracking work in flight, instead of creating "umbrella"
  issues
- ensure community participants can successfully drive changes to
  completion across one or more releases while stakeholders are adequately
  represented throughout the process

This process is supported by a unit of work called a Project Enhancement Proposal, or PEP.
A PEP attempts to combine aspects of
This proposal contains:-
- detailed explanations of how the feature should be implemented and its history
- a feature, and effort-tracking document
- a product requirements document
- a design document

into one file, which is created incrementally in collaboration with one or more
Special Interest Groups (SIGs).

## Motivation

For cross-project SIGs such as SIG Release, an abstraction beyond a
single GitHub Issue or pull request seems to be required in order to understand
and communicate upcoming changes to this Project. In a blog post describing the
[road to Go 2][], Russ Cox explains:

> that it is difficult but essential to describe the significance of a problem
> in a way that someone working in a different environment can understand

It is vital for the project to be able to track the chain of custody for a proposed
enhancement from conception through implementation.

Without a standardized mechanism for describing important enhancements, our
talented technical writers and product managers struggle to weave a coherent
narrative explaining why a particular release is important. Additionally, adopters
of critical infrastructure such as this Project need a forward-looking roadmap in
order to plan their adoption strategies.

The purpose of the PEP process is to reduce the amount of "tribal knowledge" in
our community. By moving decisions from a smattering of mailing lists, video
calls and hallway conversations into a well tracked artifact, this process aims
to enhance communication and discoverability.

A PEP is broken into sections which can be merged into source control
incrementally in order to support an iterative development process. An important
goal of the PEP process is ensuring that the process for submitting the content
contained in both clear and efficient. The PEP process
is intended to create high-quality, uniform design and implementation documents
for SIGs to deliberate.

[road to Go 2]: https://blog.golang.org/toward-go2
[design proposals]: https://github.com/kubernetes/community/tree/master/contributors/design-proposals

## Stewardship
The following [DACI][] model identifies the responsible parties for PEPs.

**Workstream** | **Driver** | **Approver** | **Contributor** | **Informed**
--- | --- | --- | --- | ---
| PEP Process Stewardship |  |  |  SIG Leadership | Community |
| Enhancement Delivery | Enhancement Owner |  SIG leadership (SIG Chairs + TLs) | Enhancement Implementer(s) (may overlap with Driver) | Community |

[DACI]: https://project-management.com/daci-model/


## Reference-Level Explanation

### What Type of Work Should Be Tracked by a PEP

The definition of what constitutes an "enhancement" is a foundational concern
for the this project. Roughly any user or operator facing
enhancement should follow the PEP process. If an enhancement would be described
in either written or verbal communication to anyone besides the PEP author or
developer, then consider creating a PEP.

Similarly, any technical effort (refactoring, major architectural change) that
will impact a large section of the development community should also be
communicated widely. The PEP process is suited for this even if it will have
zero impact on the typical user or operator.

Enhancements that have major impacts on multiple SIGs should use the PEP process.
A single SIG will own the PEP, but it is expected that the set of approvers will span the impacted SIGs.
The PEP process is the way that SIGs can negotiate and communicate changes that cross boundaries.

PEPs will also be used to drive large changes that will cut across all parts of the project.
These PEPs will be owned by SIG-architecture and should be seen as a way to communicate the most fundamental aspects of what this Project is.

### PEP Template

**The template for a PEP is precisely defined [here](/NNNN-pep-template).**

### PEP Metadata

There is a place in each PEP for a YAML document that has standard metadata.
This will be used to support tooling around filtering and display.  It is also
critical to clearly communicate the status of a PEP.

<b>
While this defines the metadata schema for now, these things tend to evolve.
The PEP template is the authoritative definition of things like the metadata
schema.
</b>

Metadata items:
* **title** Required
  * The title of the PEP in plain language.  The title will also be used in the
    PEP filename.  See the template for instructions and details.
* **status** Required
  * The current state of the PEP.
  * Must be one of `provisional`, `implementable`, `implemented`, `deferred`, `rejected`, `withdrawn`, or `replaced`.
* **authors** Required
  * A list of authors of the PEP.
    This is simply the GitHub ID.
    In the future we may enhance this to include other types of identification.
* **owning-sig** Required
  * The SIG most closely associated with this PEP. If code or
    other artifacts will result from this PEP, then it is expected that
    this SIG will take responsibility for the bulk of those artifacts.
  * Sigs are listed as `sig-abc-def`, where the name matches the
    directory entry in the `kubernetes/community` repo.
* **participating-sigs** Optional
  * A list of SIGs that are involved or impacted by this PEP.
  * A special value of `kubernetes-wide` will indicate that this PEP has impact
    across the entire project.
* **reviewers** Required
  * Reviewer(s) chosen after triage, according to the proposal process.
  * If not yet chosen, replace with `TBD`.
  * Same name/contact scheme as `authors`.
  * Reviewers should be a distinct set from authors.
* **approvers** Required
  * Approver(s) chosen after triage, according to the proposal process.
  * Approver(s) are drawn from the impacted SIGs.
    It is up to the individual SIGs to determine how they pick approvers for PEPs impacting them.
    The approvers are speaking for the SIG in the process of approving this PEP.
    The SIGs in question can modify this list as necessary.
  * The approvers are the individuals who decide when to move this PEP to the `implementable` state.
  * Approvers should be a distinct set from authors.
  * If not yet chosen, replace with `TBD`.
  * Same name/contact scheme as `authors`.
* **editor** Required
  * Someone to keep things moving forward.
  * If not yet chosen, replace with `TBD`.
  * Same name/contact scheme as `authors`.
* **creation-date** Required
  * The date that the PEP was first submitted in a PR.
  * In the form `yyyy-mm-dd`.
  * While this info will also be in source control, it is helpful to have the set of PEP files stand on their own.
* **last-updated** Optional
  * The date that the PEP was last changed significantly.
  * In the form `yyyy-mm-dd`.
* **see-also** Optional
  * A list of other PEPs that are relevant to this PEP.
  * In the form `PEP-123`.
* **replaces** Optional
  * A list of PEPs that this PEP replaces.  Those PEPs should list this PEP in
    their `superseded-by`.
  * In the form `PEP-123`.
* **superseded-by**
  * A list of PEPs that supersede this PEP. Use of this should be paired with
    this PEP moving into the `Replaced` status.
  * In the form `PEP-123`.


### PEP Workflow

A PEP has the following states:

- `provisional`: The PEP has been proposed and is actively being defined.
  This is the starting state while the PEP is being fleshed out and actively defined and discussed.
  The owning SIG has accepted that this work must be done.
- `implementable`: The approvers have approved this PEP for implementation.
- `implemented`: The PEP has been implemented and is no longer actively changed.
- `deferred`: The PEP is proposed but not actively being worked on.
- `rejected`: The approvers and authors have decided that this PEP is not moving forward.
  The PEP is kept around as a historical document.
- `withdrawn`: The authors have withdrawn the PEP.
- `replaced`: The PEP has been replaced by a new PEP.
  The `superseded-by` metadata value should point to the new PEP.

### Git and GitHub Implementation

PEPs are checked into the Enhancements repo under the `/design-documents/specifications/` directory.
PEPs in SIG specific subdirectories have limited impact outside of the SIG and can leverage SIG-specific OWNERS files.

New PEPs can be checked in with a file name in the form of `draft-YYYYMMDD-my-title.md`.
As significant work is done on the PEP, the authors can assign a PEP number.
No other changes should be put in that PR so that it can be approved quickly and minimize merge conflicts.
The PEP number can also be done as part of the initial submission if the PR is likely to be uncontested and merged quickly.

### PEP Editor Role

Taking a cue from the [Python PEP process][], we define the role of a PEP editor.
The job of an PEP editor is likely very similar to the [PEP editor responsibilities][] and will hopefully provide another opportunity for people who do not write code daily to contribute to this Project.

In keeping with the PEP editors, who:

> Read the PEP to check if it is ready: sound and complete. The ideas must make
> technical sense, even if they don't seem likely to be accepted.
> The title should accurately describe the content.
> Edit the PEP for language (spelling, grammar, sentence structure, etc.), markup
> (for reST PEPs), code style (examples should match PEP 8 & 7)

PEP editors should generally not pass judgement on a PEP beyond editorial corrections.
PEP editors can also help inform authors about the process and otherwise help things move smoothly.

[Python PEP process]: https://www.python.org/dev/peps/pep-0001/
[PEP editor responsibilities]: https://www.python.org/dev/peps/pep-0001/#pep-editor-responsibilities-workflow

### Important Metrics

It is proposed that the primary metrics that would signal the success or
failure of the PEP process are:

- how many "enhancements" are tracked with a PEP
- distribution of time a PEP spends in each state
- PEP rejection rate
- PRs referencing a PEP merged per week
- number of issues open which reference a PEP
- number of contributors who authored a PEP
- number of contributors who authored a PEP for the first time
- number of orphaned PEPs
- number of retired PEPs
- number of superseded PEPs

### Prior Art

This process is solely following the [kubernetes Design proposal][] with some enhancement.
[kubernetes Design proposal]: 
https://github.com/kubernetes/enhancements/blob/master/keps/sig-architecture/0000-kep-process/README.md

## Drawbacks

Any additional process has the potential to engender resentment within the
community. There is also a risk that the PEP process as designed will not
sufficiently address the scaling challenges we face today. PR review bandwidth is
already at a premium, and we may find that the PEP process introduces an unreasonable
bottleneck on our development velocity.

It certainly can be argued that the lack of a dedicated issue/defect tracker
beyond GitHub issues contributes to our challenges in managing a project as large
as this Project. However, given that other large organizations, including GitHub
itself, make effective use of GitHub issues, perhaps the argument is overblown.

The centrality of Git and GitHub within the PEP process also may place too high
a barrier to potential contributors. However, given that both Git and GitHub are
required to contribute code changes to this Project today, perhaps it would be reasonable
to invest in providing support to those unfamiliar with this tooling.

Expanding the proposal template beyond the single-sentence description currently
required in the [features issue template][] may be a heavy burden for non-native
English speakers. Here, the role of the PEP editor, combined with kindness and
empathy, will be crucial to making the process successful.

[features issue template]: https://git.k8s.io/features/ISSUE_TEMPLATE.md

## Alternatives

This PEP process is related to:
- the generation of an [architectural roadmap][]
- the fact that the [what constitutes a feature][] is still undefined
- [issue management][]
- the difference between an [accepted design and a proposal][]
- [the organization of design proposals][]

this proposal attempts to place these concerns within a general framework.

[architectural roadmap]: https://github.com/kubernetes/community/issues/952
[what constitutes a feature]: https://github.com/kubernetes/community/issues/531
[issue management]: https://github.com/kubernetes/community/issues/580
[accepted design and a proposal]: https://github.com/kubernetes/community/issues/914
[the organization of design proposals]: https://github.com/kubernetes/community/issues/918

### GitHub Issues vs. PEPs

The use of GitHub issues when proposing changes does not provide SIGs good
facilities for signaling approval or rejection of a proposed change to this Project,
because anyone can open a GitHub issue at any time. Additionally, managing a proposed
change across multiple releases is somewhat cumbersome as labels and milestones
need to be updated for every release that a change spans. These long-lived GitHub
issues lead to an ever-increasing number of issues open against
`product/features`, which itself has become a management problem.

In addition to the challenge of managing issues over time, searching for text
within an issue can be challenging. The flat hierarchy of issues can also make
navigation and categorization tricky. Not all community members will
be uncomfortable using Git directly, but it is imperative for our community to educate people on a standard set of tools so they can take their
experience to other projects they may decide to work on in the future. While
git is a fantastic version control system (VCS), it is neither a project management
tool nor a cogent way of managing an architectural catalog or backlog. This
proposal is limited to motivating the creation of a standardized definition of
work in order to facilitate project management. This primitive for describing
a unit of work may also allow contributors to create their own personalized
view of the state of the project while relying on Git and GitHub for consistency
and durable storage.

## Unresolved Questions

- How reviewers and approvers are assigned to a PEP
- Example schedule, deadline, and time frame for each stage of a PEP
- Communication/notification mechanisms
- Review meetings and escalation procedure

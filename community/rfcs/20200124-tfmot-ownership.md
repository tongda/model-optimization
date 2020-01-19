# TensorFlow Model Optimization Ownership

Status        | Accepted
:------------ | :------------------------------------------------------
**RFC #**     | [227](https://github.com/tensorflow/model-optimization/pull/227)
**Author(s)** | Alan Chiao (alanchiao@google.com)
**Sponsor**   | TFMOT
**Updated**   | 2020-01-24

## Objective

The TensorFlow Model Optimization repository is a central place for people to
contribute techniques. These techniques help users deploy more efficient machine
learning models. A notion of ownership is necessary to scale TensorFlow Model
Optimization (TFMOT) in all areas.

The TFMOT repository intends to only provide tools that are useful.
“Useful” by itself is ambiguous. A clear description of ownership (e.g. in terms
of user support) makes the user experience more consistent and helps ensure a
minimal bar for “usefulness”.

## Motivation

TFMOT will share ownership on a per-technique basis. This shared ownership is
significant because end-users use TFMOT one technique at a time. The ownership
structure must enable growth and iteration of new techniques, while balancing
the needs of a good, overarching user experience and TFMOT as a collective.

## Design Proposal

### Ownership

There are two terms of service (TOS), one is production-level and one is
research-level. See the community engagement  and
development sections for what each TOS involves. The TOS of a
technique will be stated in multiple areas, including the technique
documentation’s overview page.

At a high-level, ownership is split between TFMOT and technique contributors as
follows:

TFMOT

*   Overarching user experience spanning techniques and documentation
*   Shared abstractions for techniques and shared test infrastructure
*   Github project and development infrastructure: continuous integration, api
    doc generation, and release
    *   research-level TOS techniques are included but not planned for in
        non-nightly releases
*   Communication of Google-only test failures

Technique Contributor: everything specific to the technique

*   APIs and implementation up until shared abstractions
*   Documentation and tutorials
*   Community engagement and maintenance according to their TOS
*   Semi-annual reviews on benefits and usage, major updates, and TOS adherence
    (TBD on review details)

### Implementation of Ownership

#### Ownership Attribution

Attribution occurs on Github on a per-subpackage basis (e.g. a category such as
quantization may have multiple subpackages) and on
[tensorflow.org/model_optimization](https://www.tensorflow.org/model_optimization)
in a sensible manner. There may be a blog post to attribute the first owner, as
a part of the
[technique contribution process](https://github.com/tensorflow/model-optimization/blob/master/CONTRIBUTING_TECHNIQUE.md).

##### Github

Owners

| Subpackage                    | Owners            | Contact Information            |
| :-----------------------------| :---------------- | :----------------------------- |
| tfmot.quantization            | TF MOT            | @tf-mot mailing list           |
| tfmot.technique (or category) | Group Name        | @mailing list, @Person1        |
| tfmot.common.keras            | TF MOT / Google   | @Person2, @tf-mot mailing list |
| tfmot.technique2              | Google Group Name | @google mailing list           |

Note: the contact information should only be used when the OWNER or docs say so.
Regular communication should be done via Github issues and other channels. A
star by an individual's name indicates to directly assign everything to that
person, as opposed to having the OWNER assign issues.

A group name is required, with the idea that a group with vested interest is
needed to maintain a technique. In the future, SIG-MOT could be the group name,
but this would be discussed on a case-by-case basis and require an agreed path
for long-term sustenance.

##### tensorflow.org/model_optimization

Under the title of the technique overview page, there will be a “Owned By [Group
Name]” in small text. If ownership changes, this will change also. The original
owner will keep their attributions on any other existing public documents (e.g.
blog posts).

#### Community Engagement

Each technique has its own Github label. People
who contribute PRs and file issues are responsible for manually adding the
technique-specific Github label and potentially assigning them, based on
recommendations from the
[CONTRIBUTING.md](https://github.com/tensorflow/model-optimization/blob/master/CONTRIBUTING.md)
file and
[issue templates](https://github.com/tensorflow/model-optimization/issues/new/choose).


##### Production TOS

*   Answer questions and review PRs and issues, including ones in the TensorFlow
    Official Models integration
*   Provide release notes
*   Watch out for unlabeled PRs and issues that are relevant to their technique

##### Research TOS

*   Answer questions in issues. Users can only file issues for questions related
    to research (such as the theory or the algorithm) and documentation,
    including ones in the TensorFlow Official Models integration.
*   Address bugs on anything the OWNER claims support for on the technique
    overview page

The following is not a part of the research TOS, unless the technique says
otherwise on its overview page.

*   Address feature requests (e.g. model coverage beyond what's explicitly
    listed in overview page)
*   Accept PRs. There is an exception, within reason, described in the
    Development section.
*   Provide release notes

#### Development

Every technique has three stages: introduction, maintenance, and deprecation.

##### Introduction

Contributors must follow the
[technique contribution process](https://github.com/tensorflow/model-optimization/blob/master/CONTRIBUTING_TECHNIQUE.md).
Afterwards, they gain ownership, which involves the below responsibilities.

Once a go-ahead is given to develop the technique, TFMOT will grant [Github Read access](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/repository-permission-levels-for-an-organization#in-this-article) so the collaborators on a technique
can review each other's code.

##### Maintenance

*   TFMOT enables OWNER to handle PRs and issues by granting
    [Github Triage access](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/repository-permission-levels-for-an-organization#in-this-article).
    [CONTRIBUTING.md](https://github.com/tensorflow/model-optimization/blob/master/CONTRIBUTING.md)
    describes the PR submission process.
*   PRs
    *   Within reason, TFMOT can allow PR submission without the OWNER’s
        approval. In this case, TFMOT must add an OWNER for a post-submit
        review. An example would be an automated change that spans multiple
        projects including TFMOT.
    *   For changes originating from inside Google, equivalent public PRs are
        automatically created
        [(example)](https://github.com/tensorflow/model-optimization/pull/161)
        and the OWNER reviews the PRs there.
*   All public API changes must be approved by TFMOT through a process (TODO).
*   Semi-annual reviews (TODO)

**Production TOS Only**

*   All public API changes must be backwards-compatible until the next major
    release (including changes that modify convergence behavior of previously
    supported models).
*   Open source and Google-internal continuous integration and presubmit.
    *   Open source failures: error logs are visible and OWNER fixes them.
    *   Google-internal failures:
        *   If it’s not technique specific, TFMOT will fix it.
        *   If it’s technique specific: TFMOT will submit PRs with disabled,
            failing unit tests that account for the failure. The OWNER will then
            fix those unit tests.
    *   Whenever a convergence or training speed test fails (potentially days
        after integrations):
        *   TFMOT will isolate the problematic CL and do a rollback. In the
            long-term, if practical, the contributor will be responsible for
            rollbacks.
*   Adherence to TensorFlow and project health best practices (e.g. new
    TensorFlow major releases, Python compatibility). This includes
    comprehensive public API testing (see pruning as an example) within 3 months
    of initial launch.

**Research TOS Only**

*   Open source continuous integration and presubmit.
    *   Open source failures: error logs are visible and OWNER fixes them.
    *   Whenever a convergence test fails (potentially days after integrations):
        *   TFMOT will isolate the problematic CL and do a rollback.
*   Minimal project health (e.g. reasonable binary size)

##### Deprecation

*   Communicate to community with TFMOT including gauging interest.

**Production TOS Only**

*   If interest is there and the reason for deprecation is due to lack of human
    resources, find suitable OWNER to help or replace current OWNER, with the
    help of TFMOT.

## Questions and Discussion Topics

For all parties in the TFMOT community, consider their perspective: will
ownership be easy and clear?

*   What ownership boundaries remain ambiguous?
*   Does anything prevent these owners from acting like an owner for their
    areas?
*   Are the responsibilities too much given the workload and amount of
    resources?

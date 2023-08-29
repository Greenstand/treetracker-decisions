# Initial plan for organization of ADR files

* Status: proposed 
* Deciders: @ZavenArra {list everyone involved in the decision using github handle} <!-- optional -->
* Date: 2022-05-11 {YYYY-MM-DD when the decision was last updated} <!-- optional -->


## Context and Problem Statement

We have many decisions to make when building the treetracker platform, and we will use Architectural Decision Records (ADRs) to make more and more of these decisions. (https://adr.github.io/madr/)  ADRs could be organized in a few different ways - should all decisions to be tracked in one central ADR repository, or should they be split up by teams or project areas?  ADRs are assumed to be stored in a github repository or several repositories.

## Decision Drivers <!-- optional -->

* Ease of locating ADRs for developers
* Ease of finding ADRs relevant to a particular microservice, cloud service, or project focus
* Clarity of where to create a new ADR for a particular microservice, cloud service, or project focus

## Considered Options

* Place all ADRs in one repository, organization-wide.
* Create ADR respositories for aggregate project areas - cloud, frontend, and backend.
* Create a decisions folder within each repository, to hold only decisions relevant for that repository.
* … <!-- numbers of options can vary -->

<!--
## Decision Outcome

Chosen option: "{option 1}", because {justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

### Positive Consequences <!-- optional -->

* {e.g., improvement of quality attribute satisfaction, follow-up decisions required, …}
* …

### Negative Consequences <!-- optional -->

* {e.g., compromising quality attribute, follow-up decisions required, …}
* …
-->

## Pros and Cons of the Options

### Place all ADRs in one repository, organization-wide

This is the initial status quo.  https://github.com/Greenstand/treetracker-decisions

* Good, because all ADRs reside in one location and are easy to find
* Good, because we can revisit this decision later and split off separate ADR repositories as needed
* Bad, because ADRs tracking decisions of various scope, large and small, global and very specific, are combined together and thus difficult to search.

### Create ADR respositories for aggregate project areas - cloud, frontend, and backend

* Good, because ADRs are separated by areas of concern
* Bad, because combining all frontend, or all backend, ADRs is only a marginal improvement in findability
* Bad, because some decisions will span cloud, frontend, and backend, making a fourth global ADR repo still necessary

### Create a decisions folder within each repository, to hold only decisions relevant for that repository.

* Good, because specific decisions for specific services and applications are contained within the repo for that project
* Bad, because ADRs are spread apart in many different repositories, making them hard to review en masse

<!--
## Links <!-- optional -->

* {Link type} {Link to ADR} <!-- example: Refined by [ADR-0005](0005-example.md) -->
* … <!-- numbers of links can vary -->
-->

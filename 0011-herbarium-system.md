# Design of the herbarium system

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: @ZavenArra @CamWebb
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2022-06-07

Technical Story: 
* https://github.com/Greenstand/treetracker-herbarium-api/issues/1 
* https://www.figma.com/file/RrG8fahXei5SDY6AYjYYnn/Digital-Herbarium---Species%2FCommon-Names?node-id=0%3A1

## Context and Problem Statement

Tree planting and tracking organizations use various, localy relevant common names to identify the species of tree they are tracking.  This leads to inconsistencies when identifying which species was actually tracked.  Treetracker requires an herbarium service to connect these common names to the globally recognized scientific name.  The herbarium service can also also server as repository of identifying information and socially or ecologically relevant parameters for tree species tracked by organizations.


## Decision Drivers <!-- optional -->

* {driver 1, e.g., a force, facing concern, …}
* {driver 2, e.g., a force, facing concern, …}
* … <!-- numbers of drivers can vary -->

## Considered Options

1. Implement an herbarium microservice to provide organization specific common names
2. Alternative option?

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

## Pros and Cons of the Options <!-- optional -->

1. Implement an herbarium microservice to provide organization specific common names, while aggregating globally relevant species information.

ERD for the proposed herbarium microservice can be found here: https://raw.githubusercontent.com/Greenstand/treetracker-herbarium-api/main/docs/Herbarium%20Schema.png


* Good, because {argument a}
* Good, because {argument b}
* Bad, because {argument c}
* … <!-- numbers of pros and cons can vary -->

### {option 2}

{example | description | pointer to more information | …} <!-- optional -->

* Good, because {argument a}
* Good, because {argument b}
* Bad, because {argument c}
* … <!-- numbers of pros and cons can vary -->

### {option 3}

{example | description | pointer to more information | …} <!-- optional -->

* Good, because {argument a}
* Good, because {argument b}
* Bad, because {argument c}
* … <!-- numbers of pros and cons can vary -->

## Links <!-- optional -->

* {Link type} {Link to ADR} <!-- example: Refined by [ADR-0005](0005-example.md) -->
* … <!-- numbers of links can vary -->

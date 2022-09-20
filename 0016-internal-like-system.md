# Internal Like system

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Input: Welcome from anyone!
* Deciders: {list everyone involved in the decision using github handle}
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2022-09-09 <!-- optional -->


## Context and Problem Statement

The Facebook Like button on the web map currently has some challenges with the Eurpean Region, https://developers.facebook.com/docs/plugins/like-button

Another issue is that we are not able to create a custom Like button in the Greenstand style, https://github.com/Greenstand/treetracker-web-map-client/pull/395

## Decision Drivers <!-- optional -->

* {driver 1, e.g., a force, facing concern, …}

## Considered Options

* Option #1 - Internal Like system, to save likes on trees/entities directly
* Option #2 – Internal Like system linked with Keycloak user authentication system

## Decision Outcome

<!-- Chosen option: "{option 1}", because {justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}. -->

Option #2 because this way we can authenticate users and link them to their likes which resolves the bad UX issue of option #1. It also resolves the potential risk of cheating.

### Positive Consequences <!-- optional -->

Option #1
* Users do not need an social media account, for example Facebook
* The like data becomes our own
* Implementation of a custom UI Like button would be possible, currently we have a standard Facebook Like button

### Negative Consequences <!-- optional -->

Option #1
* Risk of "cheating", not being able to identify the user
* Bad UX, user who liked a tree returns back to see no record of them liking the tree

## Pros and Cons of the Options <!-- optional -->

### {option 1}

* Good, beacuse it allows every user to like an entity
* Good, because it allows for an custom Like button
* Good, because we can control and analyse the data
* Bad, because it opens up to a risk of "cheating"

### {option 2} OPEN TO YOUR SUGGESTIONS

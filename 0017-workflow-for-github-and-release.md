# The spec for how to manage Github branch and release versions to envs

* Status: proposed 
* Deciders: @nmcharlton @Kpoke @RubenSmn @dadiorchen
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date:  Tue Nov  1 10:53:58 CST 2022

Technical Story:
This proposal want to solve some problem we are facing now:
* How to develop multiple feature for our apps in parallel

  Sometimes put all new feature into a single branch is not a good idea, because:
  * It reduce the quality of the code, new feature without fully test can crash the app frequently.
  * It is difficult to code sometimes, different features fight with each other and cause conflicts.

* How to test and use them?

  We need to find a way to test the in-progress feature easily, to deliver them to our testers (in test env) and end user (in prod), for example, the beta admin panel and web map. Currently we has some ad-hoc way to do this, but it brings works to set up it evertime, we need find a reuseable way to quickly do it and switch between all kinds of different features/branches.

* How to maintain some previous versions in production.

  We need to maintain some legacy version in production (because our clients take time to upgrade their code to adapt our new versions) and fix bugs for them. For example: the old web map, and v1 wallet api. We need to find a standard and easy way to do it.

The proposal: https://docs.greenstand.org/engineering/how-to-manage-code-on-github-and-release-the-versions-to-envs

## Context and Problem Statement

## Decision Drivers <!-- optional -->


## Considered Options


## Decision Outcome


### Positive Consequences <!-- optional -->


### Negative Consequences <!-- optional -->


## Pros and Cons of the Options <!-- optional -->

## Links <!-- optional -->

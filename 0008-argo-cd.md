# Switching CD Systems from Github Workflows to ArgoCD

* Status: accepted 
* Deciders: @Emmanue @TANguyen1893 @ZavenArra
* Date: 2022-06-02

## Context and Problem Statement

Currently at Greenstand, Github is used as both a CI and CD system. Github is not designed to be a CD system. We store Kubernetes configurations as secrets within Github to be able to deploy to our environments so this creates a tight coupling between Github and our Kubernetes clusters. It is also very difficult to determine which version of an application is deployed in each of our environments without looking at the source code. It is very cumbersome for developers and operations to deploy, debug, and maintain deployments with the current setup in Github.

## Decision Drivers

* Github is a source control system with CI capabilities, not designed for CD.
* Decouple Github from our Kubernetes clusters.
* Github does not provide visibility on the applications (health, deployment statuses) deployed in our Kubernetes clusters.
* Follow more of a GitOps style with ArgoCD

## Considered Options

* Sticking with Github workflows since it is fully functional and we can deploy to production easily
* ArgoCD

## Decision Outcome

* Move to ArgoCD because it facilitates deployments and gives an entire view of our Kubernetes clusters

### Positive Consequences

* Truly follow a CD model
* Each member of Greenstand can get a complete view of the applications that are running and deployed in each environment.
  * There is only one place to visit to determine which application versions are deployed
* Git will be the source of truth for all application deployments.
  * Any manual edits to resources in the Kubernetes resources will be reverted
  * Kubernetes will mirror whatever is in Git so any edits must be done through a PR and merged. This allows for a better audit trail.

### Negative Consequences

* Higher cognitive load. Developers need to understand the new workflows and the ArgoCD interface.
* Need to port over all existing applications and their Github pipelines so that they are registered with ArgoCD.


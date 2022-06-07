# Manual Regression Test Documentation and Process

* Status: proposed 
* Deciders: @ZavenArra @nmcharlton @gwynndp Elm Outcault
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2022-05-24 <!-- optional -->

Technical Story: {description | ticket/issue URL} <!-- optional -->

## Context and Problem Statement

Manual regression tests are needed for some frontend features that do not have automated testing yet, in particular integration tests.  Regression tests need to be defined in a standard format, and a process is needed for systematically performing these tests for the QC team.

## Decision Drivers <!-- optional -->

* Ease of documenting a regression test
* Clarity of process for test performance
* Reporting of test status

## Considered Options

1. Template format for regression test description, with automation to generate test checklist for QC 
2. Other options?

<!--
## Decision Outcome

Chosen option: "{option 1}", because {justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

### Positive Consequences 

* {e.g., improvement of quality attribute satisfaction, follow-up decisions required, …}
* …

### Negative Consequences 

* {e.g., compromising quality attribute, follow-up decisions required, …}
* …

-->

## Pros and Cons of the Options <!-- optional -->

### Template format for regression test description, with automation to generate test checklist for QC

Use the following markdown template for describing each regression test:
https://github.com/Greenstand/treetracker-admin-client/blob/master/docs/template/REGRESSION_TEST_TEMPLATE.md

Store regression tests in github reposistory for relevant frontend project, in docs/REGRESSION_TESTS.md.  

Create github action that reads REGRESSION_TESTS.md and createds a github issue with checklist consisting of each regression test title.  The github issue will be named to include the release tag.  The QC operator will edit this issue as they work through tests, and mark checkboxes as passing or list failures.  

The developer will close this issue when it has been determined that a release is ready for deployment.

* Good, because there is a standard format for regression tests
* Good, because it creates a semi-automated process supporting QC operators
* Bad, because it requires some github actions development work

# Provisions for the use of identity brokering and social login when accessing treetracker web applications

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: @ZavenArra @Dadiorchen @nmcharlton @gwynndp
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2022-05-11

Technical Story: 
* https://greenstand.gitbook.io/auth-system/
* https://github.com/orgs/Greenstand/projects/47

## Context and Problem Statement

Keycloak is being implemented to replace the current auth system used by the admin panel, and to provide a more generalized auth system for web apps and microservices.  Keycloak offers identity brokering and social login integrations for accounts from other providers, most notably Google and Facebook.  We should decide which, if any, social login providers to allow our users to use when accessing our systems.

## Decision Drivers <!-- optional -->

* Ease of use for our users
* Ease of implementation
* Exposing our users to negative consequences from other providers
* Implicit support of other providers technology philosophy

## Considered Options

1. Do not implement social login
2. Implement social login for all providers supported by Keycloak
3. Implement social login only for providers that are deemed philosophically aligned with values shared by Greenstand engineers.

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

### 1. Do no implement social login

Require all users to sign up and sign in to Greenstand services with a fresh username and password.

* Good, because we have a clean and simple login interface.
* Good, because we don't risk association with other platforms with spurious ethical practices.
* Bad, because the sign up and sign in process is more cumbersome for users.
* Bad, because all users have to remember (or store) one more set of credentials.


### 2. Implement social login for all providers supported by Keycloak

From https://www.keycloak.org/docs/latest/server_admin/

With Keycloak, users can log in to your application using a social network account. Supported providers include Twitter, Facebook, Google, LinkedIn, Instagram, Microsoft, PayPal, Openshift v3, GitHub, GitLab, Bitbucket, and Stack Overflow.

* Good, because most users will have an account with at least one of these providers.
* Good, because users can sign up and log in quickly and easily.
* Good, because users will make fewer mistakes and will require less admin support.
* Bad, because the list of supported providers is very long.
* Bad, because Greenstand may be seen as endorsing the providers listed, some of whose values do not necessarily align with ours.

### 3. Implement social login only for providers that are deemed philosophically aligned with values shared by Greenstand engineers.

Which providers should be included?  How do we make the decision to include certain providers, but not others?

* Good, because users can sign up and log in quickly and easily.
* Good, because users will make fewer mistakes and will require less admin support.
* Bad, because we may eliminate a lot of the benefits of social login if we exclude certain providers.


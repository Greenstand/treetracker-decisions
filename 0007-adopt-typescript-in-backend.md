# Adopt Typescript in our Backend

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: ZavenArra Daniel Dadiorchen Nick Aron
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2022-06-02 <!-- optional -->

Technical Story: {description | ticket/issue URL} <!-- optional -->

## Context and Problem Statement

With the backend getting more and more complex, we need to adopt Typescript to make it easier to maintain, code and read.

## Decision Drivers <!-- optional -->

* The complexity of the backend is growing, a weak type of language is becoming too hard to maintain the interface between: function, class, file.
* The code productivity.
* The code quality.

## Considered Options

* Using Typescript in the backend.
* Using vanilla Javascript in the backend.

<!-- ## Decision Outcome

Chosen option: "{option 1}", because {justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

### Positive Consequences 

* {e.g., improvement of quality attribute satisfaction, follow-up decisions required, …}
* …

### Negative Consequences 

* {e.g., compromising quality attribute, follow-up decisions required, …}
* … -->

## Pros and Cons of the Options <!-- optional -->

###  Using Typescript in the backend

* Good, because of a better code maintainability.

  With Typescript, we can navigate the code pretty much easier, to find definition of function/class/variables/files etc. If we use VSCode, we can check the definition by hovering the mouse over the variable, auto-completion when coding, auto-import when coding, auto-fix when warn/error detected. Basically, to jump between files is a big cumbersome using vanilla Javascript.

  Currently, my feel is that over 60% of library (popular) is supporting Typescript, even a lib is not written in TS, the lib provider will define the TS interface for user.

  With these supports, my experience is that with TS, the code productivity gains 10% higher than with vanilla Javascript, in a complicated logic business system.

  The code in Typescript is more readable.

* Good, because it brings higher code quality. 

  With the TS type checking, we can make sure every invocation of the function/class is correct, this can help on development stage (by auto-fix), and also help on test and production stage, by denying committing or CI. Especially when the logic is complicated, and when we change the interface of the code when evolving or refactoring.

* Bad, poorer performance. 

  With TS, it's for sure, when coding, some feature like: auto-refresh would be slower. It also take extra time to compile the code.

  For production env, we can run complied pure Javascript code instead.

* Bad, higher learning curve. 

  Developer need to learn Typescript.

  But, I suggest we just use the most basic features of Typescript. Because a over-typed System goes to a wrong direction, actually it makes the code less readable.

  So I suggest we just use TS in a simple way, and if we set up the codebase with Typescript correctly, then a newcoming developer shouldn't need to take a long time to learn Typescript to start coding. 

### Using vanilla Javascript in the backend.

* Good, because of less learning curve
* Bad, because complex domain logic is poorly expressed

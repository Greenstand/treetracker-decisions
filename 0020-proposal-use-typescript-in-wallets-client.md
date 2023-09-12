# Use React with TypeScript instead of plain JavaScript in a new Wallets Admin Client

- Status: proposed
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
- Deciders: TBD
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
- Date: 2023-04-06

Technical Story: {description | ticket/issue URL} <!-- optional -->

## Context and Problem Statement

Using TypeScript (and strong-typing) can bring to a project lots of benefits.

## Decision Drivers <!-- optional -->

- Type Safety: catching errors on compilation stage instead of runtime
- Better code maintainability over the time
- No need to rewrite the code when the project will grow up
- TypeScript is already used on the Backend - Wallet API. Unified style will be easier for Full-Stack developers

## Considered Options

- **Option #1** Fully adopt TypeScript
- **Option #2** Use basic features of TypeScript like types, interfaces, ect
- **Option #3** Use plain JavaScript

## Decision Outcome

TBD

## Pros and Cons of the Options <!-- optional -->

### {option 1}

Fully adopt TypeScript

- Good, because we will benefit from type safety - will be able to catch errors early on compile time, not in runtime
- Good, because it will give higher code mainteinability over time
- Good, because it has better IDE support
- Bad, because of the higher learning curve, expeacially for junior developers
- Bad, because it will require additional project configuration

### {option 2}

Use basic features of TypeScript like types, interfaces, ect

- Good, because we will have same advantages as with option 1
- Good, because it will be much easier for new developers to start with
- Bad, because it will require additional project configuration

### {option 3}

Use plain JavaScript

- Good, because it will not require people to learn TypeScript if they are not familiar with it
- Bad, because we will not get benefits described in option 1
- … <!-- numbers of pros and cons can vary -->

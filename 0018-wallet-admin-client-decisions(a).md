# Wallet Admin client design and integration decision 

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: @OlhaD
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: March 29th, 2023

## The Context
The end goal of the wallet admin client is to enable the flow of tokens from growers to consumers. 
The token flow has several steps:
1. Field transfer (a request from the app to mint the token into a specific wallet/send it to a specific organization)
2. The verification of the token's attributes (takes place in the main admin panel)
3. The transfer of the token via the Wallet API (which is done with third-party tools such as Postman, or by tools built by integrators. 
4. This tool will use some of the same microservices as the admin panel, plus the wallet microservice

The Wallet Admin Client user is potentially a different set of users than the main admin client. 
## Links <!-- optional -->

https://github.com/Greenstand/treetracker-wallet-admin-client

## The question
Who is the main Admin Client User and what is their use case story?
What will the wallet admin client be?
What will the authentication mechanism look like?
Who is the team?
Lead @OlhaD

## Prerequisites: 
Write user story

## Considered Options

* **Option #1** Make this a 'free standing' wallet admin panel application with its own design and a separate repro
* **Option #2** Make a module/extension within the admin panel fully integrated
* **Option #3** Make a copy of the admin panel in terms of design and some of the features such as filters. 

## Use case separation

* The wallet admin user can but does not have to be an admin panel user. 
* An admin panel user can but does not have to be a wallet user. 

## Clarification : 
The authentication and authorization should not be a custom implementation. Both admin client and wallet admin client, and any other client, should all use the same method of authn and authz, so that accounts can be granted access to clients as needs arise. A probable option is keycloak for sign in. 

Note: this replaces previvious thinking in that this tool will have a seperate login.

## Decision Outcome

Start developing. We have a potential team and team lead for the development of this project. 


## Pros and Cons of the Options <!-- optional -->

### {option 1}

Make this a 'free standing' wallet admin panel application with its own design and a separate repro

* Good, Because - Clear separation of use cases: Keeping the wallet admin client as a separate application allows for clear separation of use cases. Admin Panel users do not necessarily have to be wallet users, and wallet users can have their separate authentication mechanism. This can provide better security and privacy for wallet-related operations.
* Good, Because - Reduced dependency on the main Admin Panel: Creating a separate wallet admin client reduces the dependency on the main admin panel.
* Good, Because Independent development and releases.
* Good, Because it gives the flexibility to use other development tools/approaches (like using TypeScript e.g.).
* Good, because making a separate unit could enable more flexibility in the design process 
* Good, because as a free-standing unit, newly released features will have less of a chance of impacting the current working functionality.
* Bad, because it could add more work to Nick's plate
* Bad, because it could create added ongoing maintenance that could be avoided by keeping it together with the main admin panel

### {option 2}

Make a module/extension within the admin panel fully integrated

* Good, because it keeps it all unified and relatively simple
* Good, because any user on the admin panel can 
* Bad, because there may be login issues as the wallet has more security / different login info
* Bad, because it could compromise the security of the wallet

### {option 3}

Make a copy of the admin panel in terms of design and some of the features such as filters. 

* Good, because users want to send tokens based on the trees they selected with the admin panel features.
* Bad, because it has the same concerns as option #1

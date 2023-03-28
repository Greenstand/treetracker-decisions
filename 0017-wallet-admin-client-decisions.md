# Wallet Admin client design and integration decision 

## The question

What will the wallet admin client be?
How will the authentication mechanism look like?

## Considered Options

* a free standing admin panel like application with its own design in its seperate repro
* a module / extension within the admin panel fully integrated
* a copy of the admin panel in terms of design and some of the features such as filters. 

## Use case seperation

* The wallet admin user can but does not have to be an admin panel user. 
* An admin panel user can but does not have to be a wallet user. 
* A wallet has its own password. 

## Decision Outcome

Start developing. We have a potential team and team lead for the development of this project. 

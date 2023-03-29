# Treetracker Decisions

Welcome to Treetracker Decisions, our Architectural Decision Record repository where Greenstand Engineers collaborate on important decisions about the design, architecture, and philosophy of the Treetracker platform! 

Your involvement here is encouraged; reviewing past decisions can help you learn about the goals and philosophy of the project.

The process allows your involvement by: 
- Authoring new ADRs
- Reviewing ADRs that are OPEN Pull Request
- Commenting on ADR's that are OPEN Pull Requests
- Approving and Merging ADR's that have a finalized decision (requires repository 'Write' permission)

More information on ADRs and the base template we use can be found here: https://adr.github.io/madr/

## Basic Overview
- ADRs are considered "open" when they are open Pull Requests against this master branch: https://github.com/Greenstand/treetracker-decisions
- Changing or authoring an ADR takes place on *your personal Github fork*
- Anyone can make an ADR - don't be shy, give it a try. 

Follows these steps:
1. Fork the repository to your own github repository
2. Write, name, or edit an ADR and commit it to your branch.
3. Make a Pull Request back to either the master branch on the Greenstand repository (or to the branch of a collaborator)
4. Get others involved


Note for administrators with 'write' permission - DO NOT merge the open pull request to the master branch until it is approved.

## Authoring new ADRs

* Anyone is welcome to author a new ADR, however, it's normally most useful for someone with deep knowledge of the project to author ADRs.
* By authoring an ADR, you are responsible for soliciting feedback and incorporating that into a final document that can be accepted by the deciders.
* To author an ADR, fork this repository and create a branch with a name like '{NNNN}-{adr-name}' where NNNN is next unused ADR id, and {adr-name} is a descriptive name for your ADR.
* Copy the ADR_TEMPLATE.md to a file name '{NNNN}-{adr-name}.md' and update this new file to contain all information, context, and options for your needed decision.
* Commit your ADR file and open a PR against this upstream repository (gh pr create)
* Solicit feedback for your ADR and incorporate that into your document.

## Reviewing ADRs - Basic

* ADRs open for review are available as [open pull requests](https://github.com/Greenstand/treetracker-decisions/pulls).
* Each ADR exists as a markdown file.
* To review an ADR, first navigate through the PR to the markdown file and read it.
* Then begin a PR review and add your comments or suggested additions.
* You may also open the PR itself on your own fork ( gh pr checkout #) and submit your own PR against the author's fork if you wish to make substantial additions. (See Reviewing ADRs - As new PR)
* Anyone may contribute during the ADR PR review process

## Reviewing ADRs - As New PR

1. find PR at https://github.com/Greenstand/treetracker-decisions
1. check out the PR gh pr checkout [#ofPR]
1. when ready to submit look for the url of the PR owner’s fork
1. for this PR: https://github.com/Greenstand/treetracker-decisions/pull/1
1. the PR owner’s fork is: https://github.com/ZavenArra/treetracker-decisions
1. find it by clicking through to the branch they want to merge the PR from and then remove the branch info from the end of the url
1. submit your feedback in a new PR to PR owner’s fork gh pr create -R [url/owner/repo]






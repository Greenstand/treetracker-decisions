# Treetracker Decisions

Welcome to Treetracker Decisions, our Architectural Decision Record respository where Greenstand Engineers collaborate on important decisions about the design, architecture, and philosophy of the Treetracker platform!  There are 2 main ways to be involved: reviewing and improving ADRs currently in open Pull Requests, and authoring and then guiding new ADRs into approval.  Details on both of these processes are below, and information to this end will be iteratively improved.  You may also want to review the history ADRs to get a feel for the goals and philosophy of our project.

More information on ADRs and the base template we use can be found here: https://adr.github.io/madr/


## Reviewing ADRs - Basic

* ADRs open for review are available as [pull requests](https://github.com/Greenstand/treetracker-decisions/pulls).
* Each ADR exists as a markdown file.
* To review an ADR, first navigate through the PR to the markdown file and read it.
* Then begin a PR review and add your comments or suggeseted additions.
* You may also open the PR itself on your own fork ( gh pr checkout #) and submit your own PR against the authors fork if you wish to make substaintial additions. (See Reviewing ADRs - As new PR)
* Anyone may contribute during the ADR PR review process

## Reviewing ADRs - As New PR

1. find PR at https://github.com/Greenstand/treetracker-decisions
1. check out the PR gh pr checkout [#ofPR]
1. when ready to submit look for the url of the PR owner’s fork
1. for this PR: https://github.com/Greenstand/treetracker-decisions/pull/1
1. the PR owner’s fork is: https://github.com/ZavenArra/treetracker-decisions
1. find it by clicking through to the branch they want to merge the PR from and then remove the branch info from the end of the url
1. submit your feedback in a new PR to PR owner’s fork gh pr create -R [url/owner/repo]

## Authoring new ADRs

* Anyone is welcome to author a new ADR, however it's normally most useful for someone with deep knowledge of the project to author ADRs.
* By authoring an ADR, you agree to be responsible for soliciting feedback and incorporating that into a final document that can be accepted by the deciders.
* To author an ADR, fork this repository and create a branch with a name like '{NNNN}-{adr-name}' where NNNN is next unused ADR id, and {adr-name} is a descriptive name for your ADR.
* Copy the ADR_TEMPLATE.md to a file name '{NNNN}-{adr-name}.md' and update this new file to contain all information, context, and options for your needed decision.
* Commit your ADR file and open a PR against this upstream repository (gh pr create)
* Solicit feedback for your ADR and incorporate that into your document.




# Adapt NoSQL Database into Greenstand

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: @nmcharlton @emmanue @dadiorchen @ZavenArra
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: Wed Feb  1 11:23:50 CST 2023


Technical Story: PostgreSQL is powerful relational DB and fits data storage for enterprise business. But there are cases on Greenstand that is more internet style and flavor, for example the poll system, the like system, for these cases, the characteristics of these case is: the data is not very complex, and the data is not very big, and the data is not very important, and the data is not very sensitive, but on the other hand, the data is very frequent, and the data is very dynamic, and the data is very easy to change. So we need to find a DB that is more suitable for these cases. (these two sentences are created by openAI, I think it goes my way, so I copy them here :) ), like the `like system`, it is an internet feature, it is possible that we will have millions of likes in the future, and also possible that thousands of users will click that `like` button at the same time, we don't want this kind of tiny feature to consume the resources of the PostgreSQL connections which are pretty limited and costly. So I think we can use no-sql like MongoDB to do a supplement to PostgreSQL.


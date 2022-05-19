# Greenstand should create a search engine for finding data and autocompleting user queries.

* Status: proposed 

* Deciders: @kparikh9, @dadiorchen

* Date: 2022-05-18

Technical Story: 
[Treetracker Query API Issue #90](https://github.com/Greenstand/treetracker-query-api/issues/90)

The search engine would be used to quickly and efficiently return rank-based results to a user. While Postgres can achieve the user stories described in the linked ticket with its full-text searching ability and use of the `LIKE` operator for pattern matching, it will not be able to order results based on relevance, popularity, or any other search based metric (which would have to be recorded in a separate column). Postgres assumes the inputted text appears somewhere in the values of the column it's querying, but won't find results if a character is out of order. A search engine will be able to fuzzy search what the user types and rank the results based on metrics it collects.

## Context and Problem Statement

What if an organization would like to look up the tree portfolio/wallet of a competing organization but didn't know the exact name of its wallet? What if a user wanted to search for a planter named 'Onyega Innocent G.' and all the trees he's planted but didn't remember how to spell his name?

## Decision Drivers

* ElasticSearch - can integrate well with other products of the Elastic Stack like Kibana, Logstash. Easiest to experiment with, since there are free trials available for Elastic Cloud (managed ElasticSearch deployment)

## Considered Options

* ElasticSearch
* Apache Solr
* Apache Lucene

## Decision Outcome

Chosen option: ElasticSearch ~ **Nothing determined yet**, but I've been exploring it for the past month - documentation is very specific and it seems to indicate that it is a very complex library that can suit almost any use case. Open to any input.

### Positive Consequences

* Extensive documentation
* Several examples online (blogs, case studies, etc.)

### Negative Consequences

* Steep learning curve?
* Requires more experimentation on what architecture is the best for Greenstand's use case (i.e. search over multiple indexes vs. one index)
* Heavy memory usage (requires 4.0 GB RAM just for ElasticSearch, probably more for Kibana and Logstash) - can be expensive since it requires larger compute servers and this would need to remain on at all times.
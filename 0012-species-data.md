# Management of Species Data

 * Background to this ADR: The species data management system we now
   have has grown up opportunistically. As the number of trees and
   organizations in Treetracker grows, the current system may be
   insufficient. A community conversation is needed. This ADR is an
   attempt to solicit and resolve inputs from the GS community.  The
   issue of reliably identifying the vast number of images (by humans
   or by AI) is related, but should be dealt with in another ADR. (The
   template of this ADR is slightly different from others in this
   repo.)
 * Deciders: @CamWebb, ...
 * Date: 2022-06-08

## Context

The trees being recorded by Treetracker belong to many different
species. While some organizations and token buyers do not require
species information, others do. Species vary greatly in their
ecological characteristics and any accurate measure of carbon storage
requires species data.

Identifying species and managing species information is hard. While
many species used in reforestation are globally common, there is a
very ‘long tail’ of hundreds of rarer species that will be used by
organizations. Species can be hard to identify and differentiate from
similar species using low-resolution photos.  For many species the
scientific taxonomy itself can be complex and unresolved.  Given these
complexities it is necessary to use a two-stage system: matching
images to morphotypes first, then identifying morphotypes to
scientific names, is possible.

The current Treetracker system evolved opportunistically. At first
anyone could add a species name in the admin panel.  There was soon a
mix of folk and scientific names, multiple name strings for the same
species, and some names that referred to many species.  The
involvement of an experienced tree botanist in 2020 led to the
development of a [herbarium][1] system, within GS, but not directly
linked to the database. Images from around the world were examined and
incorporated into a reference set of images and names (please read
[this][2]). The herbarium morphotype codes are now present in the
admin panel, and admin control is now exercised over the management of
names in the admin panel. (Another key issue, to be dealt with in
another ADR, is the database architecture for identifications:
currently only a single identification is stored for each image
capture, and any identification history or multiple opinions must be
recreated from the audit log - this is inefficient and in some cases
impossible.)

## Problem Statement

The current approach of a (single-)botanist-curated taxonomic
information system, external to the Treetracker database, is not
scalable, and as the numbers of captures and organizations in
Treetracker grows, is likely to fail to deliver.  How should this
system be modified to i) scale, ii) more efficiently integrate with
Treetracker data architecture, iii) better meet the needs of users?

## Decision Drivers

 * Accuracy of information vs. ability to handle increasing demands
   for less-accurate species information.
 * Stakeholder needs. Who really cares about accurate species
   information? While a botanist does, an organization just being paid
   per tree may not.
 * Technical architecture: speed and ease of data entry.
 * Technical architecture: efficiency of integration of information
   across the Treetracker system.

## Considered Options

### 1. Continue with existing system

The system: Individuals edit an [XML file][3], which then triggers (on
request) an online reference [herbarium][4]. Taxon codes are manually
copied to the species names table in the main DB.

**Pros**

 * Rapid data entry (text editor plus on-the-fly XML validation)
 * Rapid ability to change data structure (via editing the XML schema)
   This permits evolution and nuance as challenges and needs are
   better understood.
 * Permits transparent and recorded collaboration via Git and Github
   (though no one has yet collaborated).
 * Established and working well.

**Cons**

 * Use of unfamiliar/legacy tools.
 * Data structure (the XML file) is external to the main Treetracker
   DB, currently requiring manual transfer of data (though this could
   be scripted/crontabbed).
 * The level of detail (and idiosyncrasy of scripts) limits
   collaborators, which makes the system unscalable.

### 2. Alternative solutions

...

## Decision

...


[1]: https://github.com/Greenstand/Tree_Species
[2]: https://herbarium.treetracker.org/guide/names.html
[3]: https://github.com/Greenstand/Tree_Species/blob/master/tree_species.xml
[4]: https://herbarium.treetracker.org/guide/index.html

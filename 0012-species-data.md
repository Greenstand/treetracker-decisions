# Management of Species Data

## Document history

This ADR was first accepted in late 2022, with deciders @CamWebb and
@SebastianGaertner.  In late 2023 it was felt important to have a more
detailed document (a ‘whitepaper’) to guide development of a
stand-alone plant species service (see [project][4]). This ADR can
serve as this guide and is being reworked in early 2024, to be decided
by @CamWebb, @SebastianGaertner, and @dadiorchen.

## Context

**Species**: The plants being recorded by Treetracker belong to many
different species. While some organizations and token buyers do not
require species information, others do. Species information is
desirable for a wide range of reasons, including:

 * Estimating carbon storage: species vary greatly in their carbon
   density;
 * Species selection for different habitats: species vary greatly in
   their ecological characteristics and site tolerance;
 * Biodiversity banking: the simplest meaning of ‘biodiversity’ is
   species richness: more species = more biodiversity;
 * Curiosity, education, aesthetics and spiritual value: humans
   naturally seek to name things that vary, and a more diverse forest
   with elements we can name is simply more interesting than a
   monoculture, and is likely to be more valuable in the eyes of
   locals, and donors.
 
**Identification**: Identifying species and managing species
information is hard. While many species used in reforestation are
globally common, there is a very ‘long tail’ of hundreds of rarer
species that will be used by organizations. Species can be hard to
identify and differentiate from similar species using low-resolution
photos.  For many species the scientific taxonomy itself can be
complex and unresolved.  Given these complexities it is necessary to
use a two-stage approach: matching images to morphotypes first, then
identifying morphotypes to the most likely scientific name, if
possible.

**Species in the admin panel**: The current Treetracker system evolved
opportunistically. At first anyone could add a _species name in the
Admin panel_.  There was soon a mix of folk and scientific names,
multiple name strings for the same species, and some names that
referred to many species.  The involvement of an experienced botanist
in 2020 led to the development of a _[herbarium][1] system_ (and see
[guides][3]), within GS, but not directly linked to the
database. Images from around the world are examined by the botanist
and incorporated into a reference set of images and names (please read
[this][2]), and herbarium species codes can be added to the names in
the Admin panel species list where the botanist has had time to review
the Admin panel names.  Metadata about all admin panel names is
managed in a third data resource, an _Airtable database_, which serves
as a necessary information buffer between the Admin panel names and
the herbarium.  Clearly, a more integrated system is needed for
managing names and name metadata. A solution is proposed in this
document.

The current dependence on a single volunteer botanist to maintain the
reference herbarium is also a weak link as the number of taxa grows,
and is discussed her.

**Recording determinations**: Currently, the choice of species for a
capture made by an admin panel user overwrites any previous decisions
for that capture. Each choice _is_ logged in the `audit` table, but in
a JSON structure that is inefficient to use in table joins.  A new
system must efficiently record multiple determinations for each
seedling, and for matches of admin panel names and reference names.

## Problem Statements

**A** Most organizations have local names for the species they are
planting, but may not know the scientific name and may not have time
for or interest in matching their taxa with the herbarium taxa. Being
provided with the total list of accepted GS taxa in the admin panel is
overwhelming, and if one of their species has not yet been examined by
a GS botanist, the organization cannot select an identification. In
addition, for the botanist to have access to an organization’s local
species list is very helpful, but communicating with the organization
to get this list can be hard.  What is needed is: a **short list of
local names** visible to each Admin panel user, that can be edited and
added to.

**B** Infrastructure must be built to **match this local to the
global** set of taxa (with TT) by another Admin panel user.

**C** The current way that determination decisions are recorded in the
DB prevents many important queries. The act of choosing a species for
a seedling, and matching a local name to a global name (known as
“**determinations**”) needs to be recorded, with metadata of: person,
timestamp and reason.

**D** The external data in the herbarium database and Airtable
database need to be **integrated** directly into the TT data model.

**E** The metadata collated about each species in the herbarium module
and the large set of sample images provided by the links to TT
captures represents a **globally valuable information resource**. A
web service can be built around this for other users outside GS.

**F** The social question of who has the **authority** to make a
definitive match between local and global taxa must be resolved.

## Proposed solution

Note: in the following, no suggestions are made on the mechanics of
data transfer between TT and clients. TT has been moving towards a set
of RESTful API microservices, but there may still be a use for non-API
communication in some parts of this solution.

### 1. Modify TT database structure

_(addresses problems A, B, C, D)_

The following schema indicates a minimum set of new tables and fields
to contain the data for the suggested solution.  The **herbarium+**
table will likely be a set of interconnected tables that replicate the
data in the [current herbarium XML schema][5] and Airtable database.

```
            (NEW)               (NEW)           (NEW)             
trees       det                 localname       taxonmap            herbarium+
=====       ==========          =========       =============       =========
id    <-+   id              +-> id        <-+   id              +-> id
        +-- tree_id@        |   name        +-- localname_id@   |   morphocode
            localname_id@ --+   sci_name?       species_id@   --+   name
            admin_user_id@      org_id@         user_id@            (metadata)
            timestamp           user_id@        confidence
                                timestamp       reason
                                                timestamp
```

Key: 

 * “@” indicates a foreign key; “?” indicates a field may be NULL.
 * `admin_user_id` joins to existing `admin_user.id`.
 * `org_id` joins to existing `entity.id` for an organization.
 * The `trees` table listed here refers to the existing schema where a
   capture creates a `tree` entry. Some modification of this proposal
   will be needed when the new schema is adopted, i.e., with separate
   `tree` and `capture` tables. 
 * In the `localname` table: `UNIQUE KEY (name, org_id)`
 * In the `det` table, (`tree_id`, `localname_id`, `user_id`) is not
   unique, allowing a user to update (and revert) their determination, but the
   latest det should be used as the current det for that plant.
 * In the `taxonmap` table, (`species_id`, `localname_id`, `user_id`)
   is not unique, allowing a species admin to update (and revert) their mapping
   decision, but the latest species name should be used as the current
   species name for that localname.

The **displayed species** in Treetracker, will be:

 * `localname.name`, when there is not yet a `taxonmap` connection to
   the herbarium. The most recent determination will be shown if there
   are several.
 * `herbarium.name` plus `localname.name` plus `link`, when there is a
   `taxonmap` connection to the herbarium. `link` will be a hyperlink
   to the public representation of species data in `herbarium+`. Where
   there are several match choices made between a localname and a
   globalname, the latest choice will be shown, unless an
   authority/confidence hierarchy has been created for different users
   of the herbarium.

### 2. Modify ‘Verify’ admin panel

_(addresses problems A, C)_

 * Hiding the current species list
 * Adding the capability to add a species name
 * Allowing verification _only_ to the org-supplied list of species names
 * Tracking each determination of a seedling in the `det` table.

### 3. Create new ‘Herbarium’ admin panel

_(addresses problems B, C, D)_

This will have two views:

A **species** form (`herbarium+` tables) that will permit entering

 1. metadata about a scientific name, similar to the fields currently
    in the [current herbarium XML schema][5] and Airtable
    database. The form must Please consult @camwebb as this form and underlying
    tables are being designed. (Additional information on this may be
    added to this document as time permits.)
 2. links to images of plants of a species that best illustrate
    diagnostic features.
 3. The species form will also display thumbnails of images in TT that
    have been identified to this species.

A **matching** form (`taxonmap` table) that will optimize the user
experience of:

 1. selecting an ‘Org’ to work with
 2. seeing diagnostic images from the species (above) and 
 3. images of seedlings determined to a particular ‘localname’.
 4. making a decision about the best herbarium taxon, requiring a
    reason for the choice, and a confidence level.

Each matching choice will be recorded with username, timestamp and an
explicit reason for the choice.

### 4. Create external view and web service for herbarium data 

_(addresses problem E)_

The data in the `herbarium+` tables should be displayed both for TT
users seeking more information about a particular seedling, and for
external users interested in the accumulating species list and data in
TT. The eight-character ‘taxon code’ can be used as an identifier for
external users to refer to the TT herbarium.

### 5. Determine authorization process for trusted ‘taxonomic editors’

_(addresses problem F)_

The value of this system is partly based on the informed confidence a
user has that a seedling has been given the correct scientific
name. Plant taxonomy knowledge and experience are built over many
years and determine the confidence a one has in saying that two images
appear to be the same taxon. There is no shortcut for this experience.
The mapping of scientific names to images should not be open to all
admin users, and is definitely not a ‘democratic’ or poll-based
activity, as proposed in the [herbarium project][6].  At the same
time, relying on a single botanist to vet the species names is a
serious bottleneck for TT and not scaleable. Some intermediate
intermediate is needed between a single expert and a free-for-all of
admin users.  Determining this balance will be an important element of
building the proposed system.  

It is suggested that there actually be two levels of access: i) those
with authority to add/edit entries in the herbarium global species
list and to match local names, and ii) those with authority only to
match local names; numerically there will be more local-global matches
that need to be made than need to add taxa to the global list, and
semi-skilled users may be needed to make those local-global matches.

## Workflows

### Org user

 1. Each org creates a set of local names in the admin panel: name entries
    could just be local language names, or they may also have a
    tentative, unvetted scientific name. Those are the only names that
    appear to the org user (who starts with a blank list of names).
 2. The org user then determines the local name for each
    capture. Additional determinations may be made by the same or
    different users with authority to edit these captures.

### Local-global species match user

 1. A ‘species match admin’ level user of the admin panel will choose
    an Org’s local name to address. Images of seedlings determined to
    that localname will be shown on the left.
 2. A global species dropdown will populate an image panel on the
    right with diagnostic images of a particular species.
 3. Once a match has been found, e.g., `"mangga"@orgX` is mapped to
    global `MANGINDI`, a form will record text from the user about the
    reason for the match. ‘Save’ will write to the `taxonmap` table.
 
### Herbarium user

 1. Create, edit, delete taxa, filling in fields in a form. Note: even
    at this herbarium level, some taxa may still only be given
    ‘morphotype codes’ without species names. It will often be clear
    that there is a distinctive species, at several sites, but no name
    can be found for it. Similarly, some plant genera are almost
    indistinguishable at a seedling stage (e.g., _Citrus_), and will
    need to be treated in TT as a single taxonomy unit, identified
    only to genus.
 2. Diagnostic images from within TT, and from other sites
    (esp. iNaturalist) will be easily assembled into a gallery for each
    herbarium taxon.
 3. For the Herbarium UI user, who also has access to the matching UI,
    switching back and forth between these UIs should be easy and rapid.


[1]: https://github.com/Greenstand/Tree_Species
[3]: https://herbarium.treetracker.org/guide/index.html
[2]: https://herbarium.treetracker.org/guide/names.html
[4]: https://github.com/orgs/Greenstand/projects/84
[5]: https://github.com/Greenstand/Tree_Species/blob/master/tree_species.rnc
[6]: https://github.com/orgs/Greenstand/projects/84

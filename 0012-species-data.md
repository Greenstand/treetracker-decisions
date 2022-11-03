# Management of Species Data

 * Deciders: @CamWebb, @SebastianGaertner
 * Date: 2022-11-02

## Context

**Species**: The trees being recorded by Treetracker belong to many
different species. While some organizations and token buyers do not
require species information, others do. Species vary greatly in their
ecological characteristics and any accurate measure of carbon storage
requires species data.

**Identification**: Identifying species and managing species
information is hard. While many species used in reforestation are
globally common, there is a very ‘long tail’ of hundreds of rarer
species that will be used by organizations. Species can be hard to
identify and differentiate from similar species using low-resolution
photos.  For many species the scientific taxonomy itself can be
complex and unresolved.  Given these complexities it is necessary to
use a two-stage system: matching images to morphotypes first, then
identifying morphotypes to scientific names, if possible.

**Species in the admin panel**: The current Treetracker system evolved
opportunistically. At first anyone could add a species name in the
admin panel.  There was soon a mix of folk and scientific names,
multiple name strings for the same species, and some names that
referred to many species.  The involvement of an experienced tree
botanist in 2020 led to the development of a [herbarium][1] system
(and see [guides][3]), within GS, but not directly linked to the
database. Images from around the world are examined and incorporated
into a reference set of images and names (please read [this][2]). The
herbarium species codes are now present in the admin panel, and admin
control is now exercised over the management of names in the admin
panel.

**Recording determinations**: Currently, the choice of species for a
capture made by an admin panel user overwrites any previous decisions
for that capture. Each choice _is_ logged in the `audit` table, but in
a JSON structure that is inefficient to use in table joins.

_(The current dependence on a single volunteer botanist to maintain the
reference herbarium is a weak link as the number of taxa grows, but
this matter is not considered in this ADR.)_

## Problem Statement

 1. Most organizations have local names for the species they are
    planting, but may not know the scientific name and may not have
    time for or interest in matching their taxa with the herbarium
    taxa. Being provided with the total list of accepted GS taxa in
    the admin panel is overwhelming, and if one of their species has
    not yet been examined by a GS botanist, the organization cannot
    select an identification. In addition, for the botanist to have
    access to an organization’s local species list is very helpful,
    but communicating with the organization to get this list can be
    hard.  What is needed is: a _short_ list of local names that the
    organization can edit and add to.  
 2. The current way that determination decisions are recorded in the
    DB prevents many important queries. The act of choosing a species
    is a key data element, and needs its own table.

## Proposed solution

**Overall**: _1. Separate local, organization species from global,
vetted species, and 2. track each determination event, and each
local-to-global mapping within the database._

### User workflow

 1. Each org creates a set of local names in the admin panel: name entries
    could just be local language names, or they may also have a
    tentative, unvetted scientific name. Those are the only names that
    appear to the org user (who starts with a blank list of names).
 2. The org user then determines the local name for each capture. The
    determinations are logged in a joining `det` table, so that
    multiple opinions can be made about a tree’s name. In general, any
    display of the name will use the latest determination.
 3. A ‘species admin’ level user of the admin panel will create
    mappings between the localname (= morphotype) and a vetted list of
    global names (these may also just be morphotype codes in the case
    where the final scientific name has not been settled). E.g.,
    `"mangga"@orgX` is mapped to global `MANGINDI`. Once
    this vetting has happened, individual trees can are then linked
    out to the global knowledge base. E.g., wood densities can be
    imported and carbon calculations could thus be made.

### Modified schema

```
            (NEW)               (NEW)           (NEW)             
trees       det                 localname       taxonmap            tree_species
=====       ==========          =========       =============       ============
id    <-+   id              +-> id        <-+   id              +-> id
        +-- tree_id*        |   name        +-- localname_id*   |   name
            localname_id* --+   sci_name%       species_id*   --+
            user_id*            org_id*         user_id*
            timestamp           user_id*        confidence
                                timestamp       notes
                                                timestamp
```

Notes: 

 * A “*” indicates a foreign key; a “%” indicates a field may be NULL.
 * `user_id` joins to existing `admin_user.id`.
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

### New UI

A new admin panel element will be required.  If a user has species
admin privileges, they will see a “Map localname to global species”
link. The UI element could operate thus:

 * Select organization to work with
 * See a table with a list of localnames for that organization in the
   left column (a hyperlink for each name might open a new browser tab
   showing some captures that have been determined to that local name).
 * In the next column, next to each localname is a dropdown menu (or
   incremental-lookup) allowing the admin user to select a global
   species for each localname. One or many localnames can be mapped at
   one time.
 * For each decision, two further columns offer a chance to assign i) a
   confidence measure to the mapping (dropdown menu: ‘high’, ‘medium’,
   ‘low’), and a ii) notes about the mapping.
 * “Save” and write new entries to `taxonmap` table.

[1]: https://github.com/Greenstand/Tree_Species
[3]: https://herbarium.treetracker.org/guide/index.html
[2]: https://herbarium.treetracker.org/guide/names.html

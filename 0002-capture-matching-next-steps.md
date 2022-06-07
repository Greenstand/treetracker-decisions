# Selecting the next steps to take to improve capture matching capabilties

* Status: proposed
* Deciders: @ZavenArra @DavidEzraJay @CamWebb, @Elforama {list everyone involved in the decision using github handle} <!-- optional -->
* Date: 2022-05-11

Technical Story:
* https://greenstand.gitbook.io/capture-matching/


## Context and Problem Statement

We have implemented a basic manual capturing tool that allows admin users to visually match repeat captures that are withing 10 meters of an initial capture.  This approach remains slow and difficult to do correctly.  We have identified a range of enhancements that will improve the usability of this tool, and need to decide where to focus resources.


## Decision Drivers <!-- optional -->

* Ease of implementing enhancements
* Improvement to capture matching efficiency and accuracy offered by enhancement
* Stability of enhancement across diverse use cases
* Ease of use for affected users

## Considered Options

1. Implement mobile application side image collection improvements, including enforced phone rotation, enforced compass bearing, and perhaps others.
2. Research feasibility of leveraging autocorrelation between GPS measurements to improve selection of candidates for location based matching of captures.
3. Create experimental manual capture matching UI that offers manual spatial analysis relevant to disambiguating matching captures.
4. Research approaches to image analysis that can produce a similarity metric to improve selection of candidates for location based matching of captures.
5. Attempt to train models using machine learning based on existing database of capture images to identify repeat captures.
6. Incorporate assigned species information during capture matching.
7. Create curated training sets of matched trees for to support training of machine learning based models.
8. Duplicate detection mode
<!--
## Decision Outcome

Chosen option: "{option 1}", because {justification. e.g., only option, which meets k.o. criterion decision driver | which resolves force {force} | … | comes out best (see below)}.

### Positive Consequences <!-- optional -->

* {e.g., improvement of quality attribute satisfaction, follow-up decisions required, …}
* …

### Negative Consequences <!-- optional -->

* {e.g., compromising quality attribute, follow-up decisions required, …}
* …
-->

## Pros and Cons of the Options

### 1. Implement mobile application side image collection improvements, including enforced phone rotation, enforced compass bearing, and perhaps others.

Currently, images captured on the mobile application do not have specific criteria guiding capture.  Thus, they exist at a variety of camera rotation angles and compass bearings, even for the same tree being repeat captured.  This requires a manual operator to consider images of the same tree that almost always exist at different rotations and focal distances, greatly enhancing the difficulty of an already challenging manual task.

Constraints can be implemented on the mobile side to ameliorate this situation.  For instance, camera rotation around the x axis could be constrained to 0, 45, and 90 degrees.  Additionally, compass bearing could be constrained such that captured photos are always north facing.  Implementing a constraint on focal distance is also possible, as has been identified in other apps.  

* Good, because the images captured will have more constitant parameters.
* Good, because this only affect the mobile application and does not require data research or algorithm development.
* Bad, because the mobile application will become more complex for our low technical literacy users, and we will have to invest careful UX/UI development time to make these features easily usable.

### 2. Research feasibility of leveraging error autocorrelation between GPS measurements to improve selection of candidates for location based matching of captures.

Preliminary research has shown that GPS measurements between trees, and along the path of tree trackers traveling between trees, have errors against true position that are highly autocorrelated.  This means that while the absolute position of a give measurement can be 3-10m off using L2 GPS technology, the direction of travel between measurements is relatively stable across site visits.  This presents opportunities for leveraging this similarity to identify captures in the vicinity of a capture from a previous site visit that are more likely to be repeat visits to the same tree, which can be used to present more likely candidates during the manual matching process.

* Good, because this does not affect the mobile users
* Good, because this feature uses data we are already collecting
* Good, because some preliminary research has already been done (@ZavenArra) that shows potential for this solution to work.
* Bad, because this feature requires advanced skills in GIS analysis and algorithm development.

### 3. Create experimental manual capture matching UI that offers manual spatial analysis relevant to disambiguating matching captures.

As in option 2, preliminary research has shown that GPS measurements between trees, and along the path of tree trackers traveling between trees, have errors against true position that are highly autocorrelated.  Instead of developing algorithms leveraging this information, a different approach is to develop a UI for the capture matching tool that gives operators visual information about site visits.  Since trackers often follow the same path between trees, anecdotal review of the GPS tracks following during tracking reveal that manually identifying which trees are the same between visits is very straightforward in certain cases when visualized correctly.  Furthermore, incorporating GPS error correction during this process would enhacne the benefit.

* Good, because this does not affect the mobile users.
* Good, because this uses data we already have.
* Bad, because it require complex, iterative UI/UX development and admin user training.

### 4. Research approaches to image analysis that can produce a similarity metric to improve selection of candidates for location based matching of captures.

Tree seedlings typically look quite similar, however this is somewhat age dependent.  Past a certain age, branch structure is not identical between trees.  Algorithms that profile branch structure could be used to produce a similarity metric between tree images, which could be incorporated into candidates provided during manual capture matching.

* Good, because this does not affect mobile users.
* Bad, because our current image captures are not standardized.
* Bad, because only certain age ranges or species will have different branching patterns.
* Bad, because this requires complex algorithm development by individuals with image processing, feature extraction, and statistical modeling skills.
* Bad, because no one has produced a similar algorithm before to our knowledge.

### 5. Attempt to train models using machine learning based on existing database of capture images to identify repeat captures.

Some interest has been expressed in exploring options to leverage machine learning to create a 'seedling recognition' algorithm.  Cam Webb produced the following feasibility study of this approach: https://camwebb.info/files/webb2022_thtp_recaptures.pdf.  The feasibilty study reports that machine learning cannot be leveraged to produce a 'seedline recognition' algorithm.   This does not rule out some other, creative application of machine learning however, such as that consistent tiwht option #4  

* Good, because this does not affect mobile users
* Bad, because our current image captures are not standardized
* Bad, because the feasibility study indicates this is not a feasible application of machine learning targetting recognition of individuals (individual trees in this case).


### 6. Incorporate assigned species information during capture matching.

Treetracker intends to assign species information to all incoming captures as part of capture verification.  Once assigned, this information can be used to reduce the candidate captures for assigning repeat visits in the manual capture matching tool.

* Good, because this feature only affects the API the provides candidate captures for matching.
* Bad, because species information is currenly only available in a subset of projects.
* Bad, because not all projets plant multiple species at the same site.

### 7. Train models using machine learning based on a curated set of manually matched captures.

This data set can be created using the manual capture matching tool and specific sets of trees. Additional makers may be added to images of trees such as colored sticks or numbered tags to help in this process.

* Good, because this is a set to work off of and test models against
* Bad, because it is a lot of extra work and expensive to implement on the ground.
* Bad, because it is time consuming to create accurate matching set

### 8. Duplicate detection mode

Same algorythm that is used to detect recaptures is applied on a shorter time span (smaller than 7 days) to detect possible fraud

* Good, it is really needed and requested by several organizations
* Good, improves data quality and adds a vital tool to the dashboard 
* Bad, because it is time consuming to operate ?

## Links <!-- optional -->

* {Link type} {Link to ADR} <!-- example: Refined by [ADR-0005](0005-example.md) -->
* … <!-- numbers of links can vary -->

# How the Treetracker mobile app uploads images once it is multiplatform

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: @Elforama (Mobile Lead), TODO backend owner (Treetracker API)
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2026-10-29

Technical Story: [KMP and CMP Migration Plan, "Image uploads (open decision)"](https://github.com/Greenstand/treetracker-android/blob/kmp/28-module-rules-and-docs/docs/engineering/kmp-cmp-migration.md#image-uploads-open-decision)

## Context and Problem Statement

Today "`ObjectStorageClient` uses the AWS Android SDK with a Cognito identity pool to put images in S3. That SDK has no iOS/KMP equivalent we can call from common code" (plan, "Image uploads (open decision)"). The migration plan calls this "The biggest open decision" (plan, "Library swaps").

[ADR-0022](0022-treetracker-android-kmp-and-ios.md) proposes moving the app to Kotlin Multiplatform and shipping iOS. How should the app upload images so that it works on Android and iOS? One of the options needs a new Treetracker API endpoint, so the backend team has to agree to it. That is why this decision is recorded here rather than only in the Android repo.

Prior art in the org: the existing native iOS app ([treetracker-ios](https://github.com/Greenstand/treetracker-ios)) uses CocoapodsKeys "for AWS identity pool ID's", and `treetracker-rn`, a capture client added to the [treetracker monorepo](https://github.com/Greenstand/treetracker) for local end-to-end testing, uploads to S3 "via the dev Cognito identity pool (unauthenticated), replicating ObjectStorageClient.kt". **Unverified:** whether the production Cognito identity pool is also unauthenticated.

## Decision Drivers <!-- optional -->

* One upload implementation in shared code where possible, matching the plan's goal of "Shared business logic and UI in `commonMain`".
* "Since iOS will ship ... Phases 1 to 3 should already pick KMP-friendly options (for example the upload approach below) rather than Android-only stopgaps" (plan, "Decisions to make before starting").
* Whether a backend change is needed, and who would build and run it.
* Uploads on iOS must work with limited background time: "`BGTaskScheduler` + background `URLSession` for images. iOS gives far less background time than WorkManager" (plan, Phase 4, step 6).

## Considered Options

* Option 1: Interface + per-platform SDK
* Option 2: Pre-signed PUT URLs from the Treetracker API, uploaded with Ktor
* Option 3: Cognito + SigV4 signing by hand over Ktor

## Decision Outcome

Chosen option (proposed, **pending backend sign-off**): "Option 2: Pre-signed PUT URLs from the Treetracker API", with Option 1's interface first and Option 1 as the iOS fallback. This is the plan's recommendation, not a final decision:

> Recommendation: since iOS will ship, ask the backend team about option 2 now, so it can be ready by Phase 4. In Phase 2, wrap today's code behind the `ObjectStorage` interface (option 1's first half) either way. If option 2 isn't possible, fall back to option 1 on iOS.
> (plan, "Image uploads (open decision)")

This ADR cannot be accepted until the backend deciders agree to build the endpoint, or choose Option 1 or 3.

Timing:

* Phases 0 and 1 are Android-internal and do not touch this decision.
* In Phase 2 ("Swap Android-only libraries, still Android-only") today's code is wrapped behind an `ObjectStorage` interface. That step is the same whichever option wins.
* The upload implementation itself is needed by Phase 4 (iOS). The plan asks the backend team "now, so it can be ready by Phase 4".

### Positive Consequences <!-- optional -->

* One shared upload implementation, and "no AWS credentials in the app" (plan, option 2).
* The `ObjectStorage` interface from Phase 2 keeps the decision reversible: Option 1 stays available as a fallback.

### Negative Consequences <!-- optional -->

* Needs a new Treetracker API endpoint, which the backend team builds, deploys and maintains.
* Follow-up decisions required, all **open questions** the plan does not answer:
  * Which backend service hosts the endpoint, and how mobile clients authenticate to it.
  * How long URLs stay valid. Captures are often uploaded long after they are taken, and iOS uploads may run as background `URLSession` tasks, so the app may need to request URLs close to upload time.
  * Whether the S3 bucket and object key layout stay the same, so that images still show up in the admin verify tools as they do today.
  * What happens to the Cognito identity pool once Android moves off it, and whether older app versions in the field still need it.

## Pros and Cons of the Options <!-- optional -->

### Option 1: Interface + per-platform SDK

"`ObjectStorage` interface in `commonMain`; Android keeps the current SDK, iOS uses AWS SDK for Swift / Amplify, bridged in from Swift" (plan).

* Good, because it is the "lowest risk" option (plan).
* Good, because it needs "No backend change" (plan).
* Good, because the org has done it before: `treetracker-ios` used an AWS identity pool on iOS.
* Bad, because there are "two implementations to maintain" (plan).
* Bad, because AWS credentials (a Cognito identity pool) stay in the app on both platforms (implied by contrast with option 2's "no AWS credentials in the app").

### Option 2: Pre-signed PUT URLs from the Treetracker API, uploaded with Ktor

"The backend hands out a pre-signed PUT URL; the app uploads with Ktor" (plan).

* Good, because it is the "cleanest" option (plan).
* Good, because there is "One shared implementation" (plan).
* Good, because there are "no AWS credentials in the app" (plan).
* Bad, because it "Needs a backend endpoint" (plan), and so depends on the backend team's time and agreement.
* Open question: **unverified** whether any Greenstand service already issues pre-signed S3 URLs. A GitHub code search of the Greenstand org on 2026-10-29 for `presigned`, `getSignedUrl` and `createPresignedPost` found nothing.

### Option 3: Cognito + SigV4 signing by hand over Ktor

* Good, because it is "Shared code" (plan).
* Good, because it needs "no backend change" (plan).
* Bad, because "we own the AWS signing code" (plan).
* Bad, because AWS credentials (a Cognito identity pool) stay in the app, as in Option 1.

## Links <!-- optional -->

* Refines [ADR-0022](0022-treetracker-android-kmp-and-ios.md) (KMP/CMP migration and iOS)
* Migration plan: https://github.com/Greenstand/treetracker-android/blob/kmp/28-module-rules-and-docs/docs/engineering/kmp-cmp-migration.md
* Current implementation: https://github.com/Greenstand/treetracker-android/blob/master/app/src/main/java/org/greenstand/android/TreeTracker/api/ObjectStorageClient.kt

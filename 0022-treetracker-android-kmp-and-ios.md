# Move treetracker-android to Kotlin Multiplatform / Compose Multiplatform and ship an iOS app from it

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: @Elforama (Mobile Lead), TODO whoever approves org-level commitments (App Store account, iOS support)
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2026-10-29

Technical Story: [KMP and CMP Migration Plan](https://github.com/Greenstand/treetracker-android/blob/kmp/28-module-rules-and-docs/docs/engineering/kmp-cmp-migration.md) (`docs/engineering/kmp-cmp-migration.md`, on the migration's PR stack), first PR of the stack: https://github.com/Greenstand/treetracker-android/pull/1334

## Context and Problem Statement

The Treetracker Android app is a single Android module: "100% Kotlin, ~25k lines in `app/src/main`, 79 test files", with "100% Jetpack Compose (Material 1), no XML layouts" (plan, "Where we start"). A migration is underway, as a stack of small PRs, to split it into modules and move it to Kotlin Multiplatform (KMP) with Compose Multiplatform (CMP) UI. The plan says the app "is in better shape for this than most Android apps": Room, Navigation 3, Koin, kotlinx.serialization and Compose all have KMP versions, and "The hard parts are the device-facing pieces (camera, location, sensors, background upload, S3) and keeping existing installs' databases and preferences intact. The UI layer is the easy part."

The plan's goals include "An iOS app becomes possible (and, if we want it, shippable) without a rewrite", and it records iOS as decided: "**Decided: structure first, then ship iOS.** ... Phase 4 is committed work that starts once Phase 3 is done, ending in an iOS app in the App Store."

Shipping an iOS app is a commitment for the whole org, not just the Android repo, so it is recorded here. Should Greenstand make the Android app multiplatform and ship an iOS app built from the same code?

Other iOS work already in the org, which the plan does not mention:

* [Greenstand/treetracker-ios](https://github.com/Greenstand/treetracker-ios) is a native iOS app ("This is the iOS app for the TreeTracker open source project"). Its last code change was in August 2023 and its last commit a dependency bump in November 2024. **Unverified:** whether it was ever released on the App Store, and whether Greenstand already has an Apple developer account.
* [Greenstand/treetracker-ios-core](https://github.com/Greenstand/treetracker-ios-core) is a Swift library; last push September 2023.

React Native is used elsewhere in the org, by the wallet team's [treetracker-wallet-app](https://github.com/Greenstand/treetracker-wallet-app), a separate product. This ADR covers only the tree-tracking (planter) app.

## Decision Drivers <!-- optional -->

* Android must keep working exactly as today: "Shared business logic and UI in `commonMain`, with Android behaving exactly as it does today" (plan, "Goals").
* "Small, reviewable modules with one-way dependencies, so builds are faster and features are isolated" (plan, "Goals").
* "Every step ships as its own PR and leaves `master` releasable. No long-lived migration branch" (plan, "Goals").
* An iOS app "without a rewrite" (plan, "Goals").
* Existing users must not lose data: "keeping existing installs' databases and preferences intact" is one of the hard parts (plan, "Where we start").

## Considered Options

* Option 1: Stay Android-only and modularize
* Option 2: KMP/CMP with shared logic and UI, then ship iOS (the plan's choice)
* Option 3: A separate native iOS app
* Option 4: KMP for logic only, native UI per platform

## Decision Outcome

Chosen option: "Option 2: KMP/CMP with shared logic and UI, then ship iOS", because it gets an iOS app "without a rewrite" while each step stays a small PR that leaves `master` releasable, and because the first phases are worth doing even before iOS: "Phases 0 to 3 come first and stand on their own value (modules, faster builds, cleaner boundaries). Phase 4 is committed work that starts once Phase 3 is done, ending in an iOS app in the App Store" (plan, "Decisions to make before starting").

The sequence (plan, "Phases"):

* Phase 0, build foundation: Kotlin DSL, `build-logic` convention plugins, AGP 9.
* Phase 1, "Modularize on Android, no KMP yet".
* Phase 2, "Swap Android-only libraries, still Android-only".
* Phase 3, "Turn modules into KMP, Android target only".
* Phase 4, iOS target. Exit: "iOS app can sign up a planter, capture a tree with photo + GPS, and upload it to the test environment, then ships to TestFlight testers and the App Store."
* Phase 5, clean up.

The plan reports Phases 0 and 1 as done ("Status: Phases 0 and 1 are done ... Next up is Phase 2"). **Unverified:** as of 2026-10-29 that work is still in open, unmerged PRs (#1334 to #1386), so it is done on the stack but not yet on `master`.

### Positive Consequences <!-- optional -->

* Android gets modules and faster, more isolated builds whether or not iOS ships (plan, "Decisions to make before starting").
* One codebase for business logic and UI on both platforms. Only device-facing code is written twice (`core:platform` "androidMain + iosMain implementations", plan, "Target module layout").
* Because iOS is planned from the start, "Phases 1 to 3 should already pick KMP-friendly options (for example the upload approach below) rather than Android-only stopgaps" (plan, "Decisions to make before starting").
* Follow-up decision required: the image upload approach ([ADR-0023](0023-treetracker-image-upload-approach.md)).

### Negative Consequences <!-- optional -->

* **The org takes on an iOS app.** Phase 4 needs an "Apple developer account and bundle IDs per environment, privacy manifest and location/camera/motion usage strings, App Store review notes on background location", a "macOS runner" in CI, and a "TestFlight lane in fastlane" (plan, Phase 4, steps 8 and 9). After release, the iOS app needs ongoing maintenance, user support, iOS testers and App Store review cycles. **Open question:** the plan does not say who owns these after Phase 4, or what they cost.
* iOS will not match Android on background sync: "iOS gives far less background time than WorkManager, so expect a 'sync when app is open' fallback" (plan, Phase 4, step 6), and the risks table says "don't promise WorkManager parity".
* Camera blur detection is Android-specific; "iOS can ship without it at first" (plan, "Risks and how we handle them").
* Size: Phase 4 alone is "4-6 weeks, mostly camera, uploads, background sync, and store setup", "Assuming one developer with agent help" (plan, "Rough size"). Phases 0 to 3 are estimated at roughly 7 to 10 weeks more.
* **Open question:** what happens to `treetracker-ios` and `treetracker-ios-core`? Does this ADR retire them, or does an existing App Store listing get replaced?

## Pros and Cons of the Options <!-- optional -->

### Option 1: Stay Android-only and modularize

Phases 0 to 3 of the plan without Phase 4 (or Phases 0 and 1 only, without the KMP move).

* Good, because Phases 0 to 3 "stand on their own value (modules, faster builds, cleaner boundaries)" (plan, "Decisions to make before starting").
* Good, because it adds no iOS or App Store commitments for the org.
* Bad, because it does not meet the plan's iOS goal.
* Open question: the plan does not estimate how much Phases 2 and 3 are worth without iOS, or whether they would still be done.

### Option 2: KMP/CMP with shared logic and UI, then ship iOS

The plan as written: [KMP and CMP Migration Plan](https://github.com/Greenstand/treetracker-android/blob/kmp/28-module-rules-and-docs/docs/engineering/kmp-cmp-migration.md).

* Good, because most of the stack is already KMP-ready: Room ("KMP since 2.7"), Navigation 3 ("JetBrains publishes Nav3 for CMP"), Koin ("Yes after Koin 4.x"), kotlinx.serialization, and Compose ("Yes, via CMP") (plan, "Where we start").
* Good, because "The UI layer is the easy part" (plan, "Where we start"), so sharing it saves the most duplicated work.
* Good, because each step is a small PR that keeps `master` releasable, and Phases 1 to 3 stay Android-only so "Each swap is independently testable on a phone" (plan, Phase 2).
* Good, because Android data safety is planned for: migration tests for every shipped schema, a prefs migration that reads the old file, and an install-over-old-version check on a real device (plan, "Risks and how we handle them").
* Bad, because several pieces have no KMP version and need per-platform code or a replacement: uploads ("AWS Android SDK S3 + Cognito ... No, needs replacing"), CameraX, location and sensors, WorkManager, Firebase (plan, "Where we start").
* Bad, because some libraries may fall short: "maplibre-compose or CMP Nav3 missing something we use" is a listed risk, handled by spiking each one first and keeping the Android map as a fallback (plan, "Risks and how we handle them").
* Bad, because "Build gets slower with many modules" is a listed risk (plan, "Risks and how we handle them").
* Bad, because of the iOS commitments listed under Negative Consequences.

### Option 3: A separate native iOS app

A Swift / SwiftUI app maintained alongside the Android app. `treetracker-ios` is an earlier attempt at this.

* Bad, because it is the "rewrite" the plan's goal avoids ("An iOS app becomes possible ... without a rewrite", plan, "Goals"): business logic, the database schema and every screen would be written and maintained twice.
* Good, because it needs no change to the Android app.
* Open question: the plan does not compare against this option. Why did `treetracker-ios` stop in 2023, and is anything in it reusable?

### Option 4: KMP for logic only, native UI per platform

Share `core:*` (data, database, network, models) through KMP, and write the UI in Compose on Android and SwiftUI on iOS.

* Good, because the shared data and logic layers are the same as Option 2.
* Bad, because the UI would be written twice, although the plan calls it the easy part to share: "100% Jetpack Compose ... no XML layouts", "The UI layer is the easy part" (plan, "Where we start").
* Open question: the plan does not discuss this option. Would native iOS UI matter enough to planters to justify writing the screens twice?

## Links <!-- optional -->

* Refined by [ADR-0023](0023-treetracker-image-upload-approach.md) (image upload approach)
* Migration plan: https://github.com/Greenstand/treetracker-android/blob/kmp/28-module-rules-and-docs/docs/engineering/kmp-cmp-migration.md
* First PR of the migration stack: https://github.com/Greenstand/treetracker-android/pull/1334
* Existing native iOS app: https://github.com/Greenstand/treetracker-ios

# Woowacourse-Versioning Specification

## Summary

A version specification that defines the `{sprint}.{yearweek}.{baseline}` system with `{domainBuild}`.

This scheme, adapted from [SemVer](https://github.com/semver/semver) and [HeadVer](https://github.com/line/headver) to the practices of Woowacourse, supports sprint-based product development in a monorepo environment with frontend, backend, and mobile teams, ensuring long-term traceability and clarity for services delivered to real end-users.

## Goals

- Enable teams to maintain a consistent versioning rhythm aligned with **two-week sprint cycles**.
- Enable version strings to embed **week-based temporal information** so that release timing is immediately clear.
- Enable independent releases for **frontend, backend, Android, and iOS** in a shared monorepo environment.
- Enable teams to reduce overhead from versioning debates and focus on **delivering value to end-users**.

## Non-Goals

- It is not a goal to support **short-lived or introductory projects (Level 1–2)**; the design is oriented toward longer-running, team-based service projects (Level 3–4).
- It is not a goal to define versioning rules for **library APIs**; this system is intended for service products rather than reusable libraries.
- It is not a goal to introduce or enforce **semantic compatibility guarantees**; the focus is on automation, traceability, and clarity of release history.

## Motivation

Projects at Woowacourse are developed not as short-lived exercises but as **service products intended for real end-users**. These projects run on **sprint cycles**, with a release produced at the end of each iteration. Each release must be **identifiable, reproducible, and traceable** over time.

Conventional versioning practices often disrupt this process. They introduce ambiguity in version assignment and increase the likelihood of management errors. Common questions include:

- *“Is this change a minor update or just a patch?”*
- *“When was version 1.4.3 actually released?”*
- *“Frontend-only change created a backend tag?”*
- *“Did we forget to update the tag before shipping?”*

Such uncertainties hinder coordination in a **shared monorepo** that includes frontend, backend, and mobile domains. A single version stream cannot capture the **independent release flows** of these domains, leading to confusion and redundant tags. Manual tagging further increases the risk of errors, as omissions or inconsistencies can silently propagate into release history.

A versioning system is therefore required that aligns with **sprint cadence**, encodes **temporal context**, supports **domain-level independence**, and minimizes **human error through automation**. This ensures that releases remain both meaningful and traceable in practice.

## Description

### 1. {sprint}

- Serves as the **sprint counter**, manually incremented at the end of each release cycle.  
- In Woowacourse, with **two-week sprints**, `{sprint}` increases every two weeks when a release is delivered.  
- Example: `v12` → the 12th sprint release.  
- Provides a clear indicator of how many sprint-based releases have been delivered so far.  

### 2. {yearweek}

- Serves as the **temporal marker**, automatically generated using the **ISO 8601 week date system**.  
- In Woowacourse, `{yearweek}` aligns naturally with sprint cadence, making it easy to see when a release occurred.  
- Example: `2534` → indicates a release made in **late August 2025**.  
- Provides immediate context of release timing without consulting external logs.  

### 3. {baseline}

- Serves as the **shared baseline tag**, automatically incremented within the same `{sprint}.{yearweek}` whenever a new synchronized release is made.  
- In Woowacourse, `{baseline}` acts as a **checkpoint** where all domains (frontend, backend, mobile) are expected to stay aligned.  
- Example: `v12.2534.7` → the 12th sprint, week 34 of 2025, the 7th baseline in that sprint-week.  
- Provides a synchronization marker that ensures the entire project remains in a **consistent and aligned state**.  

### 4. {domainBuild}

- Serves as the **domain-specific sequence**, automatically incremented under the shared baseline for each independent release.  
- In Woowacourse’s monorepo, `{domainBuild}` allows frontend, backend, Android, and iOS teams to release at their own pace.  
- Example: `v12.2534.7+be.2` → 2nd backend-specific release after the 7th baseline.  
- Provides clarity and traceability by distinguishing independent releases while maintaining connection to the baseline.  

## Examples

- `v8.2420.3+fe.5` → 8th sprint release, 2024 week 20 (mid-May), 3rd baseline, frontend-specific 5th release.  
- `v8.2420.3+an.5` → 8th sprint release, 2024 week 20 (mid-May), 3rd baseline, Android-specific 5th release.  
- `v15.2602.1+ios.1` → 15th sprint release, 2026 week 2 (January), 1st baseline, iOS-specific 1st release.  
- `v12.2534.7+be.2` → 12th sprint release, 2025 week 34 (late August), 7th baseline, backend-specific 2nd release.  

## FAQ

**Q1. The `{sprint}` number is getting too large. Isn’t 99 a bit excessive?**  

A: `{sprint}` is not your age, it’s just your sprint counter. If it’s at 99, that means the team has delivered value to end-users 99 times. In **Woowacourse**, that’s not excessive — that’s proof of consistent effort and something to be proud of.  

**Q2. Do I really need to care about `{baseline}`?**  

A: Yes, at least a little. `{baseline}` is the team’s common checkpoint — the marker that says *“we’re all on the same page.”* In **Woowacourse**, if one teammate is debugging yesterday’s build while another is talking about today’s, confusion is guaranteed. Baseline keeps everyone aligned.  

**Q3. Isn’t `{domainBuild}` optional?**  

A: On paper it looks optional, but in a monorepo it rarely is. Frontend, backend, and Android teams often move at different speeds. `{domainBuild}` helps separate those flows so one team’s release doesn’t accidentally affect another’s. Think of it less as “optional” and more as a practical habit.  

**Q4. Can we add suffixes like `-alpha`, `-beta`, or `-rc`?**  

A: You could, but in **Woowacourse** every sprint release is already treated as a kind of real-world test with end-users. Adding extra suffixes usually adds complexity without much gain. Just bump `{sprint}` and keep the rhythm simple.  

**Q5. What if we forget to tag a release?**  

A: With automation, that shouldn’t happen. But if it ever does, the real problem isn’t missing a tag — it’s the confusion that follows: *“Wait, which version did we deploy yesterday?”* Automation is the easiest way to avoid those messy Slack threads.  

**Q6. Why is Android marked as `an` in the version string, instead of `aos`?**  

A: The Android folks at **Woowacourse**, who really love their craft, aren’t fond of the term `aos`. It’s not official and often causes confusion. That’s why we use `an` instead — short, clean, and consistent with the other abbreviations (`fe`, `be`, `ios`). Of course, if the team prefers, you can just write out `android`. What matters isn’t rigid rules, but keeping things consistent within the team.  

**Q7. Can this versioning system be used outside of Woowacourse as well?**  

A: Absolutely. While it was designed with **Woowacourse** projects in mind, it’s not limited to them. Anyone can adopt it as-is or adapt it for their own context. Think of it as a recipe you can follow or tweak, not a rigid standard.  

## Acknowledgments

- This specification was greatly inspired by [SemVer](https://github.com/semver/semver). Thank you sincerely.  
- It also benefited from the ideas and approach of [HeadVer](https://github.com/line/headver). Deep gratitude.  
- Thanks as well to the peers from [Woowacourse 7th generation](https://www.woowacourse.io/). The activities and learning shared together played a big part in shaping this document, and the FAQ having seven questions is a small shout-out to the 7th gen.  
- Finally, many thanks to every reader for taking the time to read this document. It was written during the Level 3 vacation of the Woowacourse 7th generation program, and still has its rough edges. I will refine it further in Level 4.  

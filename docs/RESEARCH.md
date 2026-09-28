# Market & Rules Research (Sept 2026)

Sources came mostly from search snippets; items marked *(unverified)* had thin sourcing. Not legal advice.

## 1. Competitors

### Screen-time blockers
| App | Mechanic | Bypass | Price |
|---|---|---|---|
| Opal | Scheduled sessions, "Deep Focus" can't be ended | Hardcore mode: 1 break/week; users bypass by revoking Screen Time permission | $99.99/yr (~$10M ARR) |
| one sec | Breathing pause before app opens | Just friction | €14.99/yr. PNAS study: −57% social use |
| ScreenZen | Escalating delays | Wait it out | Free / donations |
| Brick | $59 NFC puck you tap to unlock | 5 "emergency unbricks" | Hardware |
| Bloom | NFC card + garden limits app | — | Subscription |
| Jomo | Schedules, open limits, rules locked for days | Hard mode none | $29.99/yr |
| Clearspace | Push-ups (motion sensor) earn time | Do the exercise | ~$45/yr |
| Roots | Limits + streaks earning "cheat days" | — | $49.99/yr |

### "Earn screen time" apps (crowded, all single-purpose)
- **Exercise:** Pushscroll (1 push-up = 1 min), PushLock, Repscroll, PushUp Time, ScreenFine ($1/wk; 25 push-ups or 1,000 steps).
- **Reading:** Read to Unlock (scan the page, answer a comprehension question; 10 pages = 20 min).
- **Tasks/habits:** LockedIn, EarnScreen, Achieve!
- **Friend unlock:** **LockPact** (a partner holds a 6-digit code and gets alerted if you revoke Screen Time access), AppBlock (partner approves by email), Sheppie. → **Our Keyholder idea already exists.** Our twist is making it one backdoor among several, with escalation and a shame log.

### Commitment / money-on-the-line apps (the "bet on yourself" apps)
- **Beeminder:** escalating pledges $0 → $5 → $10 → $30 → $90 → $270. **The company keeps the money.** There's a 24h check before any charge.
- **stickK:** money goes to a charity, an **anti-charity**, or a friend. A human **referee** you pick confirms results.
- **Forfeit:** you pay only on failure, and **Forfeit keeps it**. Proof via AI-checked photos, human-reviewed timelapses, GPS, Health, Strava, or a friend. Has **Screen Time limit contracts**. Stripe. 20k+ users.
- **Overlord** (YC, same team as Forfeit): an AI "enforcer" that watches GPS, Mac activity, and spending; charges $1–5 per slip; blocks apps; texts friends; offers a paid buyout. *Closest competitor in spirit.*
- **StepBet / DietBet (WayBetter):** a shared pot where winners split the losers' money, and the house takes 15%.
- **Pact / GymPact** — shut down 2017 after a **$1.5M FTC settlement**: it charged people who had actually succeeded because verification was bad, and made cancelling hard. Lesson: **fair, disputable verification matters.**

### Proof methods in the wild
On-device pose counting · HealthKit/wearables · AI photo check (camera only, no camera roll) · AI → human escalation · human referee · GPS geofence · comprehension quiz · NFC hardware · integrations (Strava, RescueTime).

### White space
1. **Proof for knowledge work.** Nobody verifies code (GitHub), LeetCode, study, or sales activity.
2. **Many task types in one app.** Every earn-time app does exactly one thing.
3. **A priced, escalating escape plus a social escape**, in one blocker.
4. Every Screen Time blocker can be turned off in iOS Settings, so it's worth detecting that and punishing it.

## 2. Apple App Store payment rules

| Design | How to charge | Why |
|---|---|---|
| Pay to unlock right now (money → dev) | **In-App Purchase, consumable** | 3.1.1: unlocking features or functionality in the app must use IAP |
| Hall passes (pre-bought unlock tokens) | **IAP consumable** | Same rule. Passes can't expire. |
| Subscription | **IAP** (US: may also link to web checkout) | Standard |
| Stake lost on failure (Forfeit style) | **Stripe, card on file** | A penalty for real-world behavior, not digital content. Uncertain if paying directly lifts a lock. |
| Charity / anti-charity | **Not inside the app** | 3.2.2(iv): only approved nonprofits can collect in-app. You can donate part of your revenue yourself. |
| Venmo to a friend | Fine (a deep link, you never touch the money) | Honor-based, can't verify |

- **Gambling:** a self-controlled goal isn't chance-based, so it isn't gambling (5.3.4). Risk goes up if you **pay out winners from a pot**, which brings in contest rules (5.3.1–5.3.2) and state laws. Avoid "bet / win" language.
- **Apple's cut:** 15% (Small Business Program) or 30% on IAP. After *Epic v. Apple*, US apps can link out to web checkout at **0% commission for now**. A cost-based fee may come later, and the Supreme Court case is still pending.
- **Family Controls entitlement:** apply for distribution per bundle ID, including each extension. Reported waits range from a few days to a few weeks. Pitch it as a self-directed digital-wellbeing tool.

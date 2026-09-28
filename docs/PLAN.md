# Product Plan (v0 — draft)

Working name: **Toll** (you pay a toll to get into the fun stuff). Other options: *Earned*, *Keyholder*, *Bundle*.

## 1. The idea in one line

A screen-time blocker where the fun apps are locked by default, and the key is **doing the thing you're supposed to do** (temptation bundling) — with an emergency backdoor that is deliberately **expensive or embarrassing**.

## 2. Design principle: the Four Laws, inverted

Atomic Habits says good habits should be *obvious, attractive, easy, satisfying*. Bad habits get the inversion. Every feature should serve one of these:

| Law | For the bad habit (scrolling) | For the good habit (the "toll") |
|---|---|---|
| 1. Obvious / Invisible | Apps shielded; blocked icons show a custom screen, not the feed | Shield screen shows *exactly* what task unlocks it |
| 2. Attractive / Unattractive | Shield shows today's wasted minutes and the "cost" of the backdoor | Temptation bundling: the reward is right there after the task |
| 3. Easy / Difficult | Friction: waits, typing, friend codes, money | One tap to start a task; auto-verified where possible |
| 4. Satisfying / Unsatisfying | Backdoor use is logged in a Hall of Shame, streak broken | Streaks, "earned minutes" bank, never-miss-twice tracking |

## 3. Core concepts

- **Temptation** — an app, app category, or website you want to limit (Instagram, TikTok, YouTube, reddit.com…).
- **Toll** — something you must do to unlock a temptation. Pays out *earned minutes*.
- **Bank** — your balance of earned minutes. Spending time in a temptation drains it. When it hits zero, the shield comes back.
- **Rule** — ties temptations to tolls, schedules, and limits. E.g. "Instagram: locked 9pm–7am; otherwise 10 min per 15-min reading session; max 45 min/day".
- **Backdoor** — the escape hatch when you *really* need in. Always available, always painful.

## 4. Feature list

### 4.1 Blocking (the foundation)
- Pick apps / categories / websites to lock (iOS Family Activity Picker).
- Modes per rule:
  - **Earn-to-unlock** (temptation bundling) — locked until you pay a toll.
  - **Daily limit** — plain X min/day, then locked.
  - **Schedule** — hard lock during windows (bedtime, work block, first hour of the day).
  - **Open limit** — max N opens per day (kills the reflexive check).
- Custom **shield screen**: shows the toll needed, current bank, today's usage, and a "backdoor" button that is visually unappealing.
- Website blocking for Safari (via Screen Time web-domain shields).

### 4.2 Tolls (how you earn time)
Ranked roughly by how hard they are to cheat:

| Toll | Verification | Notes |
|---|---|---|
| Steps / workout | HealthKit | Auto. "1,000 steps = 10 min" |
| Focus / study session | In-app timer, fails if you leave the app | Pomodoro-style; ties to "read 20 min" |
| Reading | Timer + photo of the page you ended on | Photo is logged; honor-system-plus |
| Push-ups / squats | Camera + on-device pose detection (Vision) | Fun, very hard to fake |
| Meditation | HealthKit mindful minutes or in-app timer | |
| Go to the gym / a place | Geofence arrival + dwell time | |
| Finish a to-do | Apple Reminders (EventKit) list completion | Honor-system; good for chores |
| Journal / reflection | Write N words in-app | "What will you do after scrolling?" |
| Custom | Honor-system checkbox with a cooldown | Last resort; logged |

Earning rules: exchange rate per toll, **daily earn cap** (so you can't bank 6 hours on Sunday), and optional **bank expiry** (minutes expire at midnight).

### 4.3 Backdoors (expensive or embarrassing)
Each rule picks which backdoors are allowed. Price **escalates** each use in a day/week.

1. **Keyholder codes (friend one-time codes)** — the headline feature.
   - During setup the app shows a QR code **once**; your friend scans it into their authenticator app (Google Authenticator, 1Password, etc.) as e.g. *"Jake's Phone Shame Key"*.
   - To use the backdoor you must text/call that friend and ask for the current 6-digit code. It's standard TOTP, so **no backend, no friend app install**, works forever.
   - Optional "ask" button pre-writes a text: *"It's 11:48pm and I'd like to watch TikTok. May I please have my code? 🙏"*
   - Multiple keyholders; rule can require 1 or 2 of them for nuclear unlocks.
   - v2: server-backed version where the friend gets a link with Approve / Deny / "Deny and roast" buttons.
2. **Pay the toll in cash** — Stripe charge to a charity (or an *anti*-charity you dislike), escalating: $1 → $2 → $5. (Needs a small backend; Apple forbids IAP for this if the money isn't going to digital content, so use a donation flow.)
3. **The Confession** — type a long paragraph verbatim, no paste, typos reset it: *"I, Jake, am choosing to open Instagram instead of…"*. Length grows each use.
4. **The Wait** — 5-minute cooldown that you must stay on the screen for. Doubles each use.
5. **Hall of Shame** — every backdoor use is permanently logged with time, app, and (optional) a selfie taken at the moment of weakness. Weekly recap shows it.
6. **Tattletale** (v2) — auto-sends a message to an accountability friend/group chat when you backdoor.

### 4.4 Feedback & motivation
- Home: bank balance, today's earned vs. spent, streak.
- **Never miss twice** — a streak that tolerates one bad day but not two.
- Weekly recap: minutes reclaimed, tolls completed, shame log.
- Identity framing: "You've been a reader 5 of the last 7 days."

### 4.5 Anti-cheating / commitment
- **Strict mode**: rules can't be loosened or deleted for 24h after you ask (tightening is instant). This is the big thing most blockers get wrong.
- Changing a rule during an active lock requires a backdoor.
- Detect and log when Screen Time permission is revoked (can't prevent it on iOS, but it can break your streak and tattle).

## 5. Platform & tech (proposed)

**iOS first, native SwiftUI.** Only native apps can block other apps on iOS; React Native/Flutter would still need the Swift extensions.

Apple Screen Time API pieces:
- `FamilyControls` — authorization (`.individual`) + app/website picker. Needs the **Family Controls entitlement**; works for development on your own device right away, App Store distribution requires Apple approval (request early).
- `ManagedSettings` — applies the shields (apps, categories, web domains).
- `DeviceActivity` — schedules and usage thresholds (drains the bank, re-locks at 0).
- App extensions: `ShieldConfiguration` (custom shield look), `ShieldAction` (shield button taps), `DeviceActivityMonitor` (threshold callbacks), `DeviceActivityReport` (usage charts).
- Shared state via App Group (SwiftData/UserDefaults).

Known constraints to design around:
- The shield screen is templated (icon, title, subtitle, 2 buttons) — the real toll/backdoor UI lives in the main app; the shield button sends a notification that deep-links there.
- Apps are opaque tokens — you can't read their names in the main app; display uses `Label(token)`.
- Website blocking is Safari/WebKit only; other browsers need to be blocked as apps.
- Usage-based bank draining uses DeviceActivity thresholds (minute granularity), not exact live timing.

Other: HealthKit, Vision (pose), CoreLocation, EventKit, local TOTP (CryptoKit HMAC). Backend only needed for payments and v2 social features (Supabase or Cloudflare Workers).

Android later (UsageStats + Accessibility Service — actually more flexible, but different code).

## 6. Roadmap

**MVP — "works for me" (personal device, no backend)**
- [ ] Screen Time authorization + pick temptations
- [ ] One rule type: earn-to-unlock with bank + daily limit
- [ ] Tolls: in-app focus timer, HealthKit steps
- [ ] Custom shield screen → deep link to app
- [ ] Backdoors: Keyholder TOTP codes, The Wait
- [ ] Shame log + simple stats

**v1**
- [ ] Schedules, open limits, strict mode (24h delay on loosening)
- [ ] Tolls: push-ups (pose), reading w/ photo, Reminders, geofence
- [ ] Backdoors: Confession, escalating costs, selfie in Hall of Shame
- [ ] Weekly recap, never-miss-twice streaks, home screen widget

**v2**
- [ ] Backend: paid backdoor to charity, friend approval links, tattletale
- [ ] Mac companion (same Screen Time APIs on macOS)
- [ ] Shared challenges with friends
- [ ] Android

## 7. Open questions
- iOS only, or do you need Android / Mac too?
- Should earned minutes be global (one bank for all apps) or per-temptation?
- How strict by default — is "delete the app" an acceptable escape, or should strict mode be on from day one?
- App Store eventually, or just sideload for yourself?
- Name?

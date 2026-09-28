# HabitUp — Product Plan (v0.2, still refining)

Working name: **HabitUp** (placeholder, revisit later).
Platform: **iOS only** to start. Built for me first, designed so it *can* ship to the App Store later.

## 1. The pitch

> Instagram unlocks only after 90 minutes of deep work. Bypassing the lock costs you $10 to a cause you hate — or begging a friend for a code.

Existing apps each do one piece:
- **Opal / Bloom / Screen Time** — block apps. Friction is weak (a timer, a "are you sure?").
- **Beeminder / stickK** — money on the line. No connection to your phone.
- **Habit trackers** — log good habits. No reward attached.

HabitUp closes the loop in one app: **do the hard thing → earn the fun thing → cheating costs you real money or real embarrassment.**

The white space:
1. **Earn-to-unlock** (temptation bundling) — some apps stay locked until you finish something real.
2. **Escalating, adaptive friction** — the more you bypass, the more expensive it gets.
3. **Stakes built into the blocker** — money and social accountability live in the lock itself, not a separate app.

## 2. How time works: Allowance + Earned Bank

Two layers:

**Layer 1 — Allowance (per app / group).** Normal screen-time rules you set once:
- Daily limit (e.g. Instagram 20 min/day free)
- Schedules (e.g. nothing social before 9am or after 10pm)
- Allowance can be **0** → the app is "earn-only"

**Layer 2 — Earned Bank (shared across all apps).** When an app's allowance runs out, it draws from one shared bank of minutes you earned by completing tasks.

```
Open Instagram
  ├─ inside a blocked schedule?   → locked (bank can't override; backdoor only)
  ├─ allowance left?              → use allowance
  ├─ earned bank > 0?             → spend from bank
  └─ nothing left                 → locked → earn more, or use a backdoor
```

Open decisions:
- [ ] Can the bank override a schedule block, or are schedules absolute? (Leaning: absolute — bedtime is bedtime.)
- [ ] Does the bank roll over to tomorrow, or reset at midnight? (Leaning: partial rollover, capped.)
- [ ] Daily earn cap so you can't bank 6 hours on Sunday?

## 3. Tasks (how you earn)

Every task has an **exchange rate** (what you do → minutes you earn) and a **proof level**. Better proof earns more.

| Proof level | How it's checked | Earn rate |
|---|---|---|
| **Auto** | Verified by an API or sensor, no input from me | 100% |
| **Proof** | I submit evidence (photo, screenshot, link) | ~75% |
| **Honor** | I tap "done" | ~50%, daily cap |

Candidate tasks:

| Task | Proof | How |
|---|---|---|
| Steps | Auto | HealthKit |
| Workout (gym, run, lift) | Auto | HealthKit workouts |
| Push-ups / squats | Auto | Camera + on-device pose counting |
| Deep work block (e.g. 90 min) | Auto-ish | In-app focus timer; fails if distracting apps are opened |
| Code shipped | Auto | GitHub API: PR merged / commits pushed today |
| LeetCode problem solved | Auto | LeetCode public profile: recent accepted submissions |
| TensorTonic problem solved | Proof | No known public API → screenshot of solved problem (check again later) |
| Pages read | Proof | Photo of the page number you stopped on; or a reading timer |
| Sales calls dialed | Proof → Auto later | Screenshot of dialer / CRM count; later an integration (Salesforce, HubSpot, etc.) |
| Meditation | Auto | HealthKit mindful minutes |
| Custom | Honor | Anything else |

Example exchange rates (all tunable):
- 90 min deep work → 30 min social
- 1 LeetCode medium → 15 min
- 20 pages → 20 min
- 25 sales dials → 20 min
- 5,000 steps → 15 min

## 4. Backdoors (the cost of cheating)

Always available, never free. **Prices escalate**: each use in a rolling 7 days raises the next price, and the price cools off again after clean days.

### 4.1 Keyholder codes ⭐
- At setup the app shows a QR code **once**. A friend scans it into their authenticator app (Google Authenticator, 1Password, etc.) as e.g. *"Jake's Phone Shame Key."*
- To bypass, I have to text the friend and ask for the current 6-digit code.
- Uses the standard authenticator-app code format (TOTP), so there's **no server and the friend doesn't install anything**.
- One-tap ask pre-writes the text: *"It's 11:48pm and I'd like to watch TikTok. May I please have my code? 🙏"*
- Escalation: 1 keyholder → 2 keyholders at the same time.

### 4.2 Cash
- Pay to unlock: $1 → $3 → $10 as it escalates.
- Where the money goes (pick one):
  - **Anti-charity** — a cause I actively dislike (Beeminder / stickK style). Most motivating.
  - Regular charity.
  - **Pay my keyholder friend** (Venmo) — funny, and they'll hold me to it.
- ⚠️ App Store rules on in-app payments for penalties/donations need research before App Store launch. For personal use, a Stripe or Venmo link works fine.

### 4.3 Hall of Shame
- Every backdoor use is logged permanently: time, app, which backdoor, what it cost.
- Optional selfie taken at the moment of weakness.
- Weekly recap: "You paid $13 and bothered Mike twice to watch TikTok at midnight."

### 4.4 Parked ideas
- The Confession (type a long paragraph verbatim, no paste)
- The Wait (cooldown that doubles)
- Tattletale (auto-text an accountability friend or group chat when I bypass)

## 5. Loopholes (not day one)

The easiest way to beat any blocker is to change your own rules or delete the app. Ideas for later, as settings rather than defaults:
- Loosening a rule only takes effect after a delay; tightening is instant.
- Revoking Screen Time permission gets logged to the Hall of Shame.

## 6. Feedback loop

- Home screen: bank balance, allowance left, today's earned vs. spent.
- Streaks with a "never miss twice" rule (one bad day is OK, two in a row breaks it).
- Weekly recap: minutes reclaimed, tasks done, Hall of Shame.

## 7. iOS constraints to design around

- Blocking uses Apple's Screen Time frameworks. That needs the Family Controls entitlement: it works for development on my own phone right away, and App Store distribution needs Apple's approval.
- The screen that covers a blocked app can only be lightly customized. The real "earn / backdoor" UI lives in the main app, and a button on that screen jumps there.
- Website blocking only works in Safari. Other browsers have to be blocked as apps.

## 8. Open questions

- [ ] How much do I trust honor-system tasks? Should they earn at all?
- [ ] Where does cash go by default: anti-charity, charity, or the friend?
- [ ] Do backdoor prices escalate per week, per day, or per app?
- [ ] Should the first version include integrations (GitHub, LeetCode), or just timers + HealthKit + photo proof?
- [ ] Final name.

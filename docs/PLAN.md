# HabitUp — Product Plan (v0.3, still refining)

Working name: **HabitUp** (placeholder).
Platform: **iOS only** to start. Built for me first, designed to ship on the App Store later.
Market + Apple rules research: see [RESEARCH.md](RESEARCH.md).

## 1. The pitch

> Instagram unlocks only after 90 minutes of deep work — proven, not claimed. Getting around the lock costs real money or a humiliating text to a friend.

Where it fits:
- Blockers (Opal, Bloom, one sec) → weak friction, no reward.
- Earn-time apps (Pushscroll, Read to Unlock, ScreenFine) → **one task type each**, mostly exercise.
- Commitment apps (Beeminder, stickK, Forfeit) → money on the line, but separate from your phone.

**HabitUp = many kinds of verified task (including knowledge work nobody verifies: code, LeetCode, sales) → earned time → escape hatches that cost money or pride.**

## 2. Principles

1. **Proof is required.** No honor system. If it can't be proven, it doesn't earn.
2. **Every escape has a price** — money, pride, or time. That covers unlocking apps *and* loosening rules.
3. **Prices escalate** the more you cheat, and cool off when you're clean.
4. **Everything resets at midnight.**
5. **Fair and disputable.** If verification wrongly rejects real work, I need a way to contest it (lesson from Pact's FTC settlement).

## 3. How time works: Allowance + Earned Bank

- **Allowance (per app/group):** daily limit + schedules. Allowance 0 = earn-only.
- **Earned Bank (shared):** minutes earned from tasks; used once an allowance runs out.
- **Schedules are absolute** (bedtime is bedtime), unless I use a backdoor.
- **Everything resets at midnight:** allowance, bank, backdoor escalation for the day.

```
Open Instagram
  ├─ inside a blocked schedule?  → locked (backdoor only)
  ├─ allowance left?             → use allowance
  ├─ bank > 0?                   → spend from bank
  └─ nothing left                → earn, or pay/beg via backdoor
```

## 4. Tasks & proof

Every task needs proof. Proof types:

| Proof type | Examples |
|---|---|
| **Sensor / API** (automatic) | HealthKit steps & workouts (covers Apple Watch, and Garmin/Whoop/Oura via Health), GitHub API, LeetCode profile, in-app focus timer |
| **Camera** (in-app camera only, no camera roll) | Push-up counting via pose detection, book page photo, dialer/CRM screenshot |
| **AI-checked** | Photo or screenshot checked by a vision model; comprehension check for reading |
| **Location** | GPS geofence: arrived at the gym and stayed 45+ min |
| **Human** | A keyholder friend approves ("Mike confirms you went to the gym") |

| Task | Proof |
|---|---|
| Steps / workouts | HealthKit |
| Push-ups / squats | Camera pose counting |
| Gym visit | GPS geofence + dwell time, or HealthKit workout |
| Deep work (e.g. 90 min) | In-app focus session + Screen Time shows no distracting apps opened during it |
| Code shipped | GitHub: merged PR or meaningful commits (AI checks it's not a whitespace commit) |
| LeetCode | Public profile: accepted submissions today |
| TensorTonic | Screenshot of solved problem, AI-checked |
| Pages read | Page photo (page number must be higher than last time) + 1 AI question about what you read, or a 30-sec voice summary |
| Sales calls dialed | Screenshot of dialer/CRM activity, AI reads the count; later a CRM integration |
| Custom | Describe the task; AI proposes what proof it will accept |

## 5. Backdoors (unlocking early)

| Backdoor | Cost | Escalation |
|---|---|---|
| **Keyholder code** ⭐ | Text a friend for their 6-digit code (friend scans a QR once into an authenticator app, no server needed) | 1 friend → 2 friends |
| **Pay the dev** | In-app purchase: $0.99 → $2.99 → $9.99 | Resets at midnight (or weekly — TBD) |
| **Hall pass** | Pre-bought pack, for planned exceptions only (movie night, flight) | Must be scheduled ≥ 24h ahead, so they can't be used on impulse |
| **The Wait** | Free, but slow: stare at a countdown | 5 → 10 → 20 min |

Every use goes in the **Hall of Shame**: time, app, backdoor, cost, optional selfie. Weekly recap.

**Why cash goes to the dev:** Apple requires in-app purchase for anything that "unlocks functionality," and bans charity collection inside the app unless you're an approved nonprofit. Paying the dev via IAP is the cleanest legal path. (Apple takes 15%.) Optional later: donate a share of penalty revenue to a charity from outside the app.

## 6. Guardrails (changing settings)

Same backdoor system, so I can't just edit my way out.
- **Tightening** a rule: instant, free.
- **Loosening** a rule (raise a limit, remove an app, lower an exchange rate, delete a schedule), pick one:
  - Wait 24h (free, slow)
  - Keyholder code
  - Pay the dev
- **Revoking Screen Time permission / deleting the app:** can't be prevented on iOS. Detect it, log it to the Hall of Shame, alert keyholders (LockPact does this), and optionally charge a flat penalty.

## 7. Other ways to beat the bad habit

From Atomic Habits + what works in the market:

- **Intention check** on each unlock: "What are you opening this for?" (one sec's pause cut social use 57% in a PNAS study).
- **Session caps:** max 10 min per session, then re-lock, even with bank left. Stops binges.
- **Open limits:** max N opens/day. Kills reflexive checking.
- **Show the swap:** the lock screen suggests the good habit instead ("5 pages = 5 min. Start?").
- **Make the cost visible:** "You've spent 1h40m on TikTok this week = 3 books."
- **Identity votes:** "Every verified task is a vote for being a reader / engineer / athlete." Track votes, not just streaks.
- **Never miss twice:** streaks that survive one bad day, not two.
- **Weekly contract (Sunday):** set next week's rules while calm; they lock in for the week.
- **Accountability report:** keyholders get an optional weekly summary.
- **Commitment stakes (later):** Forfeit-style — "I owe $20 if I use TikTok > 3h this week." Stripe card on file.

## 8. AI features

1. **Proof verification** — vision model checks photos/screenshots (is this a book page? what number? how many dials?). Flags reused or duplicate images.
2. **Proof of understanding** — for reading/studying: AI asks one question about the page you photographed, or listens to a 30-sec voice summary.
3. **Custom task setup** — "I want to earn time for practicing guitar" → AI suggests proof (a 20-sec clip, a practice timer + audio check).
4. **Natural-language rules** — "No Instagram on weekdays until I've done 90 min of deep work" → rules set up automatically.
5. **The negotiator** — before a backdoor, a quick chat: "It's 11:48pm, what's actually going on?" Urge-surfing, not blocking.
6. **Adaptive friction** — weekly AI review: notices patterns ("you backdoor every night at 11pm") and suggests tighter bedtime rules or cheaper tasks.
7. **Weekly recap** — written summary with Hall of Shame highlights.

Proof requires a backend call to an AI model — the first reason the app needs a server.

## 9. Monetization (for App Store later)

- Subscription (IAP).
- Penalty unlocks + hall passes (IAP consumables).
- Later: commitment stakes via Stripe.
- Honest framing: the dev profits when I slip. Beeminder does this openly; say it plainly.

## 10. Open questions

- [ ] Backdoor escalation resets daily (matches midnight reset) or weekly (hurts more)?
- [ ] Should hall passes exist at all, or are they too easy?
- [ ] What happens if AI rejects a real proof — keyholder override? Manual review?
- [ ] Earn rates: time-for-time (90 min work → 30 min fun) or per unit?
- [ ] Which tasks are in the first version?
- [ ] Final name.

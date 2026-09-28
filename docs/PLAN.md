# HabitUp — Product Plan (v0.4, still refining)

Working name: **HabitUp** (placeholder).
Platform: **iOS only** to start. Built for me first, designed to ship on the App Store later.
Market + Apple rules research: see [RESEARCH.md](RESEARCH.md).

## 0. Decisions so far

| Topic | Decision |
|---|---|
| Platform | iOS only |
| Earned time | One shared bank + per-app allowances/schedules. **Resets at midnight.** |
| Earning | **Per unit**, weighted by difficulty ("proof I did something hard") |
| Proof | **Required.** No honor system. |
| Verification | **Proof recipes**: each person builds how their task is verified from building blocks |
| Backdoor pricing | **Escalates over the week**, resets Monday 00:00 |
| Cash | Goes to the dev via in-app purchase |
| Hall passes | **Paid only**, booked ahead |
| AI rejects real proof | **Appeal → access now, keyholder judges later; denied appeal = time taken back** |
| Settings changes | Loosening costs a backdoor (wait 24h / keyholder / pay) |
| First tasks | Reading, workouts, deep work, GitHub, LeetCode/TensorTonic |

## 1. The pitch

> An accountability app that adapts to you. You decide what "I did the hard thing" looks like and how it's proven. Your fun apps stay locked until you prove it.

Every other earn-time app hard-codes one task (push-ups, *or* steps, *or* reading). HabitUp lets you **define the task and the proof**, and the blocker enforces it. It also verifies knowledge work nobody else does: code, LeetCode, deep work.

## 2. How time works

- **Allowance (per app/group):** daily limit + schedules. Allowance 0 = earn-only.
- **Earned Bank (shared):** minutes from tasks, spent once an allowance runs out.
- **Schedules are absolute** unless I use a backdoor.
- **Two clocks:**
  - **Daily (midnight):** allowance and bank reset.
  - **Weekly (Monday):** backdoor prices reset.
- **Exception — debt:** time owed from a failed appeal carries over until it's paid off (see §6).

## 3. Proof recipes (verification you build yourself)

**Is it possible?** Yes, with one constraint: Apple doesn't allow apps to download new code, so we can't generate a new app per person. Instead, verification is made of **building blocks**, and a user's setup is just a **recipe** (data, not code) that snaps blocks together. That's allowed, and it feels like the app was built for you.

### Building blocks
| Block | What it checks |
|---|---|
| Health | HealthKit: workout type, duration, heart rate, steps, calories (Apple Watch, plus Garmin/Whoop/Oura via Health) |
| Camera photo | Live photo in the in-app camera (no camera roll), checked by AI against a rubric |
| Camera video | Short clip, e.g. rep counting with pose detection |
| Voice | Voice memo, AI checks it (e.g. "summarize what you read") |
| Timer | In-app focus session; fails if you open blocked apps |
| Location | GPS geofence + minimum time there |
| Account | API check: GitHub, LeetCode, Strava, etc. |
| AI quiz | AI asks a question based on your proof |
| Keyholder | A friend confirms |

### Recipes
A recipe = **which blocks** + **pass rules** + **unit and rate**.

Examples:
- **Workout:** Health workout ≥ 30 min **and** average heart rate ≥ 120 → 1 unit per 30 min = 20 min earned.
- **Workout (no watch):** gym geofence ≥ 45 min **and** a gym selfie.
- **Reading:** page photo (page # higher than last time) **and** AI quiz on that page → 1 min per page.
- **Deep work:** 45-min focus timer **and** no blocked apps opened → 15 min per block.
- **GitHub:** merged PR or commits today, AI checks it's real work (not a whitespace change) → 20 min per PR.
- **LeetCode:** accepted submission on public profile → Easy 5 / Medium 15 / Hard 30 min.
- **TensorTonic:** screenshot of solved problem, AI-checked → 15 min per problem.

### Building a recipe
- Describe the task in plain English: *"I want workouts to count, but only real ones."*
- AI proposes a recipe: *"Apple Watch workout, 30+ min, average HR over 120. OK?"*
- You tweak and save.
- **Changing a recipe to be easier counts as loosening a rule** (§7). Making it harder is free.

### Difficulty-weighted units
Default rates reward hard things more (LeetCode Hard > Easy, longer workouts > short). AI can suggest rates; I can tune them. The goal is "proof I did something hard that's good for me," not busywork.

## 4. Backdoors

Prices climb **each use during the week** and reset **Monday 00:00**.

| Backdoor | 1st use | 2nd | 3rd | 4th+ |
|---|---|---|---|---|
| **Keyholder code** (text a friend for their 6-digit code) | 1 friend | 1 friend | 2 friends | 2 friends |
| **Pay the dev** (in-app purchase) | $0.99 | $2.99 | $4.99 | $9.99 |
| **The Wait** | 5 min | 10 min | 20 min | 40 min |

Every use goes in the **Hall of Shame** (time, app, backdoor, cost, optional selfie) and the weekly recap.

### Hall passes (paid, planned)
- For planned exceptions: movie night, a flight, a Sunday game.
- **Paid** (in-app purchase), **booked ≥ 24h ahead**, fixed time window.
- Cheaper than an impulse unlock (planning ahead is rewarded) and doesn't raise backdoor prices.
- Cap: 2 per week.

## 5. Emergencies

**Is there a real reason to need Instagram or Clash of Clans urgently?** Almost never. Real emergencies need Phone, Messages, Maps, email, banking, rideshare, and 2FA apps — and those should **never be blockable**. Emergency calls always work on iOS regardless.

Plan:
1. **Always-allowed list** — Phone, Messages, Maps, Wallet/banking, authenticator apps, rideshare. Can't be locked.
2. **Real-but-rare cases** (a DM from someone only reachable on Instagram, event details, a Marketplace sale):
   - **Emergency pass**: 1 free per week, **5 minutes**, only after writing a reason.
   - The reason goes in the weekly recap, and keyholders see it.
   - A 2nd one that week costs the normal backdoor price.

## 6. Appeals (when AI rejects real proof)

1. AI rejects the proof → I tap **Appeal** and add a note.
2. **Time is granted immediately** (provisional). I'm not blocked while waiting.
3. The proof + my note go to a **keyholder**, who approves or denies (within 48h).
4. **Approved** → time stays; AI learns from it (the example is added to my recipe).
   **Denied** → the time becomes **debt**, taken from future earnings (next task earnings go to debt first), + a Hall of Shame entry.
   **No response in 48h** → approved.
5. Abuse guard: after 2 denied appeals in a week, appeals are no longer granted up front — you wait for the keyholder.

## 7. Guardrails (changing settings)

- **Tightening** (lower limits, harder recipes, more apps): instant.
- **Loosening** (raise limits, easier recipes, remove apps/schedules): wait 24h, keyholder code, or pay.
- **Revoking Screen Time permission / deleting the app:** can't be prevented on iOS. Detect it, log to the Hall of Shame, alert keyholders.

## 8. Other ways to beat the bad habit

- **Intention check** on each unlock: "What are you opening this for?"
- **Session caps:** max 10 min per session, then re-lock.
- **Open limits:** max N opens/day.
- **Show the swap:** the lock screen offers the good habit ("1 LeetCode medium = 15 min. Start?").
- **Make the cost visible:** "1h40m on TikTok this week = 3 LeetCode hards."
- **Identity votes:** "12 votes for being an engineer this week."
- **Never miss twice** streaks.
- **Sunday contract:** set next week's rules while calm; locked in for the week.
- **Weekly keyholder report** (optional).

## 9. AI features

1. **Recipe builder** — plain English → a proof recipe (§3).
2. **Proof checker** — photos, screenshots, voice, commits, against the recipe's rubric; flags reused images.
3. **Proof of understanding** — quiz on the page you read.
4. **Natural-language rules** — "No Instagram on weekdays until 90 min of deep work."
5. **The negotiator** — short chat before a backdoor: "It's 11:48pm, what's going on?"
6. **Adaptive friction** — weekly review of patterns, suggests tighter rules or rate changes.
7. **Weekly recap** with Hall of Shame highlights.

AI checks and keyholder appeals need a **backend** (AI calls, sending proof to a friend).

## 10. Monetization (later)

- Subscription (in-app purchase).
- Penalty unlocks + hall passes (in-app purchase consumables).
- Later: Forfeit-style stakes via Stripe.

## 11. Open questions

- [ ] How does a keyholder review an appeal: web link (no app needed) or text with photo? Leaning web link.
- [ ] Keyholders for multiple people later (my friend holds my key, I hold theirs)? That's a social/viral loop.
- [ ] Per-unit starting rates — tune after a week of real use.
- [ ] Final name.

# HabitUp — Feature Synopsis

**What it is:** An iOS app that locks distracting apps (Instagram, TikTok, games, etc.) until you prove you did something hard and good for you. It's inspired by *Atomic Habits*, especially temptation bundling. Getting around the lock costs real money or real embarrassment. Built for myself first, with a possible App Store launch later. Working name: HabitUp (not final).

**Positioning:** Blockers like Opal and Bloom have weak friction. Earn-screen-time apps (Pushscroll, Read to Unlock) each hard-code one task, and commitment apps (Beeminder, Forfeit) are separate from your phone. HabitUp combines all three:
- Verified tasks, including knowledge work nobody else verifies (code, LeetCode, deep work)
- Earned screen time
- Escape hatches that cost money or pride

## 1. Screen time model
- **Allowance per app or group of apps:** a daily limit plus schedules (e.g. no social media after 10pm). An allowance of 0 means the app only opens with earned time.
- **Earned Bank:** one shared pool of minutes earned from tasks, spent once an app's allowance runs out.
- **Schedules are absolute.** Earned time can't override bedtime; only a backdoor can.
- **Allowance and bank reset at midnight.** Backdoor prices reset weekly (Monday). Debt from a failed appeal carries over until it's paid off.
- **Never lockable:** Phone, Messages, Maps, banking/Wallet, authenticator apps, rideshare.

## 2. Tasks and proof ("proof recipes")
- **Proof is always required.** There is no honor system.
- **Building blocks for proof:**
  - Apple Health data (Apple Watch, plus Garmin/Whoop/Oura via Health)
  - In-app camera photo checked by AI (no camera roll)
  - Short video clip with pose/rep counting
  - Voice memo checked by AI
  - In-app focus timer that fails if you open a blocked app
  - GPS location plus time spent there
  - Account/API checks: GitHub, LeetCode
  - An AI quiz question
  - A friend confirming it
- **Recipes:** each user combines blocks into their own verification rules for a task, with an earning rate per unit. You describe the task in plain English, AI proposes a recipe, and you edit it. Recipes are data, not downloaded code, so Apple allows them.
- **Harder things earn more.** First tasks and default rates:
  - **Workout:** Apple Health workout of 30+ min with average heart rate ≥ 120 (or gym GPS + selfie). About 20 min per 30 min of workout.
  - **Reading:** photo of the page, where the page number must be higher than last time, plus an AI question about that page. 1 min per page.
  - **Deep work:** 45-min in-app focus block with no blocked apps opened. 15 min per block.
  - **GitHub:** merged pull request or real commits, with AI rejecting trivial ones. 20 min per pull request.
  - **LeetCode:** accepted submission on your public profile. Easy 5 / Medium 15 / Hard 30 min.
  - **TensorTonic:** AI-checked screenshot of a solved problem. 15 min.
  - Rates get tuned after a week of real use.

## 3. Backdoors (unlocking without earning)
Prices escalate with each use during the week and reset Monday at midnight.

| Use this week | Keyholder approval | Pay the dev (in-app purchase) | The Wait (free) |
|---|---|---|---|
| 1st | 1 friend | $0.99 | 5 min countdown |
| 2nd | 1 friend | $2.99 | 10 min |
| 3rd | 2 friends | $4.99 | 20 min |
| 4th+ | 2 friends | $9.99 | 40 min |

- **Hall passes:** paid, booked at least 24 hours ahead, for planned exceptions like movie night or a flight. Cheaper than an impulse unlock, capped at 2 a week, and they don't raise backdoor prices.
- **Emergency pass:** 1 free per week, 5 minutes, and you must write a reason. Keyholders see it. A second one that week costs the normal backdoor price.

## 4. Keyholders (friends as the lock)
- A keyholder needs **no app and no account**. Everything happens through a one-time web link.
- **How a request works:**
  1. I tap "Ask Mike."
  2. My own Messages app opens with an embarrassing pre-written text and a link.
  3. Mike opens the link and sees the request and context: the time, my usage this week, my backdoor count, and my proof for appeals.
  4. He taps **Approve**, **Deny**, or **Deny + roast**. A roast message appears on my lock screen and is saved to the Hall of Shame.
  5. My phone gets the answer within seconds.
- Links expire after 1 hour for unlocks and 48 hours for appeals.
- **Offline fallback:** a 6-digit authenticator code the friend scanned from a QR code at setup.
- **Key swaps and key rings:** friends hold each other's keys, in pairs or groups of 3–6. There's a weekly ring recap (most earned, most paid, most begged). Every keyholder page ends with an invite ("Want Jake to hold your key?"), which is how the app spreads.
- **Rubber-stamping and annoying friends:**
  - Each keyholder's approval rate is shown.
  - Bigger requests need 2 keyholders.
  - No approving your partner's request within an hour of asking them for one ("I'll approve yours if you approve mine").
  - Keyholders set quiet hours; requests sent then wait.
  - Max 3 requests per keyholder per week.
  - Removing a keyholder counts as loosening a rule.

## 5. Appeals (when AI wrongly rejects proof)
1. I appeal with a note, and **get the time immediately**.
2. A keyholder reviews it through the web link within 48 hours.
3. The outcome:
   - **Approved:** I keep the time, and the example is added to my recipe so AI accepts it next time.
   - **Denied:** the time becomes debt that my next earnings pay off first, plus a Hall of Shame entry.
   - **No response:** counts as approved.
4. After 2 denied appeals in a week, I have to wait for the keyholder before getting time.

## 6. Changing settings
- **Tightening** a rule (lower limits, harder recipes, more locked apps) is instant and free.
- **Loosening** a rule costs one of: a 24-hour wait, keyholder approval, or paying the dev.
- **Revoking the app's Screen Time access or deleting the app** can't be prevented on iOS. The app detects it, logs it to the Hall of Shame, and alerts keyholders.

## 7. Accountability and motivation
- **Hall of Shame:** a permanent log of every backdoor use, with time, app, method, cost, optional selfie, and any roasts.
- **Weekly recap:** minutes earned and spent, tasks done, Hall of Shame highlights, and the cost made visible ("1h40m on TikTok = 3 LeetCode hards").
- **"What are you opening this for?"** check before each unlock.
- **Session caps:** max 10 minutes per session, then the app re-locks.
- **Open limits:** max number of opens per day.
- **The lock screen suggests the good habit** ("1 LeetCode medium = 15 min. Start?").
- **Identity votes:** "12 votes for being an engineer this week."
- **"Never miss twice" streaks:** one bad day is OK, two in a row breaks the streak.
- **Sunday contract:** set next week's rules while calm, and they're locked for the week.
- Optional weekly report sent to keyholders.

## 8. AI features
1. Recipe builder: plain English in, verification recipe out.
2. Proof checker for photos, screenshots, voice and commits, including detecting reused images.
3. Proof of understanding: a quiz on what you read.
4. Rules from plain English ("No Instagram on weekdays until 90 min of deep work").
5. The negotiator: a short "what's actually going on?" chat before a backdoor.
6. Adaptive friction: a weekly review of patterns that suggests tighter rules or rate changes.
7. A written weekly recap.

## 9. Tech and business constraints
- **iOS only**, native SwiftUI, using Apple's Screen Time frameworks (FamilyControls, ManagedSettings, DeviceActivity).
- Needs Apple's Family Controls permission to distribute. It works on my own phone without it.
- **Limits of Apple's lock screen:**
  - The screen that covers a blocked app can only be lightly customized, so the real screens live in the main app.
  - Website blocking only works in Safari.
  - Users can always revoke the app's access in Settings.
- **A small server is needed** (e.g. Supabase or Cloudflare) for accounts, keyholder links and pages, AI proof checks, push notifications, and short-lived proof storage.
- **Payments:**
  - Pay-to-unlock and hall passes must use Apple in-app purchase (guideline 3.1.1; Apple takes 15%).
  - Charity donations can't be collected inside the app (3.2.2(iv)), so penalty money goes to the developer.
  - A subscription comes later.
  - Commitment stakes in the style of Forfeit could come later via Stripe.
  - Avoid "bet/win" wording and never pay out winners from a pot, to stay clear of gambling rules.

## 10. Still open
- Final name
- First real keyholders for testing
- Scope of the first version and build order

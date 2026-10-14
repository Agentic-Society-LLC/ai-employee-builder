# Routines and triggers

A **routine** is a job that runs on a schedule (the engineers' word is a cron job). A **trigger** is a job that runs when something happens (an event, a webhook). An AI employee with neither is a chatbot you still have to open. The first employee gets one routine; triggers come once that routine has produced reviewed output.

## Routines

### The schedule in plain words, then in five numbers

Ask for the schedule in words ("every weekday at 7am Central") and write both the words and the cron expression into ROUTINES.md. Five fields, separated by spaces: minute, hour, day of month, month, weekday. An asterisk means "every".

| You want | You write | Reads as |
|---|---|---|
| Every weekday at 7am | `0 7 * * 1-5` | minute 0, hour 7, Monday to Friday |
| Every day at 6am | `0 6 * * *` | minute 0, hour 6, every day |
| Every Monday at 6am | `0 6 * * 1` | Monday only |
| Every hour on the hour | `0 * * * *` | minute 0 of every hour |
| Every 15 minutes | `*/15 * * * *` | every 15th minute |
| First of the month, 9am | `0 9 1 * *` | day 1, hour 9 |
| Twice a day, 8am and 2pm | `0 8,14 * * 1-5` | hours 8 and 14, weekdays |

Platforms that take natural language ("every weekday morning at 7") still need the time zone stated. Write it.

### The trap: your 6am becomes 7am twice a year

Almost every cloud scheduler runs on UTC. The CEO's clock changes twice a year; UTC does not. A job pinned to one UTC hour silently drifts by an hour every spring and autumn, with no error.

- If the platform accepts a time zone (`America/Chicago`), set it and **test it across a DST boundary** before trusting it (change the system date on a test, or check the run record's local hour the week after the change).
- If the platform only takes UTC, register the job **twice** (the standard-time hour and the daylight-time hour) and have the job check the local hour on waking and do nothing on the wrong one. Record which expression fired.
- Never write an offset like `UTC-5` anywhere. Read the zone from a real time-zone library at run time.
- Note in the runbook the local time the job must hold and which DST state you assumed when you set it.

### Test it in three minutes, not tomorrow

- Ask the platform for a manual trigger ("run now") and fire it once. Look at the run record.
- Or set the schedule to every 5 minutes, watch two runs land, then set it back.
- Check the record says which schedule fired and what local hour it believed it was.
- A schedule typed into a dashboard and forgotten is a schedule nobody can find. Keep it where the employee's files are (ROUTINES.md) so "what is scheduled, and when?" has one answer.

### What a routine must do every time (the shape of a run)

In this order. The order is the safety property, not a style choice.

1. Check the stop button. If set, record "parked" and do nothing.
2. Confirm it is the right local time slot (the DST guard).
3. Claim the period (this day, this week, this event) in storage that outlives the run, **before** anything leaves. A second run that finds the claim does nothing and says why.
4. Read the one source. If it cannot be read, say "unreadable", never "nothing new".
5. Do the work. Code for anything with a right answer, the model for judgement.
6. Deliver once, through one path, and verify it landed.
7. Write the run record: id, time, outcome (ok / failed / parked), what it read, what it sent, what a human should do if anything.

On the simple platforms most of this is handled for you or is not available; keep whatever the platform offers (a run log, a pause switch) and write the rest into the job's skill file as instructions the employee follows. On Hermes and OpenClaw the controls are real code; see `go-live.md`.

## Triggers

### The catalog

Pick from these or write one in the same shape: **when** (the event, from which system) → **then** (the action) → **approval** (goes to a human first, or acts on its own once promoted).

| When | Then (examples) | Typical seat |
|---|---|---|
| Stripe records a new customer or subscription | draft the welcome email and the onboarding checklist; log the client; notify the owner | Onboarding Agent |
| Stripe payment fails or a subscription is cancelled | draft a check-in or save message; flag the client at-risk | Retention Agent, CFO |
| A sales call ends (Fathom, Zoom, Granola transcript lands) | summarise, score, write coaching notes and the next step; file it in the deal | Sales Coach |
| A website form or calendar booking is submitted | enrich the lead, draft the first reply, set the owner and next step | Rev Ops, Inbound Inquiry Analyst |
| An email arrives from a client or a VIP | triage, draft a reply for approval | Client Services Rep, Executive Assistant |
| A message contains "cancel", "refund", "unhappy" | escalate with one action; draft a response | Client Services, Retention |
| A meeting ends (any transcript lands) | turn it into tasks with owners and dates | Project Manager |
| A task passes its due date | nudge the owner; add it to the morning digest | Project Manager |
| A file lands in a folder (recording, receipt, contract) | process it: transcribe, categorise, file | Bookkeeper, Social Media Manager |
| A bank transaction over a threshold posts | categorise and add to the "ask the owner" list | Bookkeeper |
| An ad set exceeds its cost ceiling for N days | write the kill/scale recommendation | Meta Ads Manager |
| A client has not logged in or attended for 14 days | draft a check-in; add to the at-risk list | Retention Agent |
| A post crosses a reach threshold | add it to the "repeat these" list; draft the follow-up post | Social Media Manager |
| A KPI falls below its floor two periods running | write the diagnosis and three options | CMO, CFO |

### How triggers reach the employee

Three mechanisms, from simplest to most capable. `connections.md` has the wiring detail.

1. **Polling routine.** The employee checks the source on a schedule ("every 15 minutes, look for new customers since the last check") and acts on what is new. Works on every platform, needs no webhook, and the period claim handles duplicates. Start here.
2. **Platform-native trigger.** The platform listens for the event itself (an inbox watcher, a Slack mention, a Telegram message, a folder watcher). Use it when it exists.
3. **Webhook.** The source system (Stripe, the form tool, the CRM, Zapier or Composio in front of them) calls a URL when the event happens, and the employee's runtime receives it. The most immediate and the most work; on Hermes and OpenClaw this is a real endpoint, and it must arm the same guards as the scheduled path (stop button, claim per event, audience, action ceiling).

### Rules that keep triggers safe

- Every trigger names its **approval**: human first, or promoted to act alone. New triggers start human-first.
- Every trigger has a **claim per event** (the Stripe customer id, the call id, the message id) so a replayed event does not act twice.
- Every trigger states its **max actions per event** (usually one) and its widest audience (usually team).
- A trigger that would reach a customer directly ("reply to the inquirer") is not built until the employee is promoted; until then the action is "draft for approval".
- Write each trigger as one row in `triggers.md` and one skill file if the action is more than a few steps.

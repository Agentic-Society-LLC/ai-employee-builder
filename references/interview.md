# The interview: question bank, follow-ups, defaults

SKILL.md holds the rules (one question per message, plain language, "I don't know" is fine). This file holds the questions. Adapt the wording to the person; keep the intent. Skip any question an earlier answer already covered, and say so ("you already told me who approves, so skipping that").

Each question lists **why it is asked**, the **follow-up** when the answer is vague, and the **default** to state if the CEO does not know. A default is written into the contract as ASSUMED and shown in the playback; it is never silently applied.

Timing: about 34 questions in total (Phase 0: 5, Phase 1: 5, Phase 2: 5, Phase 3: 8, Phase 4: 4, Phase 5: 7). Most people answer one in about a minute. The tracker shows "about N minutes left" from the questions still to ask, so set those totals at the start, adjust `questionsTotal` for Phases 3 and 4 as the number of jobs and tools becomes clear, and keep `questionsDone` current. One question per turn means one question mark per turn: where a follow-up below needs two answers, it is two turns.

Before Phase 0, **look before you ask.** If you are inside the CEO's folder (a company brain, a repo, a Drive), read what is there first: a README, a brand or voice doc, an org chart, a tools list, existing agents. Every fact you find is a question you do not ask. Say what you found in one line and confirm it, rather than asking from scratch.

---

## Phase 0: You and the company (5 questions)

**0.1 Who are you and what is the company?** Name, company, what you sell, who buys it, roughly how big (team size, revenue band if they volunteer it). One answer usually covers all of it.
- Follow-up: "What do people actually pay you for, in one sentence?"
- Default: none. This one is required.

**0.2 What is your role, and what does your week look like?** You are listening for where the CEO's time goes and what they wish they could hand off. This often answers Phase 1 early.
- Follow-up: "What is the thing you keep doing yourself that someone else should own?"

**0.3 Who is on the team today, humans and AI?** Names and roles. This is the org chart the new employee joins, and it tells you who the approver, the alert reader, and the handoff partners are.
- Default: "Just you" is a complete answer.

**0.4 How do you sound?** Three words for the brand voice, and a sample if they have one (a recent email, a post, a page). If a voice doc exists in the folder, quote it back instead of asking.
- Follow-up: "Who is a writer or brand whose tone you would steal?"
- Default: "plain, warm, direct" and the employee copies the CEO's own messages in the interview as the sample.

**0.5 What do customers say when they thank you?** The outcome in the customer's words. This becomes the employee's north star and goes into USER.md.
- Default: skip; the outcome from 0.1 stands in.

---

## Phase 1: The role (5 questions)

**1.1 What is the job you want off your plate, or the gap nobody is filling, and who does it today?** The starter question. Most CEOs answer with a task ("post on Instagram", "do the books"). Your job is to lift it to a seat.
- Follow-up: "If that task were done perfectly every week, what would be different in the business?" The answer is the outcome.

**1.2 Which seat does that belong to?** Offer the org chart: Marketing (AI CMO, AI Social Media Manager, AI Meta Ads Manager, AI Email Marketer), Operations (AI Project Manager, AI CFO, AI Bookkeeper, AI Executive Assistant), Sales (AI Sales Coach, AI Rev Ops, AI Inbound Inquiry Analyst), Client Success (AI Client Services Rep, AI Onboarding Agent, AI Retention Agent). Recommend one and say why, then let them pick. **Hire for the role, not the task.** A CEO who wants "someone to post on Instagram" is hiring an AI Social Media Manager; a CEO who wants "the books done" is hiring an AI Bookkeeper, and may want an AI CFO above it later.
- Follow-up: if it spans two seats, pick the one where the first job lives and note the other as a future hire.

**1.3 What is the one-sentence business outcome?** "So that ___." Not a job description: a result. Read the profile's outcome from `role-library.md` as a draft.
- Default: the profile's outcome, marked ASSUMED.

**1.4 How will you know it is working? Two or three numbers.** The KPIs. Offer the profile's list; keep the ones the CEO already tracks or could.
- Follow-up: "Which of those do you look at today, and where?"
- Default: the first two of the profile's KPIs that the employee will be able to measure with the access it is given (an employee with no Instagram access cannot report reach); otherwise a count the owner can keep by hand, such as drafts approved per week.

**1.5 Who owns this employee?** The human whose name goes on it, who approves its work, and who it reports to. Usually the CEO at first.
- Default: the CEO.

---

## Phase 2: The dream hire (5 questions)

Read the matching profile in `role-library.md` before this phase. Then present it as a draft résumé in three or four lines and ask:

**2.1 Here is who I would recruit for this seat. What would you change?** Where they worked, what they read, how they think. The CEO edits; you keep what they keep.
- Follow-up: "Is there a book, a mentor, or a company whose way of doing this you want copied?"
- Default: the profile as written, marked ASSUMED.

**2.2 What does great look like, and what does mediocre look like, for this job at your company?** One example of each. This is the most useful answer in the interview: it becomes the pass and fail examples in the job's skill file and in the evals.
- Follow-up: "Think of the best person you ever saw do this. What did they do that others did not?"

**2.3 What should it never assume, and always ask?** The judgement boundary. Example: "never assume a client is unhappy from one short message; always ask me before flagging a client as at-risk."
- Default: the profile's never-does list, marked ASSUMED.

**2.4 What do you want to call it?** Offer three names from the profile and one built from what it does. The CEO will be talking to it and about it for a long time. The name goes on every output.
- Follow-up: "Say its name in a sentence to your team. Does it fit?"

**2.5 Listen to it introduce itself.** Write the self-introduction in the employee's own voice, two to four sentences: name, title, what it owns, how it works, what it will always check with the CEO. Ask: "Is that the hire?" Revise until yes. This line goes at the top of SOUL.md, into the tracker's `employee.intro`, and is the first thing the employee says on every platform.

---

## Phase 3: Jobs, routines, triggers (8 questions: 3.1, then 3.2 to 3.5b for the first job in full, 3.6, and 3.7 covering the other jobs)

Only the first job gets the full 3.2 to 3.5b treatment. Raise `questionsTotal` if the CEO wants a second job in full now.

**If the role's first job is event-driven** (the Sales Coach's call review, the Onboarding Agent's new-customer packet, the Project Manager's meeting-to-tasks), do not force it into a schedule and do not park it as "a trigger for later". Ask 3.2 as "what is the event that starts it?", build it as a **polling routine with a claim per event** (`routines-and-triggers.md`, mechanism 1: every N minutes, look for new items since the last check, act once per item), and show the polling interval as ASSUMED in the playback. The first-job test still holds: one event source, one output, one declared destination.

**3.1 What are the recurring jobs this seat owns?** Three to six. Offer the profile's list. Mark one as the **first job**: the one that passes the first-job test (one schedule or event, one source, one output, one human).
- Follow-up: "Which one would you want done tomorrow morning if you could only have one?"

For each job, starting with the first job:

**3.2 When does it happen?** A schedule ("every weekday at 7am", "Mondays at 6", "first business day of the month") or an event ("when a sales call ends", "when Stripe gets a new customer", "when a form is submitted"). Get the local time and the time zone. Note whether it must hold that local time across daylight saving; almost always yes.
- Follow-up for "whenever": "If it ran at the same time every day, what time would you want to see the result?"
- Default: weekdays at 7am local, marked ASSUMED.

**3.3 Where does the information come from?** Name the system and ask for one real example ("show me one record", "forward me one of these emails"). "Our CRM" is not a source; "HubSpot, the Deals pipeline, stage Discovery and later" is.
- Follow-up, only if they named several places: "Which one of those is the truth when they disagree?" The first job reads one source. Extra sources go to the second job.

**3.4 What does the finished thing look like, and where does it land?** One output (a brief, a draft, a list, a filled record) in one destination (an email to the CEO, a Slack channel, a Google Doc, a draft in the email tool). Ask for or sketch an example output and read it back.
- Default: a one-page brief emailed to the owner, marked ASSUMED.

**3.5 Who checks it before it counts as done, and what do they do when it is wrong?** The approver and the correction path. "Nobody" is an answer that raises the risk tier; say so.
- Default: the owner approves by replying; a wrong output gets a one-line correction that the employee records as a lesson.

**3.5b If it accidentally ran twice one morning, what would go wrong?** Its own turn. The answer decides the claim window (one run per day, per week, per event) without using the word. "The rep gets two notes and stops trusting it" means a claim per call; "two emails, annoying" means a claim per day.
- Default: one run per period of the schedule, or one per event, marked ASSUMED.

**3.6 Is there an event that should start something, even when nobody asks?** The triggers beyond the first job. Offer the profile's typical triggers and the catalog in `routines-and-triggers.md` (new customer in Stripe, call ended, form submitted, payment failed, message contains "cancel", task overdue, file added to a folder). For each: the event, the action, and whether the action goes to a human first.
- Default: no further triggers at first. They are added in Wire mode after the first job works. (If the first job is itself event-driven, it is already covered by 3.2 as a polling routine.)

**3.7 And the other jobs, in one line each: when, from where, to whom?** Capture each answer as one routine row marked `planned`; each gets its own detail in Wire mode after the first job works.

---

## Phase 4: Tools and access (4 questions: 4.1 to 4.3 for the first job's source and destination, then the close; add one per extra tool the CEO names)

**4.1 What do you use here for that?** For each source and destination the jobs named, and for each tool the profile lists, ask what *this company* uses. Names, not categories: "HubSpot", not "a CRM"; "Gmail on Google Workspace", not "email".
- Follow-up: "Is that the one everyone on the team uses, or just you?"

**4.2 Is it already connected to your AI?** Working today, or not yet. If you can check (an MCP is loaded, a CLI responds, a connector is listed), check instead of asking, and report what you found.
- Default: not yet; it becomes a go-live step.

**4.3 Should the employee only read from it, or also be able to change things in it?** Read-only is the default for every tool until the employee is promoted. Explain once: "Reading lets it see; acting lets it change. We start with seeing."
- Default: read-only, marked ASSUMED.

Close the phase by listing every tool with read/act and connected/not, and asking "Anything it would need that we have not named?"

---

## Phase 5: Boundaries and platform (7 questions)

**5.1 On a scale of 1 to 10, how comfortable are you with technical setup?** 1 is "I would rather never see a settings screen", 10 is "I write code or run servers". Ask them to say the number and one line about why.

**5.2 When this employee needs a tool, do you want to click Connect and sign in, or do you, or someone technical on your side, plan to wire things up with code and build more agents and workflows on top of this one?** Plain words only; no acronyms in the question. "Click and connect" means no intention to use APIs, MCPs or CLIs; "wire it up and build on it" means yes. With 5.1, this decides the platform. Rule: 7 or higher **and** "wire it up" → Hermes (or OpenClaw); otherwise Grok (or Muse, Dots). See `platforms.md` for the fit criteria and the runner-up. The one-line glossary (API: a direct software connection; MCP: a standard plug that lets an AI use a tool; CLI: a typed command) belongs in the platform-decision message, not in this question.

**5.3 Where should it live: a computer you leave on, or the cloud?** An always-on Mac mini or desktop versus a hosted service. If they have neither, the cloud.
- Default: whatever the chosen platform hosts, marked ASSUMED.

**5.4 What must it never do, even when it would seem helpful?** Write the list verbatim. Then turn each item into a tool that is simply not granted ("never spend money" → no payment credential; "never email a client" → no send-to-external tool). The prohibitions become absent capabilities, not prompt rules.

**5.5 Who may see what it produces? Could it ever reach a customer or the public, even by accident?** Internal (only the owner and logs), team (colleagues, internal channels), or public (customers, members). Public raises the review bar and the go-live gate.
- Default: team, marked ASSUMED.

**5.6 If it starts behaving badly overnight, who notices first?** The alert reader. Name the person and how fast they look (same business day is fine). Then, as its own turn: **"What should that person be able to do about it, without calling anyone technical?"** The stop button. It must park this one employee without touching anything else and without a developer. If the person who notices is not the owner, note whether the platform lets a second person reach the switch (on a per-account hosted platform it often does not; then the stop is "they tell the owner, who pauses it", and that is written down as the stop button, with the gap marked OPEN).
- Default: the owner, same business day, via the platform's pause switch, marked ASSUMED.

**5.7 What is it allowed to spend per run, in time or money, before it stops and asks?** A cost or token ceiling, or "stop after N minutes". Keep it low for the first job.
- Default, marked ASSUMED: on the simple tier "one task, stop after 10 minutes, never start a second pass"; on the advanced tier "one model call per item, about $1 per run, and stop at the first item that fails".

---

## The platform decision, then the playback

After 5.7, send the platform decision as its own message (SKILL.md, "The platform decision"): no question, the CEO may override. Then fill the contract (the table in SKILL.md) and show it with every row labelled **DECIDED** (the CEO said it) or **ASSUMED** (you proposed it). Read the ASSUMED rows aloud, since each one is a decision made on the CEO's behalf. Print the employee's self-introduction under the table. Ask the CEO to confirm or correct. Do not write a single file until they say yes.

Write the confirmed contract into the tracker's `contract[]` with `source: "decided"` or `"assumed"`, and move every open row to `"open"` with a note on what would settle it.

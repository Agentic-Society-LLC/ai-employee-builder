# Go-live: make it live, make it better

Two tiers, one ladder. The tier is decided by the platform (see `platforms.md`): **simple** for Muse, Grok and Dots; **advanced** for Hermes and OpenClaw. Both tiers end on the same earned-autonomy ladder, because the rule is the same everywhere: **trust, but verify. Autonomy is earned with evidence.**

Pacing rules for both tiers (binding):
- One stage per message. Finish a stage, say in one sentence what now exists that did not, then stop. (Stage 3 is the exception by design: one *system* per message, each confirmed before the next.)
- Separate your work from the CEO's work. When it is their turn (accounts, secrets, permissions, a switch to flip), say so and give numbered literal steps. Never ask them to paste a secret.
- Stop and wait when a stage needs them. Never describe a step as done when it is waiting on them.
- End every stage with **how they can see for themselves that it worked**: the page to open, the record to look at, the thing to click.
- Explain each new term once, in one line, the first time.
- If the CEO asks for something that breaks a control, say which one and what it costs in one sentence, then do it their way if they still want it. You advise; they decide.
- Update the tracker's `goLive[]` after every step, with the evidence line ("run record #2 at 7:03, outcome ok").

---

## Simple tier (Muse, Grok, Dots): seven steps

Done when **one routine has produced one output the CEO reviewed**. Everything else is a later Wire-mode visit.

1. **Install the employee.** Paste the rendered instructions (see `packet.md`, "Rendering") into the platform's instructions field. Set the name and avatar. Evidence: the employee introduces itself, in its own words, when the CEO says hello.
2. **Connect the first source, read-only.** Follow `connections.md`. Evidence: the tiny read ("list my five most recent…") returns something the CEO recognises.
3. **Connect the delivery destination.** Usually the CEO's own inbox or a private channel. Evidence: a test message arrives, signed with the employee's name. Where the platform can only *draft* (Grok Bot prepares email and Slack messages as drafts; see `platforms.md`), the destination is the draft itself in the owner's own account, and the evidence is the draft appearing there; write that in ROUTINES.md as the delivery.
4. **Set the first routine.** The schedule from ROUTINES.md with the time zone. Then run it by hand once (the platform's "run now" or a direct ask). Evidence: the first real output, side by side with the CEO's "great" example from the interview. Revise the skill file, not the output, until it is right; on a simple-tier platform, every skill-file edit is followed by re-rendering the instructions and pasting them back into the platform (packet.md, "Rendering"), or the employee keeps running the old version.
5. **Set the approval step.** The output goes to the owner, who approves by reply or edit. Nothing goes further. Evidence: AGENTS.md's audience line matches what the platform is configured to do.
6. **Find the stop button.** Show the CEO the pause switch for this employee and have them flip it, run the routine, and see it do nothing, then flip it back. Evidence: they did it themselves.
7. **Watch the first real scheduled run.** The next morning: did it fire at the right local time, did the output land, was it good? Evidence: the platform's run log and the output. If it did not fire, that is the whole problem to solve before anything else. This step ends the session: leave it `doing`, tell the CEO to come back tomorrow with "it ran" or "it didn't", and stop.

Then: the first trigger, if the interview named one, as a polling routine first (`routines-and-triggers.md`). And then the ladder below.

---

## Advanced tier (Hermes, OpenClaw): the nine gated stages

This is the Starter Prompt's build, condensed. The Starter Prompt, the Build Guide and the Hackathon Deck at agenticsociety.com/resources are the long form; read them when a stage needs more than this page. Stages marked WAIT do not continue until the CEO answers.

- **Stage 1: the plan in plain English.** What will be built, roughly what it costs to run per month, and the complete list of accounts and access the CEO must go and get. No code. WAIT.
- **Stage 2: the paperwork.** PRD, architecture (trigger, data path, state, tools, authority), runbook, launch gate, plus the packet (SOUL, AGENTS, skills, evals). Show the PRD in plain English, not the file. WAIT.
- **Stage 3: the CEO's turn.** Accounts, secrets into the platform's secret store (Hermes: `~/.hermes/.env` plus `terminal.env_passthrough` for anything the script reads; OpenClaw: `~/.openclaw/.env`, never the workspace; see `platforms.md`), permissions. One system at a time, numbered literal steps, confirm each before the next. WAIT.
- **Stage 4: a skeleton that runs and does nothing.** It fires on the schedule, writes one run record, and stops. No sources, no model call, no output. Then trigger it by hand so there is a second record. **Do not continue until the CEO has seen a run record with their own eyes.** This is the stage everyone skips and the one that decides whether the rest works. WAIT.

  **Where the code lives on Hermes and OpenClaw.** The packet gets a `runtime/run.mjs` (or `.py`) that owns the run shape, and the scheduler runs it **directly, with no agent session in between**: Hermes `hermes cron create "<schedule>" "<name>" --script runtime/run.mjs --no-agent`, OpenClaw an automation with a command payload (`platforms.md`). The claim, the ceiling and audience check, the run record, and the stop button all live in that script, at single call sites that throw; the script calls the model itself for the judgement step. Say this to the CEO plainly: the platform enforces none of the four controls itself; the script does. (If a platform version only offers message payloads, the message is one line, "run `node <packet>/runtime/run.mjs --job <job>`", and that launcher line is the one thing resting on instructions; say so.)
- **Stage 5: the four floor controls**, before the first real output:
  1. *The period claim.* One claim per business day (or per event), in storage that survives a restart, written before the first thing leaves, and if the claim cannot be read, do not run and record the error. Evidence: trigger it twice; the second run does nothing and names the claim.
  2. *The action ceiling and the audience.* Declared in code: widest audience (internal / team / public) and max external actions per run. Checked when the run opens. An undeclared job is refused, not defaulted. Going over the limit records an error and throws; it never trims quietly. Exactly one place in the code can send. Evidence: set the limit to 1, try to send twice, watch the second throw.
  3. *The run record and the watcher.* Every run writes a record (ok / failed / parked), including failed and parked runs. One separate scheduled job reads the records and is the only thing that alerts. It tells healthy, parked and broken apart. A second check watches the watcher. Evidence: delete a record on purpose; the watcher notices the gap.
  4. *The stop button.* A flag the CEO sets without a deploy. When set, the job wakes, records parked, returns success, changes nothing. A forced run must not bypass it. Evidence: the CEO sets it, triggers the job, sees parked, unsets it, sees it work.
  The spend ceiling on this tier: bound it inside the script (one model call per item, a cap on input size, a `max_tokens`) and record the token count and a price-list estimate in the run record. Anthropic's cost report is daily, not per request, so the script's estimate is the per-run number; Hermes (`hermes insights`) and OpenClaw (`openclaw gateway usage-cost`) give the aggregate to check it against. WAIT, after the CEO has tested the stop button themselves.
- **Stage 6: the actual job, confined.** Real sources, real output, delivered only to the narrow destination (the CEO's own channel), with the output saying it was redirected. If the first job's declared reader is someone else (a rep), the rep does not see it until promotion; the contract's "today I reach" line says the owner only, and the promotion in Stage 9 widens it. Absent from the promoted list means confined. Show a real output; ask "is that the output you wanted?" WAIT.
- **Stage 7: the failure tests**, one at a time, failure paths first. Unplug something real where possible. Each must do what the table says:

  | Break this | It must |
  |---|---|
  | Nothing to report | a correct empty result, clearly not a failure |
  | Source unreachable | say unreadable, never report an empty day |
  | Fired twice | the second run does nothing and names the claim |
  | Claim store unreadable | refuse to run, record the error |
  | Its own key missing | throw, never act as the CEO |
  | Over the action ceiling | record an error and throw, never truncate |
  | Wrong audience | refuse to send, say why |
  | Produced nothing when it owed a reply | deliver the final text anyway, record the rescue |
  | Stop switch set | record parked, return success, change nothing |
  | Provider timeout | retry within the limit, then fail loudly |
  | Permission removed | fail closed, name the access it needed and where it looked |
  | Output rejected by the owner | keep the feedback, do not overwrite approved memory |

  WAIT.
- **Stage 8: the launch gate, mechanical.** One file, `docs/launch-gate.md`, and a script (about twenty lines) that runs in CI and fails the build when the ship note's gate line is missing or an item is unconfirmed. Each item is confirmed or `N/A - <reason>`; a bare N/A fails. Then break the gate line once so the CEO watches the build go red. WAIT.
- **Stage 9: promotion on evidence.** Show the run records for three consecutive clean runs, and one clean run of every job shape (the daily and the weekly are different shapes). Clean means: a healthy record, nothing sent outside the confined destination, no escalation, the claim written before any side effect. An error resets the count. Recommend promote or wait. **The decision is the CEO's**, and it is written down with a date and a reason.

Then stop. Do not add a second employee this week. Run this one for a fortnight; the corrections made in that time are worth more than anything built instead.

Rules that apply to the advanced tier at every stage (from the Starter Prompt):
- A rule in a prompt is not a control. Every constraint is a check in the code path that performs the act, at one call site, that throws when violated. Tell the CEO which rules are enforced in code and which remain advisory, and why.
- Where a capability is prohibited, do not build the tool.
- Route work: code for anything with a right answer, the cheapest capable model for sorting and formatting, a stronger model only for judgement. Keep the cached instruction block byte-identical between runs; per-run data goes in the user turn.
- Escalations carry one action, a cause labelled measured or inferred, the failed step, the run id, and what was checked. One human-actionable escalation per run. Never route work the CEO cannot do (stack traces, logs).
- Self-healing is bounded: retry safe faults, re-read state, open an issue, draft a fix for review. Never deploy, merge its own change, change permissions, spend, or act as a human.

---

## The earned-autonomy ladder (both tiers)

The employee climbs one rung at a time. Each rung is earned by evidence from the run records, never by feeling, and each promotion is written in README.md with a date and a reason.

| Rung | What it may do | Earned by |
|---|---|---|
| 0. Hired | introduces itself, answers questions about its job | the packet is confirmed |
| 1. Works once by hand | produces the first job's output when asked | the CEO reviewed one output and said "that is the output I wanted" |
| 2. Runs on its schedule, to the owner | the routine fires on time; output goes to the owner only | 3 consecutive on-time runs with reviewed output |
| 3. Runs on real data at real volume | handles the week's real inputs, including empty days and messy cases | the failure tests (simple tier: at least empty, unreadable, and fired-twice) pass |
| 4. Wider audience | delivers to the team channel, or acts in a tool with scoped write access | 3 clean runs + every job shape clean, and the owner's written promotion |
| 5. Triggers live | reacts to events human-first, then promoted per trigger | each trigger earns rung 2 to 4 on its own |
| 6. Self-learning | captures lessons after confirmed corrections; promoted rules are read on every run | MEMORY.md has callers: capture, digest, review queue, retirement |
| 7. Self-healing | retries safe faults, re-reads state, opens an issue, drafts a fix for review | a runbook exists and the owner has handled one alert from it |

Public-facing action (replying to a customer, posting publicly) is never below rung 4, and on the simple tier it stays human-first unless the owner decides otherwise in writing.

## Improve mode: making it better

When the CEO comes back with "it did X wrong", "make it better", or "review last week":
1. Read the run records or the platform's log for the period. Say what ran, what did not, and what the outputs were. Measured facts first; label guesses as guesses.
2. For each correction the CEO confirms, capture one lesson in MEMORY.md (scope it: global or this employee) and, where it is really a rule, edit the job's skill file. Instructions synced in the same change, or the correction is not done.
3. Turn the correction into a pass/fail example in `skills/<job>.md`.
4. Check the ladder: does the evidence support a promotion, a hold, or a demotion? Say which and why. Never widen autonomy in Improve mode without the run evidence for the next rung.
5. Update the tracker's `log[]` and the README's status line.

"Dreaming" (the nightly digest on Hermes/OpenClaw) is the synthesize verb: one cheap pass per day that turns the day's records into a short internal digest with the lessons waiting for review. It publishes to no human surface except the owner's review queue.

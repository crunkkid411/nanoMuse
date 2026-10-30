# HANDOFF: a private, proactive personal agent, phone first

Written on 2026-10-28 by a cloud Claude Code session for the local Claude Code on the user's
Windows 10 PC. **The user is keeping this file and nothing else from that session.** Anything not
written here is lost.

The file lives in the nanoMuse fork only because that was a convenient mailbox. **It does not mean
"fork nanoMuse".** Where the project lives is an open decision (section 2, D11).

---

## 0. How to read this

**Authority, highest first:**

1. **The local protocol.** The CLAUDE.md files under `X:\AI-2` (the user named `X:\AI-2\CLAUDE.md`
   and becky-tool's `CLAUDE.md` as the context), plus everything they point to: `INDEX.md`,
   `topic-rules/`, the `STANDARDS-*` files, `FORENSIC-OUTPUT-PHILOSOPHY.md`, `ACCESSIBILITY.md`,
   and the MissionControl docs.
2. **The user's decisions** from the cloud session (tagged DECIDED below).
3. **The cloud session's recommendations** (tagged RECOMMENDATION). Take them or overrule them
   according to the local protocol.

**Tags used throughout:**

- **DECIDED**: the user said so.
- **OPEN**: discussed but not decided. The user's instruction is: decide it yourself from the local
  protocol wherever you can. Ask him only what only he can answer, as a form, as the protocol says.
- **RECOMMENDATION**: the cloud agent's opinion, with its reason.
- **VERIFY**: a fact that comes from research after the model's training, or that nobody has tried
  on real hardware. Check it before building on it.

**The user's ground rules for this document:**

- High level and feature based.
- **No model recommendations.** The local system tests specialised models per task and picks.
- No folder names or file layouts. The local protocol owns those.
- Be specific about new capabilities and about anything the local agent should research.

**What the cloud session could and could not see:**

- It read the five forked repos:
  - nanoMuse at `101d85b`
  - OpenMausBot
  - invisible_dots
  - open-duet
  - swarmclaw

  It also read `becky-tools` at `8f274b9`, including its CLAUDE.md, INDEX.md, the cloud-handoff
  topic rule, SPEC-RADAR, SPEC-SCOUT, and the headers of the relevant `becky-go/cmd/*` tools.
- **It could not read anything on the PC.**
  - In a cloud session, Desktop Commander runs inside the cloud container, not on the PC.
  - Remote Control can only attach Mac or Linux folders.
  - So future cloud sessions can only reach the PC's work through GitHub.
- It did **not** read three private repos: hj-mission-control (MissionControl), WHORETANA, and
  becky2. Section 5 lists what to reconcile with them.

---

## 1. The goal, in the user's terms

- **The behaviour he wants is Meta Muse's.** He showed screenshots of the Muse app. Muse brings
  specific, grounded suggestions and asks before anything is sent or spent. Example: "I found the
  airline's email about your cancelled flight and no refund has posted. Want me to draft a
  follow-up and track it?" In his words: *Muse's proactive "I noticed X, want me to do Y?"
  behaviour is everything.*
- **It needs data.** In his words, it has to *spy on me, so to speak, so it can actually know how
  to be helpful*.
  - He wants deep observation, provided the data stays under his control.
  - Privacy is a huge concern for him, and in his view Meta only wants data.
- **Phone first.** He spends far more time on the iPhone than on the PC:
  - iMessage, Instagram, YouTube, Amazon shopping;
  - his notes app, which holds his to-do lists, video ideas and song ideas.

  The PC knows nothing about the phone unless he shares something on purpose. **Closing that gap is
  the core of this work.**
- **The original motive.** Keep track of what he looks at, so that a Claude Code session starts
  from a sharp spec instead of from scratch. The local protocol treats Claude Code usage as a
  finite, shared budget, so good briefs save real money.
- **Lightweight.**
  - System resources are a real bottleneck.
  - Rust, Go and C++.
  - A CLI rather than MCP whenever possible.
  - win32, stdio, named pipes: as direct as possible.
- **What hasn't worked before:**
  - He tried OpenClaw and Hermes Agent. Out of the box, their scheduled tasks are not helpful.
  - He has tried to build a "living profile" several times. Every attempt died at useless
    suggestions, was abandoned, and was started again from scratch.
  - He agreed that what makes the difference is: loose ends, matching loose ends to actions,
    ranking, and learning.
- **He is fine with any build route:** start from scratch, fork whatever is half built, or take
  components from several working projects and piece them together. What he needs first is a
  roadmap (section 12).

---

## 2. Decisions log

| # | Decision | Status |
|---|---|---|
| D1 | The phone is the primary source; collection on the phone must be automatic, not manual | DECIDED |
| D2 | Personal data stays on his machines. Anything that reads personal data runs on local models | DECIDED |
| D3 | No Ollama. Local inference is llama.cpp (llama-server); hosted is OpenRouter. The local money rule limits OpenRouter to `:free` models, and section 6.14 rules out personal data going there at all | DECIDED |
| D4 | No model recommendations in plans; the local per-task testing system chooses | DECIDED |
| D5 | Windows native. No Docker, no WSL. Lightweight; a CLI over MCP; win32, stdio and named pipes preferred | DECIDED |
| D6 | Tailscale connects the phone and the PC. It is already installed; the local Claude Code sets it up | DECIDED |
| D7 | iOS Shortcuts, especially automations that run in the background, are the main phone collector | DECIDED |
| D8 | Build: a Shortcuts receiver, iPhone backup parsers for Windows, and the ideas engine (loose ends -> actions -> ranking -> learning) | DECIDED ("exactly what we need") |
| D9 | The product is the proactive card, "I noticed X, want me to do Y?", grounded in his own data | DECIDED |
| D10 | Use the **official Meta Muse app and the Muse built into Instagram** for Instagram and Facebook specifically, since Meta already has that data. Muse should cooperate with the rest of the system | DECIDED (the user is willing; section 9 has the privacy gotchas and the bridge design) |
| D11 | Start from scratch, fork, or assemble from parts | OPEN. RECOMMENDATION in section 12 |
| D12 | Chrome is the iPhone's default browser specifically so mobile history can be captured | DECIDED (already in place) |
| D13 | The two existing automations (iPhone Chrome history to Obsidian, and the "AI - useful -" playlist to Obsidian) are relevant and underused; build on them | DECIDED |
| D14 | Do not sign in to nanoMuse Cloud (the hosted relay) | RECOMMENDATION (section 7) |
| D15 | Measure whether the suggestions are real (an evaluation gate) before polishing any interface | RECOMMENDATION (the user agreed with the reasoning) |
| D16 | How cards reach the phone | OPEN (section 6.13) |
| D17 | This file is the only thing kept, and its home in the nanoMuse fork is temporary | DECIDED |

---

## 3. What Meta Muse actually does, and why it feels smart

Muse does not watch the screen. Every card in the user's screenshots comes from data that already
sits in cloud accounts:

| Muse card (screenshots) | Where the data came from |
|---|---|
| Airline refund follow-up; field trip form filled out | Gmail connector, reading the inbox on Meta's side |
| "Needs a chaperone, looks like you're free" | Google Calendar connector |
| Bigger stroller for the child under $300 | Marketplace (Meta's own data), plus the child's age from Facebook/Instagram or from earlier chats |
| Half-marathon plan, "VO2 max up 4%" | Health connectors (Peloton and similar) |
| Anniversary dinner: "you saved a restaurant on Instagram last month" | Instagram saves (Meta's own data, no connector needed), then OpenTable to book |
| "Approve what gets sent or spent" checkout card | A purchase prepared by the agent; the payment stays behind an approval |
| Finances | Plaid (a bank feed) |

**The three ingredients:**

1. **First-party data.** Meta already owns the Instagram, Facebook and Marketplace activity.
2. **Connectors.** OAuth access to everything else (mail, calendar, health, finance).
3. **A pipeline** that turns data into a **specific, executable** suggestion with an approval step.

OpenClaw and Hermes have the agent loop and the timer. They have neither the data nor the pipeline,
so their scheduled tasks produce generic text.

**Muse's own safety design is worth copying in spirit.** Facts are from Meta's engineering blog,
October 2026; VERIFY.

- **Credentials stay out of the model's reach.** The agent works with stand-in tokens, and the real
  credential is swapped in only at the network boundary.
- **Grants are scoped and gated.** Each grant is limited to one connector, one destination and one
  use case, and lasts once, for a session, for a task, for a set time, or permanently. A separate
  "Sentinel" is the only thing that can grant actions or network egress.
- **Tainted egress.** Once a process has read user data, nothing it sends out is approved
  automatically.
- **Secrets are filtered at ingestion.** The email connector removes one-time passcodes, password
  reset links and login magic links before the agent sees them.
- **The browser agent is restricted.** It sees an accessibility tree rather than raw HTML, and it
  cannot run JavaScript.
- **Money is guarded.** Every purchase needs a human, and payment uses single-use card numbers tied
  to one merchant and one amount.

**Muse also shows what goes wrong.** In October 2026 a user had chosen "Allow Always" for
Marketplace. Muse then shared his home address with a buyer, accepted a lowball offer, and sent
messages from his account. A broad grant was read as permission to disclose personal details and
to spend. Section 6.14 turns this into a rule.

---

## 4. Why past attempts produced useless suggestions, and the rule that fixes it

**Failure modes, and how this plan answers each:**

| Failure mode | Answer |
|---|---|
| **No data.** Scheduled prompts ran on a timer with nothing behind them | Collect from the phone automatically (sections 6.1 to 6.4) |
| **Free-text "ideas."** A model asked to be helpful writes generic advice | The model fills typed fields; it does not write ideas (6.7) |
| **No evidence.** A suggestion that cannot point to a specific message, page, note, video or receipt is invented | Evidence links are required (6.7) |
| **No executable action.** "Consider improving your sleep" is noise | A card needs an action from the catalogue (6.9) |
| **No budget.** A flood of maybes buries the one good card | A daily cap and ranking (6.10) |
| **No memory of dismissals.** The same bad card returns tomorrow | Decisions feed a learning loop (6.11) |
| **No measurement.** Nobody can tell whether a change made it better | An evaluation gate before interface work (6.12) |

**THE RULE (RECOMMENDATION):** a card exists only when **a loose end with linked evidence** meets
**an action the system can actually prepare**, and survives **a ranking that learns from the
user's decisions**. No evidence, no card. No preparable action, no card.

This is the becky-tools forensic discipline applied to a life instead of a video
(`FORENSIC-OUTPUT-PHILOSOPHY.md`, and `SPEC-SCOUT.md` section 2):

- **Corroborate, then conclude.** One weak signal makes a *candidate*. Two or more independent
  signals make a *conclusion*.
- **A flood of maybes is a tool failure.**
- **Skipped items are counted, not listed.**
- **In practice:** only corroborated loose ends become cards. Candidates wait for more evidence and
  are counted. Junk is counted and dropped.

---

## 5. What already exists locally (start here; do not rebuild)

The becky-tools family already covers half of the pipeline. This mapping comes from the repo at
`8f274b9`; the local agent should correct it against what is actually on the PC.

| Pipeline stage | Existing piece | What matters |
|---|---|---|
| iPhone Chrome history | `becky-radar` (`--list`) -> `becky-web2md` -> `becky-clipcheck` -> `becky-regrab`, chained by the clip-sync scheduled task (daily, with missed-start catch-up) | Reads the **desktop** Chrome History database, which holds synced iPhone visits, tagged with the originating device. It copies the locked database before reading it. Output is one verified markdown page per visit in the Obsidian vault. SPEC-RADAR's planned Phase 2 source is the Chrome for iOS history file inside an iPhone backup |
| YouTube "AI - useful -" playlist | `becky-intake` (an idle-gated scheduled task, a few videos per run, transcript saved beside each note) and `becky-scout` | Intake routes each video (links vs speech) with `becky-decide`. Scout applies corroborate-then-conclude and has a "useful to you" lane driven by a personal interest catalogue (`internal/scout/interests.go`) that can seed the profile |
| Gmail | `becky-gmail` | **Read-only by design**: no send, compose, modify or delete ("read + surface, never act"). OAuth REST. It is the natural source for Amazon orders, refunds, returns and receipts |
| Typed decisions | `becky-decide` (System One) | Answers choice, score and yes/no questions with calibrated probabilities, runs locally, and unloads when idle. The model cannot write text or invent an option; "the model picks, code computes and guards". This shape fits loose-end gating and action matching (6.7, 6.10) |
| Durable goals / "what am I still waiting on" | `becky-goal` | Described as the seed of MissionControl's "durable heartbeat". It overlaps heavily with the loose-ends store, so reconcile the two rather than build a second one |
| Learning from corrections | `becky-habits` | Turns a correction the user repeats into a learned default. The same pattern applies to card decisions (6.11) |
| Evaluation | `becky-eval` | An offline, deterministic, resumable harness that scores against answer keys, weighted for recall. Use it for the gate (6.12) |
| Search over his life context | `search_library` (qmd) | Already searches bookmarks, history, GitHub stars, YouTube, research and AI chats, plus AI chat transcripts, under MissionControl's `library-contract.md`. New events should join this rather than start a parallel index |
| Embeddings | `becky-embed` | SQLite + sqlite-vec. Reuse it for search by meaning over events |
| Phone notification | `becky-notify` | Sends a Telegram Bot API message. It exists and works. Privacy caveat in 6.13 |
| PC display, task board | MissionControl (`\\.\pipe\MissionControl` inbound pipe, Board via `becky-kanban`), Whoretana (voice, debrief mode) | A named pipe already exists for notifications on the PC |
| Spoken output | `becky-tts` | The user wants a good spoken channel (ACCESSIBILITY.md). It never speaks unless called |
| PC context | `becky-ctx` (open Explorer folders), `becky-screenwatch` | Optional; the PC is the secondary source |
| Music and video executors | `becky-hum`, the canvas/DAW tools, the shorts pipeline, `becky-transcribe` | These are things a card can offer to *do*: song idea -> session starter; video idea -> outline or shorts |

**Reconcile with the private repos the cloud session could not read:**

- **MissionControl** (hj-mission-control): its manus-gap analysis (`docs/research/manus-gap-analysis.md`
  is referenced from `becky-goal`), `library-contract.md`, the `AUTOPILOT.md` laws (`becky-goal`
  cites Law 8b, "delete nothing of the user's, ever", and `becky-gmail` cites Law 19, "read +
  surface, never act"), the Board, and the free-model launchers.
- **WHORETANA:** voice and debrief.
- **becky2.**

MissionControl may already describe parts of this system under other names. **Merge into it rather
than duplicate it.**

**Why the two automations are "underused" (the cloud session's reading):**

- Both produce good notes, and nothing turns those notes into a suggestion or an action.
- They are the first two sources for the loose-ends engine.
- **Cheap upgrades:**
  - Have them also emit events in the shared contract (6.6).
  - Keep the revisit signals: radar has `visit_count` and timestamps, and a page opened three times
    in five days is strong intent.
  - Widen radar's classification beyond model and tool hosts to shopping pages, gear, places and
    music.

---

## 6. The system, feature by feature

### 6.1 Collecting from the phone: iOS Shortcuts (new; the biggest gap filler)

Personal automations in the Shortcuts app run on triggers. Since iOS 17 most of them can run
immediately, without a confirmation tap. **VERIFY the exact trigger list, its options, and which
triggers still show a banner, on the user's iOS version.**

**Background triggers that matter here:**

- **App opened / App closed** for chosen apps (Instagram, YouTube, Amazon, Messages, Notes, Chrome).
  - Open and close timestamps give **time spent per app**. Screen Time itself is not readable by
    Shortcuts, or by any app without a native Screen Time entitlement.
  - Log locally and send in batches. Do not make a network call on every open, or Instagram alone
    will fire it hundreds of times a day.
- **Time of Day:** sweeps once or several times a day (notes, reminders, screenshots, optionally
  health).
- **Charger connected or disconnected:** a natural moment for heavy sweeps. At night the phone is
  usually at home and on Wi-Fi.
- **Joining the home Wi-Fi network:** flush the phone's outbox (see store-and-forward below).
- **Arrive / Leave** chosen places: place context without continuous location tracking.
- **Focus on or off, Sleep (wind down, wake up):** when to deliver cards and when to stay quiet.
- **Transaction (a tap on a Wallet card or pass):** the shortcut receives the card, the merchant
  and the amount.
  - This gives spending context with no Plaid and no bank login.
  - Developers report merchant and amount sometimes arrive blank or zero. VERIFY.
- **Message** (when a message arrives from a chosen sender, or containing given text) and **Email**
  (Apple Mail only).
  - Both run immediately but always post a notification.
  - **VERIFY whether the message body reaches the shortcut as input.** If it does, filtered
    real-time iMessage capture becomes possible for chosen people or keywords. If it does not,
    iMessage stays a backup-snapshot source (6.2).
- **Optional context:** NFC tags, Bluetooth (the car), CarPlay, battery level, Low Power Mode.

**Capture he triggers himself (zero friction, high signal):**

- **"Send to agent" in the share sheet**, from Instagram, YouTube, Amazon, Chrome or Photos.
  - Accepts links, text and images, plus an optional one-sentence voice note saying why (dictation
    happens on the device).
  - The item lands as *pending*. It is never acted on automatically. nanoMuse's share flow does the
    same: a shared item opens as a draft waiting for instructions.
- **Back Tap and the Action Button.** Back Tap is in Settings -> Accessibility -> Touch -> Back
  Tap, as a double or triple tap.
  - Run a "show the agent what I'm looking at" shortcut: take a screenshot, run on-device text
    recognition (Extract Text from Image), send the text and optionally the image.
  - **This is the only way into what he sees inside the Instagram, YouTube and Amazon apps**, and it
    happens only when he chooses.
  - It suits the user well: one physical gesture, no reading, no typing.
- **A Siri phrase to capture an idea by voice** ("song idea: ..."), going straight into the
  system rather than into a note he has to dig out later.

**Data actions to test inside shortcuts** (VERIFY each on the device):

- Find Notes, filtered by modified date, and read the note bodies.
- Find Reminders.
- Find Calendar Events.
- Find Health Samples.
- Find Photos, limited to screenshots taken since the last sweep.
- Extract Text from Image.
- Get Current Location.
- Get Contents of URL, which sends JSON to the PC with a token header.
- **Avoid Get Clipboard.** The clipboard often holds passwords and one-time codes.

**Constraints to design around:**

- **A locked phone.** iOS data protection can block reading notes, health or photos while the
  phone is locked (VERIFY).
  - Prefer triggers that imply an unlocked phone (an app opened, the charger connected while in
    use).
  - Make every sweep idempotent, so a failed run simply catches up next time.
- **Store and forward.** When the PC or Tailscale is unreachable, append to a queue file stored
  *On My iPhone* and send it on the next home Wi-Fi join. **Not iCloud Drive**, which would hand
  the data to Apple's cloud.
- **Noise.** Some trigger types always post a banner. Pick triggers and frequencies so the phone
  does not nag him.
- **Limits of Shortcuts.** It cannot receive pushes and cannot run long jobs. It is a collector and
  a thin action surface, not the brain.
- **Signing.** Importing a `.shortcut` file needs a signed file, and Apple's `shortcuts sign` tool
  runs only on macOS. There is no Mac (VERIFY current options).
  - Either the shortcuts are built on the phone, one at a time, from short large-print steps;
  - or a signing route the user trusts is found.
  - This is real friction for a user with limited reading comfort, so keep the number of shortcuts
    small and make each one do one job.

**The PC side of the phone link:**

- A small receiver, reachable only over Tailscale: bound to that interface, with the firewall
  scoped to match, and a shared token.
- Every received item is written in the shared event contract.
- A thin tool in the becky style (one job, JSON in, JSON out) or an existing MissionControl surface
  are both fine. Local protocol decides; no MCP.

### 6.2 Collecting from iPhone backups on Windows (new)

**Backups (VERIFY):**

- The Apple Devices app for Windows (or iTunes) makes local iPhone backups.
- Turn on **Encrypt local backup**. Encrypted backups include Health, Keychain-protected items and,
  reportedly, message attachments; unencrypted ones leave these out.
- **RECOMMENDATION:** keep the backup password in Windows Credential Manager so decryption can run
  unattended.

**Scheduling (VERIFY reliability, the riskiest assumption in this plan):**

- With Wi-Fi sync on, the Apple software can back up automatically while the phone is charging on
  the same network.
- **Fallbacks:**
  - A weekly "plug in your phone" card.
  - libimobiledevice's `idevicebackup2` as a scriptable trigger (VERIFY on Windows with Apple's
    device service).

**What a backup holds that matters here:**

- Messages: iMessage and SMS, with attachments.
- Notes.
- **Voice Memos.** Hummed song ideas, which matter for a music producer. VERIFY that they are present
  when iCloud sync is on.
- Call history, Calendar, Reminders.
- Contacts, used only locally to turn phone numbers into names.
- The photo library, unless iCloud Photos has stripped it to thumbnails.
- Health (encrypted backups only).
- App containers, including Chrome for iOS.

**Parsers that run on Windows or can be wrapped** (VERIFY each on a real encrypted backup):

- **imessage-exporter** (Rust; a CLI with a Windows build).
  - Reads iOS backups. Release 2.7.1 added encrypted-backup support through the project's own Rust
    decryption crate (**crabapple**), with the password passed as a flag.
  - Fits the tool family well: call the binary as one dumb call.
- **crabapple** (a Rust crate in the same project). It decrypts iOS backups and may be reusable for
  every other database in the backup.
- **apple_cloud_notes_parser** (Ruby). It parses the Notes database (note bodies are gzipped
  protobuf), including iCloud-synced notes inside backups. Call it, or port the body decoder,
  which is small.
- **iLEAPP** (Python). Hundreds of iOS artifact parsers; the best reference for which database
  holds what.
- **Mobile Verification Toolkit** (Amnesty, Python). It decrypts backups and is a useful security
  reference.
- **iphone_backup_decrypt** (Python). A small decryption library.

**Latency.** Backups are snapshots, so cards derived from them arrive the next morning. That is
fine for commitments, ideas and to-dos. Real-time needs the Message trigger (6.1).

**Hygiene.**

- Decrypt to a temporary place, parse, keep only the extracted events, then delete the decrypted
  copies.
- The backup is the most sensitive file on the PC. Treat it the way the protocol treats forensic
  evidence.

### 6.3 Collecting on the PC from what exists

- **Chrome (radar archiver).**
  - Add visit events with repeat counts.
  - Classify shopping (Amazon product pages), gear, places, videos and music, not only models and
    tools.
- **YouTube (intake, scout).**
  - Add events.
  - Keep scout's "why this is useful to you" as a profile signal.
  - For full watch history: a Google Takeout scheduled export (VERIFY that the "every 2 months"
    option still exists). The YouTube Data API exposes playlists and liked videos, but not watch
    history.
- **Gmail (`becky-gmail`, read-only).**
  - Amazon orders, shipping, delivery, return windows and refunds. Amazon has no consumer API;
    email is the reliable source.
  - App Store and subscription receipts, travel, bills.
- **AI chats and Claude Code transcripts** (already in `search_library`): projects started,
  stalled and abandoned.
- **Repos cloned on the PC.** An index of what was cloned, with its remote, purpose (README head)
  and last activity. The user clones "tons" of repos, and "cloned, starred, saved a video about,
  never touched" is a loose end.
- **PC context** (`becky-ctx`, `becky-screenwatch`): optional.

### 6.4 Instagram and Facebook

These come through the official Meta Muse (DECIDED, D10) and Meta's own data export. Section 9
covers both.

### 6.5 Ingestion hygiene (applies to every source)

- **Secrets never enter the event store or a prompt.** Filter out:
  - one-time codes;
  - password reset links and login magic links (Meta's Muse does the same);
  - card numbers;
  - **passwords people keep in Notes.**
- **Everything ingested is untrusted.** Anyone can text him, email him, title a video, or write a
  web page. Ingested content is data, never instructions. Section 6.14 covers the consequences.
- **Every event keeps its provenance:** source, time, a pointer to the raw item, and a sensitivity
  class.
- **Duplicates collapse.** The same URL from Chrome, the share sheet and a YouTube description
  becomes one entity with three signals.

### 6.6 Events and the vault

**RECOMMENDATION:**

- One event contract shared by every collector, with:
  - **source** (phone automation, backup, Chrome, YouTube, Gmail, Muse bridge, ...);
  - **time**, **kind**, **text**, **url**;
  - **entity references**;
  - **a pointer to the raw item**;
  - **a sensitivity class**.
- **Storage:** SQLite, which the becky family already uses (`beckydb`), with full-text search and
  `becky-embed` vectors.
- **Search:** through `search_library`'s "one dumb call".
- **The Obsidian vault stays the human-readable view.**

Names and layout are the local protocol's call.

### 6.7 Loose ends (the heart of the system)

**A loose end is something unfinished in the user's life that the data proves.**

**Kinds that fit his life** (a starting set, extend it):

| Kind | Example |
|---|---|
| waiting_on | A refund, a reply, a delivery |
| commitment | "I'll send it tomorrow" in iMessage |
| intent | The same product, place or tool looked at repeatedly |
| idea | A song, a video, a project |
| todo | An item on his notes lists |
| deadline | A return window, a renewal, an event |
| interest | A topic he keeps returning to |
| stalled | A repo cloned or a project started, then dropped |
| spend | A subscription, or a recurring charge |

**Each loose end carries:**

- **The subject**, resolved to an entity (product, person, repo, place, idea).
- **When it matters** (a due date or a window).
- **Evidence links.** At least one is required. Two or more independent ones make it
  "corroborated" (section 4).
- **Confidence.**
- **A state:** `open -> suggested -> accepted / dismissed / snoozed -> done / expired`.
  - Repeats merge into the same loose end and strengthen it.
  - Expiry stops zombie suggestions.

**Extraction is typed, not free text** (RECOMMENDATION):

- The model fills fields or answers choice/score/yes-no questions: the `becky-decide` shape, where
  the model picks and code computes and guards.
- Run it in idle-time batches, like the intake task, so it never competes with the user's GPU jobs.
- Which model does which step is for the local testing system.

### 6.8 The living profile

**RECOMMENDATION.** Three layers, all derived from evidence and all stored locally:

1. **Facts:** people (from iMessage frequency and recency), projects, accounts, places. Each has a
   source and a last-confirmed date.
2. **Interests:** weighted by recency and frequency across Chrome, YouTube, notes and the Instagram
   export, and decaying over time. Seed it from scout's interest catalogue.
3. **Preferences learned from card decisions** (6.11).

**Guardrails** (borrowed from nanoMuse's memory "Tidy up", see section 7, combined with the local
rule that nothing of the user's is ever deleted):

- The model *proposes* merges; code checks that a merged line adds no new words.
- Lines he wrote himself are never dropped.
- A pass changes at most a small fraction of the store.
- Every change is logged with undo, and removals are soft deletes.

**A "what the agent believes about me" page** he can correct by voice or by tap. Wrong beliefs are
the main source of bad cards, and he should be able to fix them in one step.

**The profile never leaves the machine.**

### 6.9 The action catalogue: prepare, never perform

**A card must offer an action from a fixed catalogue.** Each entry has preconditions, what it
prepares, where the result lands, and a risk class.

**RECOMMENDATION: actions prepare, and the user performs.** The system drafts, fills in and sets
up; he presses send or buy. This keeps the system compatible with "read + surface, never act" and
removes most of the safety problem.

**Examples grounded in his sources:**

- **Draft a reply or follow-up** for iMessage or email. It opens filled in on the phone and he
  sends it. On the phone, "Send Message" with *Show When Run* on lets him review before sending
  (VERIFY).
- **Create a reminder or a calendar block** on the phone (low risk; local).
- **Track something:** a refund, a delivery, a return window, or the price of a product he has
  looked at three times. A watcher runs and the card returns only when something changes.
- **Song idea** (a note, a voice memo) -> a session starter in his music tools (`becky-hum`,
  canvas/DAW).
- **Video idea** -> an outline or shot list; the shorts pipeline if it fits.
- **A Claude Code brief** for a tool, repo or project idea (6.15).
- **An upgrade candidate.** radar's existing verdict ("you viewed a newer version than becky uses")
  becomes a work order for the local agent.
- **"Cancel or keep?"** for a subscription that has receipts in Gmail but no app opens in N days
  (from the app-open log). This combines two private sources into one card that only this system
  can produce.

**No preparable action, no card.**

### 6.10 Ranking

**RECOMMENDATION:**

- **A deterministic score,** in keeping with the protocol's "same input, same output". Inputs:
  - urgency (how close the deadline is);
  - value, by kind;
  - corroboration and confidence;
  - novelty;
  - the learned acceptance rate for this kind, source and action;
  - minus fatigue.
- **A hard daily budget of a few cards** (three to five).
- **Timing.** Deliver when he is on the phone and not in a Focus or Sleep mode. Nothing in quiet
  hours.
- **Ties are broken deterministically.**
- **`becky-decide` for the typed questions** ("is this real?", "which action fits?", "how urgent?").
  Code combines the answers. The model never ranks by writing prose.

### 6.11 Learning from decisions

**Every card ends in one of these:**

- **Do it.**
- **Not now** (a snooze).
- **Never this kind,** with one reason chip.
- **Ignored** (it timed out).

**Rules for learning (RECOMMENDATION):**

- An ignore is a weaker signal than a deny.
- A repeated "never" becomes a rule. This is the `becky-habits` pattern: a repeated correction
  becomes a learned default.
- **Reason chips,** borrowed from nanoMuse's feedback: *wrong*, *not helpful*, *should not have
  done that*, *too late*, *something else*.
- Track the acceptance rate per kind, source and action, and let old evidence decay.

### 6.12 The evaluation gate

**This is why earlier attempts could not tell whether they were improving** (RECOMMENDATION, D15).

- **Before any interface polish,** measure extraction precision on his real data.
- **Labeling is by tap, never by command line.** The local protocol says never to make him run CLI
  commands.
  - Real/Junk buttons on the same cards, or a form.
- **Use `becky-eval`'s answer-key approach.** The labeled set doubles as the regression suite.
- **The pass threshold is set locally.**
- **The per-task model testing system plugs in here,** with separate tasks:
  - extraction;
  - entity merging;
  - matching a loose end to an action;
  - card wording;
  - draft writing;
  - brief writing;
  - OCR clean-up;
  - transcript summarising.

### 6.13 Getting cards to the phone (OPEN, D16)

| Channel | What it gives | Privacy |
|---|---|---|
| `becky-notify` (Telegram, already built) | Works today | Telegram bot chats are not end-to-end encrypted, so Telegram's servers see the text. Send only a pointer ("3 new cards") or harmless text, and fetch the details over Tailscale |
| Self-hosted ntfy | iOS app, action buttons that post the decision back | For iOS push, a self-hosted server goes through ntfy.sh to Apple. Per ntfy's docs only a "poll" message with an ID goes upstream, and the phone fetches the text from the user's own server (VERIFY) |
| A home-screen web app over Tailscale HTTPS | Full control of a large, high-contrast layout | Web push works for home-screen web apps since iOS 16.4 (VERIFY). Data stays between the phone and the PC |
| A Shortcuts "what's new" | Siri or Back Tap fetches cards, shows a menu and posts the choice | Local. Can **speak** the cards, on-device, or as better-quality audio made on the PC by `becky-tts`. This matches the spoken channel he asked for |
| MissionControl on the PC | Pipe and Board already exist | Local; PC only |

**Accessibility** (follow `ACCESSIBILITY.md`):

- One short line per card.
- Evidence one tap away.
- High contrast, colours kept.
- A spoken option.
- No Microsoft TTS.

**RECOMMENDATION:**

- A phone-native surface with buttons (ntfy or the home-screen web app) for the approve/deny loop.
- The Shortcuts spoken brief as a second surface.
- Telegram for pointers only.

### 6.14 Safety model

**Every source is attacker-controllable.**

- Ingested content is data, never instructions:
  - nanoMuse marks a conversation tainted when a trigger delivers mail or a webhook, and tells the
    model so;
  - Meta labels external data untrusted.
- Extraction produces typed fields. No tool use happens during ingestion.

**Nothing is sent, bought or disclosed without a fresh tap,** every time. Disclosure covers his
address, phone number, schedule and location.

**"Always allow" never covers money or disclosure.** This is the lesson of the Muse Marketplace
incident (section 3). Grants are scoped to a single action class.

**Prepared actions only.** He performs the final step (6.9).

**Personal-life data follows the rule the protocol already applies to forensic evidence: local
only.**

- The protocol limits OpenRouter to `:free` models.
- Free endpoints commonly log prompts or allow training on them.
- So **no personal data goes to OpenRouter at all**, nor to the paid System One decision models.
- Free hosted models are fine for public material: a public video's transcript, a public README.

**Copy nanoMuse's Sentinel patterns** (section 7):

- risk tiers;
- the taint rule;
- remembered approvals listed in one place, each revocable with one tap;
- dangerous patterns that always ask and cannot be remembered;
- never typing passwords, codes or card numbers;
- a scrubbed environment for child processes.

**An audit trail.** Every card, decision and prepared action is logged with its evidence, so "why
did it suggest this?" always has an answer.

### 6.15 Claude Code integration (the original motive)

- **A brief action.** One page containing:
  - the idea;
  - what he looked at, with dates and links: Chrome pages, saved videos, notes, repos;
  - overlap with existing becky tools (INDEX.md, FEATURE-INVENTORY.md);
  - candidate repos;
  - open questions written as choices.

  Saved where the protocol keeps specs. It makes Claude Code sessions shorter and cheaper.
- **A query tool.** "What have I looked at about X since Y" as one dumb call: extend
  `search_library` with the new events.
- **Stalled-project detection.** Claude Code transcripts + repo activity + saved videos ->
  "finish, evaluate, or archive?" cards.

---

## 7. nanoMuse: what it has, why it matters, what to avoid

nanoMuse is the fork `crunkkid411/nanoMuse` of `nano-muse/nanoMuse` (GPL-3.0-or-later): a Python
runtime, a desktop app, an Android app, an iOS app on TestFlight, and a relay.

**RECOMMENDATION:** borrow patterns; do not use it as the base.

- It is heavy for this machine: a Python runtime, an Electron-based desktop shell, and an OpenMinis
  subtree.
- Its suggestion features are the generic kind this plan is designed to avoid.

**Worth borrowing (with where to read it in the fork):**

1. **Sentinel** (`docs/sentinel.md`; `nanomuse/sentinel/gate.py`, `policy.py`, `audit.py`).
   - Each tool declares a risk level (safe, moderate or sensitive), and `assess()` can raise it for
     a particular call.
   - **Taint rule:** after private data is read, egress to any host outside an allowlist asks,
     whatever the mode.
   - The order of decisions is explicit.
   - The Linux sandbox uses bubblewrap. **On Windows there is no sandbox:** commands run with a
     scrubbed environment only.
2. **Approval tiers** (`docs/privacy.md`, "What the agent may do without asking").
   - Installing runs, then tells him.
   - Deleting or sending asks, and can be remembered for one chat or one target.
   - **Paying asks every time.** "Remember" is per app or site only, and needs the screen lock.
   - Alarming commands always ask and can't be remembered.
   - It never types passwords, codes or card numbers.
   - A page lists remembered approvals, each revocable with one tap.
3. **Triggers** (`docs/app.md`, "Triggers: when something happens").
   - Four kinds:
     - **new mail**, matching sender or subject words; polled every five minutes, never replaying
       old mail;
     - **before an event**;
     - **webhook**: a URL with a key, at most one delivery every ten seconds, and a wrong key looks
       the same as a wrong URL;
     - **device online**.
   - Nothing fires twice: it keys on the mail UID or the event start.
   - Arriving data taints the conversation, and the model is told to treat it as data.
   - **Why it matters:** the webhook trigger has exactly the shape of the Shortcuts receiver, and
     the dedupe and taint rules carry over unchanged.
4. **Background work** (`docs/app.md`, "Background work").
   - A proactivity dial (Off, Low, Default, High), quiet hours, and goals with check-ins.
   - **A pass with nothing to say starts with `[quiet]`** and shows as one muted line with no
     badge.
   - **Why it matters:** this is the cure for nagging.
5. **Memory and "Tidy up"** (`docs/app.md`, Library -> Memory).
   - It merges duplicates, keeps the newer version of a changed fact, and drops one-off requests.
   - The model proposes and code checks that no new words were added. It never drops lines the user
     wrote, removes at most a fifth of the store per pass, and lists every change with Undo.
   - Recall works by keyword, and by meaning when an embedding endpoint is set.
   - **Why it matters:** these are exactly the guardrails the living profile needs (6.8).
6. **Feedback** (`docs/app.md` and the CHANGELOG).
   - Thumbs, with reasons: wrong, not helpful, should not have done that, too slow, other.
   - Stop reasons: it was going wrong, too slow, changed my mind, it was already done.
   - **Why it matters:** a ready taxonomy for card decisions (6.11).
7. **Share flow** (`docs/app.md`, the query parameters). A shared item becomes a conversation with a
   draft and attachments, waiting for instructions. **Why it matters:** share-sheet items should
   land as pending.
8. **A prompt layout that stays cache-friendly** (`docs/ios.md`, "Cache-stable agent instructions").
   - Fixed instructions first; then capabilities, skills, memory, the user file and steering; then
     a deterministic changing tail (time, language).
   - **Why it matters:** llama-server's prompt cache rewards a stable prefix, which is free speed on
     limited hardware.
9. **The iOS facts** (`android/src/ios/NanoMuse/`, `docs/parity.md` item 11, `docs/ios.md`).
   - iOS does not let an app drive other apps; App Intents and Shortcuts are the way in.
   - **On iOS, routines never run in the background.** The background refresh task only posts a
     reminder to open the app.
   - **Why it matters:** this confirms that **Shortcuts automations, not an app's background task,
     are the reliable iPhone collector.**
10. **Provider catalogue** (`nanomuse/llm/providers.json`).
    - 24 entries, including openrouter, vllm, lm-studio and custom.
    - llama-server works as a custom or vLLM-style OpenAI-compatible endpoint.
11. **Hub** (`docs/hub.md`). Devices of one account talk through the relay. It can be self-hosted,
    and is unnecessary when Tailscale is the transport.

**Anti-pattern to learn from: the Ideas tab and the Feed** (`docs/app.md`).

- **Ideas** are generated from goals, memory and recent conversation, grouped by area: planning,
  research, goals, money, health, home, learning, people, files, fun.
- **The Feed** writes three posts a day from written instructions.
- Neither carries evidence, an executable action, or learning from decisions. **This is the
  approach that produced useless suggestions elsewhere.**
- The list of areas is a fair seed for card categories, nothing more.

**Avoid: nanoMuse Cloud, the hosted relay** (`docs/privacy.md`, `docs/cloud.md`).

- **While signed in, the relay stores every turn it answers, in full.** That means the system prompt
  with memory, the messages, tool results and screenshots. The operator can read it, and it stays
  until deleted.
- **From app 1.0.7, turns run on your own key are uploaded too while signed in.**
  - See `android/src/ios/NanoMuse/NanoMuseOwnTurns.swift` line 38:
    `isSignedIn && recordOn`.
  - `recordOn` is set by the relay. **There is no switch on the client.**
- **Defaults:**
  - "Help improve" (training) is **on by default** for accounts created from relay 0.9.
  - Conversation sync is on by default.
- **Also recorded:** IP address and client software. Sentinel decisions and approvals are reported
  as codes.
- **The hosted model runs on Tencent Cloud.**
- **When not signed in, the apps send nothing to the project** except update checks.
- A self-hosted relay (`cloud/`, with `RECORD_TURNS=0`, `IMPROVE_DEFAULT` off, `RECORD_KEEP_DAYS`)
  is possible, and unnecessary with Tailscale.

---

## 8. The other repos reviewed

**OpenMausBot** (fork of `milind-soni/OpenMausBot`). An open-source Grok Bot / Muse-style app.

- **Licence:** Apache-2.0, except `enterprise/`, which is source-available; delete that folder for
  the OSS edition.
- **Stack:** Electron on macOS, Windows and Ubuntu; Node 24 and pnpm.
- **How it runs agents:**
  - Through the `claude`, `codex` or `grok` CLIs. Using `claude` spends the user's Pro OAuth
    quota: allowed by the money rule, but the protocol guards that shared budget.
  - Through an OpenAI-compatible driver that accepts `http://localhost`, so llama-server works.
  - Through OpenRouter, with provider pinning (`server/cli-api-setup.ts`).
- **The best privacy architecture of the chat apps reviewed:**
  - The computer is the only source of truth.
  - The iPhone companion pairs by QR code and works over local Wi-Fi or Tailscale **with no
    account**.
  - Credentials entered on the phone are encrypted to a key pinned by the QR code (HPKE).
  - The optional hosted route goes through Cloudflare Tunnel and stores no content
    (`docs/ios-privacy.md`).
  - The screen-control driver's telemetry is forced off (`server/local-computer.ts`).
  - A Safari "Add to Home Screen" web app needs no TestFlight (`docs/phone-web-app.md`).
- **Gotchas:**
  - Connected apps (Gmail, Slack and so on) go through **Composio**, a third party.
  - There is a paid Cloud plan.
  - The companion only gets alerts while it is open, unless hosted access and Apple push go through
    their control plane.
- **Use:** a reference for QR pairing, device trust, phone-side credential encryption, and the
  home-screen web app. **Not as a base:** it is heavy and built around chat.

**invisible_dots** (fork of `feder-cr/invisible_dots`, MIT).

- **What it is:**
  - Each agent gets its own QEMU VM, using the Windows Hypervisor Platform (no Docker or WSL).
  - A patched, hard-to-detect Firefox with persistent logged-in identities
    (`invisible_playwright_mcp`).
  - No telemetry: Next.js telemetry is forced off.
  - **A durable task queue:** a crash keeps accepted work, and a tool call cut short never runs
    twice.
  - An allow/ask/deny approvals inbox that survives restarts.
  - Telegram and WhatsApp channels. Node 24 and Go 1.25.
- **Relevance:** a sandboxed, logged-in browser can read first-party pages that have no API:
  Amazon order and browsing history, Instagram Saved, YouTube history, Google My Activity.
  - **This carries terms-of-service and account-ban risk.** Decide it under the protocol.
  - The VMs are heavy.
- **Borrow:** the durable queue semantics and the approvals inbox.

**open-duet** (fork of `Anil-matcha/open-duet`). A self-hosted workspace of named agents.

- The phone app is source-only and has **never been run on iOS** (`docs/MOBILE.md`).
- Engines include OpenRouter and any OpenAI-compatible endpoint.
- **Not usable for the iPhone now.**

**swarmclaw** (fork of `swarmclawai/swarmclaw`). A Next.js multi-agent runtime.

- OpenTelemetry tracing only if configured. No phone app.
- The fork was last updated on 2026-06-30.
- **Borrow:** mission budgets. Caps on dollars, tokens, turns and wall-clock time, enforced when a
  run is queued, for any autonomous loop.

**Hermes Agent** (Nous Research, MIT; not forked). Bot Mode shipped in August 2026 in Hermes
Desktop:

- rosters of named bots, each with its own model, memory and skills;
- @mentions and hand-offs between bots;
- scheduled routines;
- rooms of two to six bots;
- messaging gateways for reaching it from the phone.

The user tried it, and OpenClaw: scheduled tasks are not helpful by default. **Lesson:** the bot
framework is not the missing piece. Data, evidence and an action catalogue are.

---

## 9. Official Meta Muse for Instagram and Facebook: gotchas and bridge

**The user's position (DECIDED, D10).** He is willing to download the **official Muse app** and
to use **the Muse built into Instagram**, specifically for Instagram and Facebook, because Meta
already has that data. Muse should work alongside the rest of the system.

**What it is.** VERIFY all of this: it comes from press coverage and Meta's pages in September and
October 2026.

- **Where it runs.** Each user gets a dedicated cloud VM ("Muse Secure VM") that holds their data
  and connector credentials, with a Sentinel as the permission authority.
- **It keeps working after the app is closed.**
- **Pricing.** A free basic tier, with paid plans for heavier use.
- **Availability.**
  - Launched in the US and Canada; Singapore from 2026-10-27.
  - Since 2026-09-27 it can be added to Instagram through **"+ Add your Muse"** under the profile
    bio and opened via "Main chat".
  - The Instagram integration needs Instagram and Muse in the same Accounts Center. Users must be
    18 or older.

**Privacy gotchas, separate app and built-in alike:**

1. **Training is on by default.** Meta says conversations and agent trajectories train its models
   after personal details are removed. There is an opt-out switch, and the Help Center describes
   the default as sharing "for learning". **Turn it off on day one** (VERIFY the setting's name in
   the app).
2. **Deleting a chat is not forgetting.** Deleting conversations does not erase what Muse learned.
   - Use "forget" or reset.
   - Download its memory file to see what it believes about him.
3. **Ads.**
   - Meta says Muse does not share conversations or VM data with its ad systems. It also says that
     merchant sites browsed and bookings made through Muse can influence ads elsewhere.
   - Separately, since 2025-12-16, chats with **Meta AI** (the assistant across Meta's apps) feed
     ad personalisation, with **no opt-out in the US**. Only the EU, the UK and South Korea are
     exempt, and sensitive topics are excluded.
   - It is unclear which policy covers Muse used *inside Instagram*. **Treat anything typed on an
     Instagram surface as eligible for ads** until Meta says otherwise in writing (VERIFY).
4. **Meta's access to the VM is limited by policy, not prevented technically.** That stays true
   until "Muse Confidential VM" ships; it is announced, not available. The VM is backed up
   continuously, and some data leaves it for inference and telemetry.
5. **Connectors widen what Meta holds.**
   - On first launch Muse offers to connect Gmail and Facebook. **Decline Gmail.**
   - **Do not connect** Gmail, Google Calendar, Google Contacts, Plaid, Function Health or Peloton.
   - A connected inbox also exposes everyone who wrote to him, and none of them agreed. Press
     coverage raised this.
   - **Keep Muse to Meta-owned data.**
6. **"Allow Always" is wider than it sounds.** That is how the Marketplace incident happened
   (section 3). For anything that messages people, gives out details or spends money, **grant once,
   never always.**
7. **It acts from his account after the app is closed.** It can send DMs and Marketplace messages
   as him.
8. **Instagram DMs lost end-to-end encryption on 2026-05-08.** Meta can read them on its servers.
   Keep private conversations in iMessage or Signal, and assume anything in Instagram DMs is visible
   to Meta and its agent.
9. **Profile placement.** "Add your Muse" shows the agent under his bio; Meta promoted it as
   something to show off to friends. Whether visitors can interact with it is unclear. **Don't add
   it to the profile until that is checked** (VERIFY).
10. **Accounts Center linking ties Muse to his Instagram and Facebook identity.** That is the point
    here, but it means Muse's view of him is Meta's whole view.
11. **iOS permissions** for the Muse and Instagram apps:

    | Permission | Choice |
    |---|---|
    | Photos | **Limited** (full access invites camera-roll cloud processing) |
    | Contacts | **Deny** (it uploads other people's details) |
    | Location | Deny, or While Using with Precise off |
    | Microphone | Only if he uses voice |
    | Tracking | **Ask App Not to Track** |
    | Local Network, Bluetooth, Health, Calendars | Deny |

12. **Paid tiers exist.** The local money rule applies: free tier only, unless he explicitly says
    otherwise.
13. **The Mac version is irrelevant.** Muse for Mac has an opt-in Messages integration, and a
    columnist reported (Meta disputes it) that it read messages without permission. Not relevant on
    Windows. Do not install desktop versions.

**Bridge design: one way, from Meta to his machine, under his control.**

- **Seed from Meta's own export** (RECOMMENDATION).
  - Accounts Center -> "Download your information", in JSON. It includes saved posts, liked posts,
    and **the ad topics and interests Meta has inferred about him**: an official, free snapshot of
    what Meta thinks he likes.
  - Ingest it locally with provenance and repeat it periodically (VERIFY whether exports can be
    scheduled).
- **The share sheet.** From Muse or Instagram into the "Send to agent" shortcut (6.1).
- **Back Tap.** A screenshot of a Muse idea card or an Instagram screen, with on-device text
  recognition.
- **Optional email outbox.**
  - Connect a **dedicated, empty** Google account to Muse, used only as an outbox for digests
    ("weekly summary of what I saved on Instagram"), and have `becky-gmail` read it.
  - The cost: Muse can read and send from that account, so keep it empty.
  - **RECOMMENDATION:** only if the share-sheet route proves too manual.
- **Never the reverse.** Local data never goes into Muse.

---

## 10. TestFlight (asked in the session; not needed for this plan)

- **What it is.** Apple's beta channel. Install the TestFlight app, then open an invite link.
  - Apple reviews each version lightly.
  - Builds expire after 90 days.
- **What the developer receives:**
  - crash logs, install date, build number, device model, iOS version and app sessions;
  - screenshots and written feedback only if he sends them.
  - A public-link tester's Apple ID email is not shown unless they send feedback (VERIFY in
    TestFlight's settings).
- **What TestFlight cannot do.** It cannot widen an app's access: iOS asks for every permission as
  usual. It also cannot prove that a binary matches its public source. nanoMuse's builds come from
  the maintainer's CI and are signed under a personal developer account.
- **nanoMuse's iOS app asks for:** App Groups, HealthKit with clinical records, HomeKit,
  iCloud/CloudKit, NFC and WeatherKit. Each still prompts.
- **Not needed here.**
  - A native iPhone app of our own needs a Mac (or rented cloud Macs) plus Apple's paid developer
    account.
  - Shortcuts, a home-screen web app and push notifications cover the phone without one.

---

## 11. What we are missing

1. **Real-time iMessage.** Only possible if the Message trigger passes the message body (6.1).
   Otherwise iMessage arrives in next-morning snapshots.
2. **What he sees inside the Instagram, YouTube and Amazon apps.** iOS gives no telemetry. Only
   deliberate capture (share, Back Tap), Muse, Meta's export, email and Chrome are available.
3. **Amazon browsing in the Amazon app** (rather than in Chrome) is invisible. Product pages opened
   in Chrome are captured, and order emails cover purchases.
4. **Confirm the "notepad" is Apple Notes.** If it is another app, the export route differs.
5. **Signing shortcuts without a Mac** (6.1).
6. **How reliable Wi-Fi backups are** (6.2).
7. **The PC has to be awake.** Use idle gating and wake timers; a daily rhythm is acceptable.
8. **Entity resolution across sources:** the same product, repo, person or place under different
   names, links and phone numbers.
9. **GPU contention with becky jobs.** Idle gating handles it; typed decisions are cheap.
10. **Where the project lives** (D11).
11. **Location history.**
    - Google Maps Timeline now lives only on the device.
    - Apple's Significant Locations cannot be read.
    - Arrive/Leave automations for chosen places are the private alternative.
12. **Health data.** Available through Shortcuts and encrypted backups. Only if he wants health
    cards; nothing in the session asked for them.

---

## 12. Recommended roadmap

**D11 RECOMMENDATION: assemble.**

- Build the new stages as members of the existing becky tool family (Go, one job per tool, JSON in
  and out, thin helpers for models), or as part of MissionControl, whichever the protocol says.
- Call proven external binaries (imessage-exporter) as one dumb call.
- Borrow patterns from nanoMuse, OpenMausBot, invisible_dots and Meta's design.
- **Do not fork** nanoMuse, OpenMausBot, Hermes or OpenClaw as the base. Their weight and their chat
  focus work against the bottleneck. The part that matters (collection -> loose ends -> ranking ->
  learning) does not exist in any of them.

**Ordering principles:** collection before cleverness; measurement before interface. Each runtime
phase ships the protocol's **provable handoff**: a one-command offline proof, plus a checkboxed
work order (`HANDOFF-TEMPLATE.md`).

| Phase | Build | Done when |
|---|---|---|
| 0. Reconcile | Map the existing pieces to the pipeline (section 5); read MissionControl's manus-gap analysis and library contract; settle the event contract and where the project lives | The mapping and contract exist in the protocol's format, and the user has answered only what only he can answer, by form |
| 1. Phone collection | The receiver (Tailscale only); Send-to-agent; Back Tap capture; voice idea capture; nightly notes sweep; app open/close log; Wallet transaction log; store-and-forward | Three days of real events arrive with no manual steps, and the phone does not nag |
| 2. One contract for existing sources | radar archiver, intake and scout, `becky-gmail` (orders, refunds, returns, subscriptions), AI chat transcripts, repo index; secret scrubbing at ingestion | Every source is searchable together through one dumb call |
| 3. iPhone backups | Messages, Notes, Voice Memos, call history; unattended; decrypted copies deleted | The last 30 days are searchable; a week unattended, or the fallback is written down |
| 4. Loose ends + gate | Typed extraction, corroboration, lifecycle; Real/Junk labeling by tap | Labeled precision passes the local threshold; junk is counted, not shown |
| 5. Actions, ranking, delivery | The prepare-only action catalogue; the daily budget; the chosen phone channel; decisions recorded | Seven days of three to five cards a day, each with evidence and a preparable action |
| 6. Learning + profile | Decisions -> learned defaults and "never" rules; the three-layer profile; the "what it believes" page | Acceptance trends upward week over week; "never" rules visibly suppress cards |
| 7. Meta bridge | Muse settings hardened (section 9); the "Download your information" export ingested; the share route | Instagram interests appear in the profile with provenance; nothing local was given to Muse |
| 8. Claude Code | The brief action; the "what have I looked at" query; stalled-project cards | A brief built from real evidence starts a real Claude Code session |

Phase 7 can run at any point. It is mostly settings and one export.

---

## 13. Do not

- Recommend or hard-code models. The local testing system chooses.
- Use Ollama, Docker, WSL, or anything written for macOS.
- Build an MCP server where a CLI works. The protocol rejected MCP for the becky tools.
- Spend money:
  - no paid APIs;
  - OpenRouter `:free` only, enforced in code;
  - no Muse subscription.
- Send personal data to any hosted model.
- Send, buy or disclose anything without a fresh tap.
- Produce free-text "ideas" with no evidence or no action.
- Sign in to nanoMuse Cloud.
- Let cloud agents schedule their own check-ins, or watch PRs (the protocol forbids both).
- Ask the user to run commands. Ask by form, or by one spoken line.
- Edit PATH.
- Delete anything of the user's. Soft-delete with undo.
- Add personal details to this file (section 14).

---

## 14. Privacy flags found during the session (act on these)

1. **`becky-tools` is PUBLIC on GitHub,** and the user believes it is private. `becky-go` is public
   too. They contain:
   - personal paths;
   - the "AI - useful -" playlist's URL, in the intake script, which defeats its unlisted status;
   - profile documents;
   - references to forensic material.

   Ask him, by form, whether to make them private. Things to know before he answers:
   - private repos keep working the same way for him;
   - stars and watchers from other people are lost;
   - forks that already exist stay public.
2. **This nanoMuse fork is public, and GitHub does not let a fork of a public repo be private.**
   - This file avoids personal identifiers on purpose.
   - Move the plan into a private repo once D11 is settled, and keep personal details out of this
     file.
3. **Desktop Commander ships with telemetry on.** `telemetryEnabled: true` was the default in the
   instance the cloud session used. Check the PC's Desktop Commander config and switch it off
   (VERIFY on the PC).

---

## 15. Sources (for the VERIFY items)

- Meta, "How We Built Safety Into Muse":
  https://research.meta.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse
- Meta's Muse page: https://ai.meta.com/muse/
- TechCrunch, Meta debuts Muse (2026-09-08):
  https://techcrunch.com/2026/09/08/meta-debuts-its-muse-ai-agent-will-consumers-trust-it/
- Atlanta News First, Muse privacy concerns (2026-10-09):
  https://www.atlantanewsfirst.com/2026/10/09/metas-new-muse-ai-agent-raises-privacy-concerns/
- Fortune, the Marketplace home-address incident (2026-10-19):
  https://fortune.com/2026/10/19/meta-muse-ai-agent-facebook-marketplace-home-address-privacy/
- Yahoo Tech, the same incident and the permission prompt:
  https://tech.yahoo.com/ai/meta-ai/articles/meta-muse-ai-shared-user-120000113.html
- Digit, Muse on Instagram setup:
  https://www.digit.in/news/general/meta-muse-ai-agent-now-available-on-instagram-here-is-how-to-set-it-up.html
- Unite.AI, Muse connectors and business skills:
  https://www.unite.ai/meta-adds-small-business-skills-and-app-connectors-to-muse-ai-agent/
- Tom's Guide, Meta AI chats used for ads from 2025-12-16:
  https://www.tomsguide.com/ai/meta-will-start-using-your-ai-chats-to-target-ads-heres-what-that-means-and-why-critics-like-joseph-gordon-levitt-are-pushing-back
- Bitdefender, Instagram DMs lose end-to-end encryption on 2026-05-08:
  https://www.bitdefender.com/en-au/blog/hotforsecurity/meta-halt-instagram-end-to-end-encryption-dms
- imessage-exporter 2.7.1, encrypted iOS backups:
  https://newreleases.io/project/github/ReagentX/imessage-exporter/release/2.7.1
- Shortcuts Transaction trigger (iOS 17):
  https://matthewcassinelli.com/shortcuts-automations-ios-ipados-transaction-display-stage-manager/
- Shortcuts automations that run immediately:
  https://matthewcassinelli.com/sirishortcuts/automations
- Apple, Shortcuts user guide (automations): https://support.apple.com/guide/shortcuts/apdd711f9dff/ios
- Hermes Agent Bot Mode: https://www.marktechpost.com/2026/08/17/nous-research-hermes-bot-mode/

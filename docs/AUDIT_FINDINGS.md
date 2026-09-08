# Fryyday — Consolidated Audit Findings

**Date:** 2026-09-08 · **Status:** open · **Sources:** three independent specialist audits
(UI/UX interaction · design direction · user research), each run against the **shipped v1 Flutter
code**, not the spec.

> **How to use this file.** Every item is numbered and checkable. Nothing here is a matter of taste —
> each finding cites the file, line or schema object that produces it. Items marked 🔴 change whether
> the product works at all.

---

## 0. The three that break the product silently

These are grouped first because each one is invisible in normal use, and each one disables a system
you believe is running.

### 🔴 A1 — 40% of the trust score is the constant `1.0`

`no_show_count` is a column (§10.1), is consumed by the trust formula (§9.5), and **has no writer
anywhere** — no endpoint, no job, no screen, in either the spec or the Flutter code.

```
reliability = plansCompleted / (plansCompleted + no_show_count)
            = plansCompleted / (plansCompleted + 0)
            = 1.0   for every user, forever
```

Reliability is weighted 0.4 in the composite. **You cannot currently detect a no-show**, which is the
failure mode most likely to be killing your retention.

- [ ] Add a writer for `no_show_count` **before** building anything that depends on attendance data.

### 🔴 A2 — There is no way to leave. Anywhere.

- No leave-plan endpoint; `plan_members` has no `left_at` (§10.1)
- `request_status_t` includes `'cancelled'` and **nothing writes it** — `POST /v1/requests/:id/decide`
  takes `{accepted}` and is the *host's* action (§8.3)
- No "can't make it" in the spec or in `lib/features/` (grep, both)

**The only exit from a commitment is silence.** So users take it, feel bad, and don't return — and a
host is burned on the way out. The no-show problem is manufactured by the absence of a graceful exit.

- [ ] Ship `Can't make it` on the plan, the pending request, and in chat. No reason required. Spot reopens.

### 🔴 A3 — Path B can never record a bad outcome

`ratings` is keyed `(plan_id, rater_id, ratee_id)` (§10.1). **A group has no plan.** So a person met
through a Find — 1:1, spontaneous, proximity-matched, the highest-risk path in the product — is
structurally unrateable. Their trust score stays clean no matter what happens.

Related: `groups` has no venue column at all (§10.1). `venue_public` enforces **I3** for plans and
**does not exist for groups**. The public-venue invariant covers the daytime group path and skips the
spontaneous 1:1 path.

- [ ] Let a group generate a rateable "we met" event.
- [ ] Apply the public-venue rule to groups, or require a venue before a meet is arranged.

---

## 1. Trust signals that are currently fabricated

### 🔴 B1 — A fresh install ships pre-verified with a fake reputation
`legacy-flutter/lib/shared/data/seed_data.dart:15-29`
```dart
static const me = Person(id: 'me', name: 'You', verified: true,
  rating: 4.8, plansCompleted: 12, ratingCount: 14, ...);
```
Before typing a digit you are a HIGH-trust 4.8-star host with 12 completed plans. Verification then
announces a badge you already had. If a user notices their own rating is fake, every other rating in
the product becomes suspect.

- [ ] New user: `verified: false, rating: 0, ratingCount: 0, plansCompleted: 0`
- [ ] Stats render `—` / `NO RATINGS YET`; trust tile reads `NEW`, not `BASIC`

### 🔴 B2 — "I'm going" increments completed meetups
`fryyday_repository.dart:657` — `confirmAttendance()` does `plansCompleted + 1`, wired to the RSVP
button on *upcoming* plans. The number strangers use to decide whether to meet you is produced by a
button that means "maybe."

- [ ] Split RSVP from attendance. Only mutual post-meetup confirmation increments.

### 🔴 B3 — Plans are marked attended the second they start
`providers.dart:207` — `myPastPlansProvider` = `!p.dateTime.isAfter(now)`, and `_PastCard` renders
the `attended` chip unconditionally. An 18:30 coffee is "attended" at 18:30:01, the countdown vanishes
mid-journey, and the app asks you to rate people you may never have met.

- [ ] Three buckets: `NOW` (start−30m → start+3h) · `UPCOMING` · `PAST`. Chip only after mutual confirm.

### 🔴 B4 — Publish invents a fake public venue, then badges it as verified
`create_plan_screen.dart` has zero validation and an always-enabled button. `fryyday_repository.dart:130-137`
substitutes `'TBD · public place'` and `'${activity.label} plan'`. Plan Detail then renders that
hand-typed string with `MonoChip(label: 'public', icon: Icons.verified_user)`.

**The app manufactures exactly what invariant I3 exists to prevent, then certifies it.**

- [ ] Gate publish on `title.length >= 3 && venue.isNotEmpty && when.isAfter(now)`
- [ ] Delete both repository fallbacks
- [ ] Remove the `public` chip until a resolved place backs it
- [ ] Bound the time picker, not just the date (today + 09:00 at 6pm currently publishes into the past)

### 🟠 B5 — "Verified" does not mean what users will read
Rekognition Face Liveness proves *a live human took this selfie*. Not an identity, not a name, not an
age — `age` is self-reported behind a `CHECK (18–99)`. A 41-year-old presenting as 27 passes cleanly;
re-entry after a ban costs one new SIM.

The PRD itself says *"a trust badge that is not earned is worse than no badge."*

- [ ] Either raise the bar for **hosts specifically** (document/DigiLocker), or rename to
      "Live photo checked" with one honest line about what that proves.

---

## 2. The funnel

### 🔴 C1 — The face scan sits before any value, and is stricter than your own rule
**I2 gates hosting and Find only** (§2.3). Putting verification inside the `(auth)` group is stricter
than the invariant demands — you are paying your largest funnel cost for a rule you didn't write. v1
had a "Skip for now"; v2 removes it.

The users most likely to abandon at a face scan are those with the highest perceived social risk —
which correlates with the population you need at 35%. **This step doesn't just shrink the funnel, it
skews it.**

- [ ] Move verification behind the first host-or-Find. Browse-first, read-only.

### 🔴 C2 — The ask has no voice
`requestToJoin(Plan)` carries no message. `join_requests.message TEXT` **already exists in the schema**
(§10.1) and no screen writes it. Meanwhile `requests_screen.dart` `_pitch()` writes a sentence *on the
requester's behalf*.

The asymmetry: the **Find** path — higher-stakes, 1:1 — *has* a note field in model, schema and seed.
The lower-stakes group ask doesn't.

Consequence: the host decides on a photo, an age and a number. **That is the dating-app decision
procedure, running inside a product whose claim is that it isn't one.**

- [ ] Optional ~140-char note. Prompt: *"What made you pick this one?"* (not "anything to add?")

### 🔴 C3 — v2.0 deletes the low-cost rung that v1 already shipped
v1 has `Ping` — message, audience, pin. Its docstring: *"asking for a title, a time and a group size
before it can leave the phone is how a spontaneous idea turns into a chore."*

And `PingAudience.private` (`ping.dart:16-17`): *"Nobody browses it. Fryyday contacts one good match
directly and your message stays hidden until they answer."* **That is precisely an ask-without-being-
seen-asking primitive, already working in Dart.**

**The word "ping" appears zero times in the v2 PRD.** So does `LiveIntent` — v1's per-person broadcast
(*"Working from Third Wave till 6. Happy to share a table…"*), whose seed comment reads *"a map full
of 'Available' says nothing."*

**Deleting both makes v2 more dating-app-shaped than v1 was.**

- [ ] Restore a private, low-cost ask before v2 scope is frozen.
- [ ] Bug if restored: home default is `PingAudience.friends`, and `_peopleIHaveMet` is empty for a new
      user → the signature gesture returns "CLOSED · NOBODY IN RANGE" on day one. The demo hides this
      because `Seed.me` has 12 completed plans.

### 🟠 C4 — Decline: two problems, and the expensive one is invisible
**Loud:** `plan_detail_screen.dart` renders declined as a disabled dead-end. Note the Request Scanner
already handles the *same event* well — *"Not this time / there are other plans nearby"* + a live CTA.
Two surfaces, one event, opposite quality.

**Expensive:** §9.2's hard gate excludes anyone *"declined by this host within the cooldown (7 days)."*
In a single-neighbourhood launch with 20–30 seeded hosts, three declines silently removes ~10–15% of
her world for a week, unexplained. She reads a thinning feed as *the app is dying*.

**Also permanent:** `requestToJoin` (`fryyday_repository.dart:145-149`) checks only whether a request
*exists*, not its status — a declined user can never ask again, even if the host mis-tapped.

- [ ] Ship the scanner's treatment everywhere; never a disabled dead end.
- [ ] Decide **in writing** whether the 7-day cooldown filters the **feed** or only the host's
      **invite candidate list**. These are wildly different products.
- [ ] Allow re-request when the plan is no longer full and >2h out.

### 🔴 C5 — The 48 hours before the meetup are a locked room
One push at T−2h (§8.6) is the entire product content of this window. Combined with A2 (no exit),
anxiety has exactly one outlet: disappearing.

- [ ] **T−24h "Still on?"** — both sides answer `Yes` / `Can't make it`
- [ ] **T−30m the doorway** — venue photo, what the host looks like, who's confirmed
- [ ] **T+0 "I'm here"** — one tap, visible to the group; the honest attendance signal
- [ ] Promote the icebreakers you already wrote (`On my way!` · `Running 5 min late` ·
      `What's everyone wearing?`) **out of the chat** and into the doorway surface. Somebody
      understood this moment; the copy is buried behind an act of courage.

### 🟠 C6 — Rating can't receive bad news
Ratings are keyed `(plan_id, rater_id, ratee_id)` in groups of 2–8 — effectively non-anonymous.
Report/block sit in a separate overflow menu. So a bad experience becomes three stars, the Bayesian
average barely moves, and **the incident never enters the system.** A `→ 0` incident rate will be met
by not measuring.

- [ ] Add a private *"Something felt off"* on the same card as the stars — never moves a visible
      number, routes to moderation, invisible to the other party. Separate the reputation signal from
      the safety signal; they need opposite privacy properties.

---

## 3. Design system

### 🔴 D1 — The elevation layer does not render, and it caused the accent inflation
Measured on `#000000`: `glassFill` (white @3%) = **1.05:1** · `glassBorder` (neon @15%) = **1.35:1**.
A resting `GlassCard` is functionally invisible — on a mid-range Android outdoors, the boundary is gone.

Causal chain: no perceptible card edge → every card needs a bright element inside to be findable →
every card gets a neon icon → **neon is now referenced 299 times across 48 files.**

The rule ("two neon CTAs on one screen is a bug") wasn't broken by carelessness. **Fix elevation and
the accent economy repairs itself.**

Corroboration: 8 raw hex values outside the theme — `#14150E`, `#15160E`, `#17180D` — are **all warm/
olive (G > R > B)**, the opposite direction from the blue-biased tokens. Three developers independently
reached past the token file for a warmer near-black.

- [ ] Warm the neutral ramp; add `surfaceRaised`; give cards a real fill and a real hairline.

### 🔴 D2 — The governing metaphor is surveillance, not nightlife
From the code's own comments: `radar_scanner.dart` — *"a **crosshair**… pulsing blips"*, parameter
documented as *"how many nearby **contacts**."* `signal_lock_scanner.dart` — *"a **reticle**… **snap
inward on lock**,"* explicitly *"a reticle closing on a face."*

**Seven surfaces** are built on radar/sonar/targeting. `verification_screen.dart` draws neon corner
brackets around the user's face with *"Hold still while we verify your face"* — the composition and
copy of a border checkpoint, at the exact moment she is deciding whether to trust you.

- [ ] Reverse the vector: brackets **expand and dissolve** on acceptance. A door opening, not a scope closing.
- [ ] Rename the seven "scanner" surfaces.

### 🟠 D3 — The imagery contradicts the product
- Hero tile is **a lone woman walking away down an empty street at dusk**, labelled `WALK`.
- `activity_tile.dart` applies `alpha: 0.55` darken **then** an `0.8` scrim — **no human face survives
  the image system**, in an app selling "there are real people here."
- 15 Unsplash portraits as avatars for "verified members," including one of the most reused stock
  headshots in tech. `pubspec.yaml` has **no `image_picker` and no `camera`** — the face-scan
  verification uses no camera at all.
- Your seed data is daylight (morning coffee, Cubbon Park walk, filter coffee). Your photography is
  dusk and night.

- [ ] Daylight only · people plural, faces visible · never a person alone or from behind
- [ ] Bottom-only scrim, ~45% height
- [ ] Real photo capture; monogram placeholder, never a stranger's face

### 🟠 D4 — Safety has no visual currency
Verified, women-only, trust tier and SOS are rendered in stock Material icons and **the same neon that
marks the pickleball tile**. The one signal that must read differently is drawn identically to
everything else.

- [ ] Add a `trust` token (cool, calm, deliberately *not* the accent). Reserve `#EAFF00` for action only.
- [ ] Commission one custom verification glyph. `Icons.verified` is Google's and means nothing.

### 🟡 D5 — The system is improvising
- **22 font sizes** against a documented 7; **75 of ~111** text instances are ≤11px; display sizes
  used exactly once each.
- `AppSpacing` is labelled 8px-based and contains no 8 and no 16 → patched at the call site **65 times**.
- Elevation is two ideas: 4 black `BoxShadow`s survive, including on `command_bar.dart:72-75`, the
  most important control in the product.
- `command_bar.dart:103` — **`TextCapitalization.characters`** forces the user's own words into
  uppercase. Two other screens correctly use `.sentences`. **One-line fix.**

---

## 4. Craft and accessibility

- [ ] 🟠 **E1** — `Semantics(` appears 5 times, `semanticLabel` **zero**. Every primary CTA is a raw
      `GestureDetector`; TalkBack reads a *disabled* button identically to an enabled one. One change
      in `NeonButton` + `MonoChip` covers most of the app.
- [ ] 🟠 **E2** — `MonoChip` is a **24pt** target and it is the most-used control (interests, gender,
      join rule, the entire Feed filter bar). `_Action` accept/decline is 40pt. Header icons 40pt.
- [ ] 🟠 **E3** — Form borders **1.15:1**, placeholders **1.9:1**. The PRD does this contrast maths
      and the components ignore it.
- [ ] 🟠 **E4** — CTAs enabled with empty required fields on 4 screens. `NeonButton` implements
      disabled correctly; the screens just never pass `null`. `ping_composer_screen.dart:204` does it
      right — copy that line.
- [ ] 🟠 **E5** — `ping_composer_screen.dart` `_locate()` silently swallows location denial
      (`if (fix == null) return;`) and the pin stays on the user's **saved home area**, which is about
      to be broadcast. `live_map_screen.dart:1305` has an excellent `_DenialBanner` 200 lines away —
      lift it into `shared/`.
- [ ] 🟠 **E6** — OTP: step dots frozen at 1, any 6 digits pass, no wrong-code state, no lockout,
      `RESEND` is a ~14pt `GestureDetector` on a `Text`.
- [ ] 🟡 **E7** — Zero loading skeletons anywhere; the Feed's pull-to-refresh is
      `Future.delayed(600ms)` and refetches nothing.
- [ ] 🟡 **E8** — Offline does not exist as a concept in any core screen. No send tick, no queue.
- [ ] 🟡 **E9** — No error branch (`catch`) anywhere in `lib/features/` outside `location_service.dart`.
- [ ] 🟡 **E10** — SOS dialog has no **`Call 112`**. In India that is the number; she should not have
      to leave the app to find it.
- [ ] 🟡 **E11** — `skip_registration_button.dart` puts `SKIP` in the onboarding AppBar at the same
      weight as the real path. Debug builds only.
- [ ] 🟡 **E12** — No other-person profile exists. `InviteCard` **has** an `onTapPerson` parameter and
      `requests_screen.dart` never passes it. Hosts admit strangers to physical meetups based on a
      46pt avatar.

---

## 5. Contradictions to resolve in writing

| # | Contradiction | Sources |
|---|---|---|
| **X1** | Does `requestToJoin` cost a credit? PRD §9.1 says pull types are free · `invite_type.dart` says `creditCost: 1` · `fryyday_repository.dart` never charges it | 3-way |
| **X2** | Credits are *"an anti-spam budget, not a paywall"* (§2.1) — but S23 Wallet sells **packs** and `buyCredits()` is implemented. Both cannot be true; a bad actor buys past the anti-spam mechanic while a shy user can't afford to be shy | §2.1 vs §6.2 |
| **X3** | Home tiles: PRD S6 says they pre-fill **Create Plan**; shipped code routes to the **Ping composer**. Two products are fighting over the same five tiles | §5.4 vs `home_launch_pad.dart` |
| **X4** | Does the 7-day decline cooldown filter the **feed** or only **invite candidates**? | §9.2, unstated |

**On X1:** the recommendation is that requesting to join is **free and uncapped** — I1 is already the
throttle, and a spammer declined three times a day is neutralised at zero cost. Credits are regressive
with respect to anxiety: they tax the hesitant user (who deliberates, spends nothing, and leaves with
a full wallet) and never touch the confident one. Reserve credits for **initiated, push-eligible**
contact — `directInvite`, `lastMinute`, `groupInvite` — which is the actual unsolicited-contact vector.

---

## 6. The launch-sequencing decision

**This is the only item that cannot be fixed later.**

The mechanism, concretely: a woman installs on day 3, hands over phone, face, gender and location,
lands on Home where the offered action is *host a plan*, opens Feed (plans hosted by men), opens
Discover (a proximity-sorted grid of faces, mostly men, with a "Find" button under each), and looks
for the women-only filter — **which requires the host to be a woman** (§6.2 S11). There are none.

**The one safety feature built for her requires its own outcome as an input.**

She closes the app. She doesn't report, rate, or angrily uninstall. In analytics she is an install with
zero requests, indistinguishable from a distraction. **You never see her leave**, and nothing about the
state has changed, so the next woman has an identical experience.

This is not a normal cold start. What she needs is not supply — it is **visible co-presence**, which
only women showing up creates. **It is not a curve you climb; it is a fixed point you must be placed
at.** Every week run open moves you further from it, and it eats the supply side too: a host whose
coffee fills with four male strangers has had a bad time and doesn't host again.

- [ ] Treat **35% as a launch gate, not a metric** — measured in the seeded cohort, before one organic
      user is admitted
- [ ] Hand-recruit the seed cohort **≥60% women**, one neighbourhood (yoga studios, run clubs, book
      clubs, women's coworking, alumni groups)
- [ ] **Waitlist men for the first weeks.** Letting men in at week 1 is not neutral — it is an
      irreversible decision about what the product is
- [ ] Make women-only reachable on day one — seed women-hosted women-only plans, or decouple the join
      rule from host gender
- [ ] **Ship Path B off or restricted.** 1:1, spontaneous, proximity-sorted, no venue guarantee, no
      trust data — it is the single feature that makes Fryyday legible as a dating app in the first
      thirty seconds. Turn it on once Path A has a track record. *(Also buys back M5 of schedule.)*

**The readiness test, and it doubles as the first screen after onboarding:**
> *"8 of the 12 plans near you are hosted by women."*
> **If that sentence isn't true, you are not ready to launch to women.**

---

## 7. What is genuinely good — protect it

All three audits independently flagged these. A redesign will quietly destroy them if nobody says so.

1. **The Request Scanner** — *"the best screen here, not close."* Its progress arc is bound to
   `FryydayRepository.hostDecisionDelay`, the same constant the decision actually fires on, **so the
   instrument cannot lie.** And *"Chat unlocks the moment they accept — not before"* teaches I1 at the
   one second the user cares. Keep the sonar. Keep that sentence.
2. **The `_JoinCta` state machine** (`plan_detail_screen.dart:190-243`) — derived purely from store
   state, no local `isRequested` boolean. It already avoids the exact bug PRD §6.3 was written to
   prevent. Port as-is.
3. **The activity-first framing.** Not branding — the only frame under which the target user can use
   this without a status cost. Every deletion flagged in C3 moves away from it.
4. **`features/map/`** — `maxZoom 16.5` clamped as *"a privacy control, not a rendering preference"*,
   your own position the only full-precision point on the map, opacity encoding staleness. Called
   *"safety product design of a genuinely high standard, currently wearing the wrong clothes."*
5. **`InviteCard`** — one card, four facts, same order, with a doc comment explaining why no screen may
   drop the age or the message.
6. **`_pitch()` in `requests_screen.dart`**, which explicitly refuses to invent a quote the requester
   never wrote — editorial integrity expressed in code.
7. **`_DenialBanner`** — five distinct denial reasons, each saying what happened and what the app did
   instead. The error-state standard for everything else.
8. **`_EmptyGroups`** — *"the only correct empty state in the app."* The template.
9. **PRD §5.5 accessibility gate** — real ratios, "never colour alone" with its rationale, reduced-motion
   mapped to specific animations. *"Most teams write 'we care about a11y.' You wrote merge criteria."*
10. **The Bayesian shrink** and the **one-neighbourhood launch**. Both unglamorous, both correct, both
    will be argued against by someone.
11. **`#EAFF00`, the bolt mark, the Launch Pad masonry.** Distinctive, ownable, not any competitor's.
    Keep exactly.

---

## 8. If you do five things

1. **Move the face scan behind first value.** Gate hosting and Find only — your own I2. *(C1)*
2. **Put a text field on the request.** The column already exists. *(C2)*
3. **Ship "Can't make it" — and make `no_show_count` writable**, so you can see the thing that is
   killing you. *(A1, A2)*
4. **Restore a private, low-cost way to ask.** It is already written in Dart. *(C3)*
5. **Don't launch open.** Assemble the first cohort by hand, women-first, and hold men on a waitlist
   until the first screen a woman sees is majority women. *(§6)*

Items 1–4 are roughly **two weeks** between them and sit on the largest leaks.
**Item 5 is free, and it is the only one that cannot be done later.**

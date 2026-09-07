# FRYYDAY — Full Screen Design Specification

> Generated from a scan of the existing homepage (Stitch export) + the Fryyday PRD v1.0.
> Design system: **"Striking Lightning"** — Dark glassmorphism × high-contrast cyberpunk.
> Target build: **Flutter** (Android + iOS, single codebase), Firebase/Supabase backend.

---

## PART A — Homepage Scan (Teardown)

The current homepage (`stitch_fryyday_ai_invitation_interface (5)/code.html`) is the **"Launch Pad"** —
a spontaneous, one-tap activity launcher. Structure top→bottom:

### A.1 Layout anatomy
| Region | Content | Notes |
|--------|---------|-------|
| **Fixed header** | `menu` icon · **FRYYDAY** wordmark (italic, extrabold, neon) · circular avatar with gradient ring | `bg-black/60` + `backdrop-blur-2xl`, 1px bottom hairline |
| **Atmospheric BG** | Two large radial neon glows (`blur-120px`, 5% opacity) | Pure-black canvas, sits behind everything |
| **Activity masonry grid** | 6×6 grid, `aspect 4/5` mobile → square desktop, max 60vh | 5 glass tiles, photo bg @ 40% opacity, neon icon + mono label, hover lifts + glows |
| **Command bar** | Audience `select` (Friends/Group/Public) · `add` · text field `"INVITE NOW / ASK ANYTHING"` · neon send FAB w/ shimmer | The single most important control — the "create plan in <60s" promise |
| **Footer** | `OBSIDIAN CORE v2.0 • FRYYDAY LAB` mono microtext | Decorative |

### A.2 Masonry grid map (6 cols × 6 rows)
```
┌───────────┬───────────┐
│           │  COFFEE   │  WALK  = col-span-3 row-span-4 (large vertical)
│   WALK    ├───────────┤  COFFEE= col-span-3 row-span-2
│           │ PICKLEBALL│  PICKLE= col-span-3 row-span-2
├───────┬───┴───────────┤  MOVIE = col-span-2 row-span-2
│ MOVIE │    TRAVEL     │  TRAVEL= col-span-4 row-span-2
└───────┴───────────────┘
```
Icons (Material Symbols): `directions_walk`, `local_cafe`, `sports_tennis`, `movie`, `flight`.

### A.3 Interaction signatures (must port to Flutter)
- **Glass card**: `linear-gradient(135°, white 3% → 1%)` + `blur(20)` + `1px` border `neon@15%`.
- **Hover/press**: border → `neon@60%`, `translateY(-2px)`, outer glow `0 0 30px neon@10%`.
- **Send FAB**: solid neon, black icon, `shimmer` sweep every 6s, glow `0 0 20px neon@30%`.
- **Focus**: command bar border → `neon@50%` + inset neon glow.

### A.4 Verdict
The homepage optimises for **one job**: launch an activity instantly. It is the app's
"Create Plan" entry point (PRD §6.1) fused with the AI/quick-ask command bar. Every other
screen hangs off this hub.

---

## PART B — Product Model (from PRD)

**Fryyday** = a safety-first app that turns nearby verified people into real, in-person plans
(coffee, walk, games, travel, movie). Not dating, not noisy group chats.

- **Core loop**: Create plan → nearby users see it (map/feed) → request to join → host accepts →
  group chat opens → meet → both confirm + rate.
- **Safety gate**: *accept-before-chat*. No message reaches anyone who hasn't accepted.
- **Invite credits**: spending a credit only when *you* initiate 1:1 contact (anti-spam, not paywall).
- **Roles**: Member (host/join), plus later Venue/Organiser/Business (P2–P3).

---

## PART C — Design System ("Striking Lightning")

### C.1 Color tokens
| Token | Hex | Use |
|-------|-----|-----|
| `background` | `#000000` | App canvas |
| `surface` | `#0A0A0B` | Base surface |
| `surfaceBright` | `#121214` | Raised solid surface |
| `primary` (neon) | `#EAFF00` | CTAs, active states, icons, status — **use sparingly** |
| `onPrimary` | `#000000` | Text/icon on neon |
| `onSurface` | `#E3E2E7` | Primary text |
| `onSurfaceVariant` | `#C7C9AB` | Secondary text |
| `outline` | `#919378` | Borders/labels |
| `glassFill` | `white @ 3%→1%` gradient | Card fill |
| `glassBorder` | `#EAFF00 @ 15%` (→60% active) | Card border |
| `success` | `#00C853` | Confirmed / verified |
| `error` | `#FF1744` | Decline / report |

Light variant ("Striking Lightning Light", from export #4) kept for future light mode:
`background #F8F9FA`, `primary #EAFF00` on dark text, `onSurface #191C1D`.

### C.2 Typography
- **Display/Body**: `Hanken Grotesk` (400 / 600 / 800).
- **Mono/labels**: `JetBrains Mono` (500 / 700) — uppercase, `letterSpacing 0.05–0.2em`.
- Scale: `displayLg 56/800/-0.02em` · `headlineLg 32/700` · `titleMd 20/600` ·
  `bodyLg 16/400/1.6` · `bodySm 14/400` · `labelMd 12/500 mono`.

### C.3 Shape & spacing
- Radius: tag `4px` · button/input `8px` · card `12–20px` · pill `full`.
- Spacing (8px base): `xs 4 · sm 12 · md 24 · lg 48 · xl 80` · gutter `24`.

### C.4 Elevation (no drop shadows — glass + glow)
- L0 solid `#000`; L1 glass blur-20 + neon@15 border; L2 active = neon@60 border + outer glow.

### C.5 Component library (to build as reusable widgets)
`GlassCard` · `NeonButton` (primary/ghost) · `CommandBar` · `ActivityTile` · `MonoLabel/Chip` ·
`NeonTextField` · `Avatar` (gradient ring) · `CountdownTimer` · `StatusDot` · `SosButton` ·
`SectionDivider` (gradient hairline) · `FryydayAppBar` · `FryydayBottomNav`.

---

## PART D — Full Screen Inventory (16 MVP screens)

Each screen: purpose · key elements · components · neon/glass treatment.

### D1. Splash / Onboarding
Brand reveal (animated FRYYDAY wordmark + lightning flicker), 3-slide value prop
(safe · spontaneous · real), `Get Started` neon CTA. Atmospheric glow bg.

### D2. Phone + OTP
Number entry (country code + NeonTextField), `Send code` → 6-box OTP with neon focus-fill,
resend timer (mono countdown). Power-up flicker on verify success.

### D3. Profile Setup
Photo upload (gradient-ring avatar), name, age, bio, **interest chips** (coffee/walk/sports/
travel/movie/gym — mono pills, neon when selected), neighbourhood picker. Stepper progress (neon flicker bar).

### D4. Verification
Selfie capture frame (neon corner brackets) + ID capture, status states
(pending/verified/failed) with `success` badge. Trust-tier explainer card.

### D5. Home / Launch Pad  ⭐ (the scanned homepage)
Activity masonry grid + Command bar (audience select · add · "INVITE NOW / ASK ANYTHING" · send FAB)
+ FryydayAppBar + atmospheric glow. This is the hub. (Bottom nav optional — see export #2.)

### D6. Feed
Vertical list of nearby plans (GlassCard rows: activity icon, title, host avatar, time chip,
distance, "X going", Request-to-Join). Filter bar (interest + time mono chips). Pull-to-refresh.

### D7. Map (Home alt view)
Full-bleed dark map, neon plan pins (clustered), filter bar, central `Create Plan` FAB,
bottom peek-sheet of the selected plan.

### D8. Plan Detail
Hero (activity image + gradient scrim), title, host card (avatar/rating/verified), venue row
(public-venue badge), time + countdown, "who's going" avatar stack, join-rule chip,
**Request to Join** neon CTA, Report/Block overflow.

### D9. Create Plan
Activity-type grid (reuse ActivityTile), time picker, venue picker (map search),
group size stepper, join rules (Everyone / Women-only / Skill level), preview card, **Publish** FAB.

### D10. Discover People
Opt-in nearby users as glass cards (avatar, interests, mutuals, distance), `Send invite` (mono:
"uses 1 credit"). Empty/consent state if discoverability off.

### D11. Requests / Inbox
Tabs: **Incoming** / **Outgoing**. Cards with Accept (neon) / Decline (ghost-error).
Mirrors light export #4 "ACTIVE REQUESTS" — featured invitation hero + pending list with "X waiting".

### D12. Chat (group & 1:1)
Pinned plan header w/ live **CountdownTimer** + LIVE status (per export #1 & #2), message list
(neon sender labels, glass bubbles), shared media/map cards, icebreaker chips, SOS access in header,
input bar (mirrors command-bar styling).

### D13. My Plans
Tabs: Upcoming / Past. Upcoming = countdown cards + "I'm here" confirm; Past = rate-each-other
flow (neon star rating) + "attended" badge.

### D14. Profile (self & others)
Gradient-ring avatar, name + verified badge, stats row (rating · plans completed · trust tier),
interests chips, plan history. **Self**: edit + settings entry. **Other**: invite / report / block.

### D15. Wallet / Credits
Credit balance (big mono number + neon), buy packs (glass tiles w/ price), premium upsell card
(featured glow), transaction ledger list.

### D16. Settings + Safety Centre
Settings: discoverability toggle, privacy, notifications, language (EN/HI), data deletion.
Safety Centre: guidelines, **SOS**, share-my-live-plan, report history, blocked users.

### Global navigation
Bottom nav (from export #2): **Home · Feed/People · Requests · Profile/Settings** (4 tabs),
with Home center-weighted. Compass icon = discover.

---

## PART E — Flutter Architecture & Build Plan

```
lib/
├── main.dart
├── app.dart                       # MaterialApp.router, theme, routes
├── core/
│   ├── theme/
│   │   ├── app_colors.dart        # Striking Lightning tokens (dark + light)
│   │   ├── app_typography.dart    # Hanken Grotesk + JetBrains Mono TextThemes
│   │   ├── app_spacing.dart       # 8px scale + radii
│   │   └── app_theme.dart         # ThemeData (dark default) + ThemeExtension<Glass>
│   ├── router/app_router.dart     # GoRouter, all 16 routes
│   └── utils/
├── shared/widgets/                # GlassCard, NeonButton, CommandBar, ActivityTile,
│                                  # NeonTextField, Avatar, CountdownTimer, MonoChip,
│                                  # FryydayAppBar, FryydayBottomNav, SectionDivider, StatusDot
└── features/
    ├── onboarding/ (splash, phone_otp, profile_setup, verification)
    ├── home/       (home_launch_pad, feed, map)
    ├── plans/      (plan_detail, create_plan, my_plans)
    ├── discover/   (discover_people)
    ├── requests/   (requests_inbox)
    ├── chat/       (chat_screen)
    ├── profile/    (profile)
    ├── wallet/     (wallet)
    └── settings/   (settings, safety_centre)
```
- **State**: Riverpod (lightweight controllers; mock repositories with seeded data so every screen renders).
- **Fonts/packages**: `google_fonts`, `go_router`, `flutter_riverpod`, `flutter_animate`
  (shimmer/flicker/staggered reveals), `cached_network_image`.
- **Quality bar**: responsive (mobile→tablet), const widgets, Semantics, dark-first,
  2–3 meaningful animations per key screen (shimmer FAB, staggered tile reveal, countdown pulse,
  OTP power-up flicker).

### Build phases
1. **Foundation** — pubspec, theme system, shared widget library.
2. **Home / Launch Pad** — pixel-faithful port of the scanned homepage (the ⭐ screen).
3. **Core loop** — Feed, Plan Detail, Create Plan, Requests, Chat.
4. **Identity** — Splash, Phone/OTP, Profile Setup, Verification.
5. **Remaining** — Map, Discover, My Plans, Profile, Wallet, Settings + Safety.

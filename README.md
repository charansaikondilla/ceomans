# Fryyday — Production Specification

> ### ⚠️ PROTOTYPE
> **This repository is a specification and design prototype, not a running application.**
> It documents the complete production build of Fryyday: what it is, how it works, how it is
> designed, and how it gets to the App Store and Google Play. **No application code is
> implemented yet** — §16 of the PRD is the plan for building it.

**📄 Read the spec: [charansaikondilla.github.io/ceomans](https://charansaikondilla.github.io/ceomans/)**

---

## What Fryyday is

A **safety-first, activity-first social app** that turns nearby verified people into real
in-person plans — coffee, a walk, pickleball, a movie, travel.

Dating apps optimise for matching. Messaging apps optimise for talking.
**Fryyday optimises for actually showing up somewhere.**

```
Create a plan → nearby people discover it → request to join → host accepts
   → chat unlocks → meet at a public venue → both confirm + rate → trust rises
```

A feature that does not move a user one step around that loop does not ship.

## The five safety invariants

These are the product. All five are enforced **server-side**, and each has a named test that
fails the build if broken.

| | Invariant |
|---|---|
| **I1** | **Accept-before-chat.** Nobody can message you until *you* accepted them |
| **I2** | **Verified-first.** Identity verification gates hosting and 1:1 connections |
| **I3** | **Public venues by default** |
| **I4** | **Consent is revocable and cheap.** Block, report, quiet hours, women-only — one tap |
| **I5** | **No location leakage.** The API returns a distance band, never another user's coordinates |

## Stack

| Layer | Choice |
|---|---|
| **Mobile** | React Native · Expo SDK 54 · TypeScript · expo-router · TanStack Query · Reanimated · Skia |
| **Backend** | NestJS 11 · Fastify · Prisma · Socket.IO · BullMQ |
| **Data** | PostgreSQL 16 + PostGIS 3.4 · ElastiCache Valkey |
| **Cloud** | AWS — ECS Fargate · RDS · S3 + CloudFront · Cognito · Rekognition |
| **IaC** | AWS CDK (TypeScript) — the entire cloud in one `cdk deploy` |
| **Design** | "Striking Lightning" — true black, neon `#EAFF00`, glassmorphism, glow over shadow |

One codebase ships to **iOS and Android**.

## Repository contents

```
index.html                    the hosted specification (GitHub Pages)
docs/FRYYDAY_PRD.md           full PRD — the single source of truth
design/FRYYDAY_DESIGN_SPEC.md design system + screen inventory
design/stitch-exports/        original UI design exports
```

## Status

| | |
|---|---|
| Specification | ✅ Complete — PRD v2.0 |
| Design system | ✅ Defined — tokens, components, accessibility rules |
| Application code | ⬜ Not started — 14-week plan in PRD §16 |
| AWS infrastructure | ⬜ Not started — architecture in PRD §13 |

**Next step:** M0 — monorepo, CDK skeleton, dev environment. And **start TRAI DLT
registration in week 1**; it gates SMS delivery in India and takes 1–3 weeks.

---

*Aiverse · 2026*

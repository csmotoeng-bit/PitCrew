# PitCrew — Operating Rules

**PIT CREW: British Shitbox Championship** — a 2–6 player online
cooperative physics-comedy motorsport game built in Unreal Engine 5.8.x
for Windows PC. Players run a broke racing team through an eight-round
fictional British Shitbox Championship (BSC) season: garage prep, physical
pit stops, short races, damage, bodged repairs, and a tight economy,
played first-person on foot with first-person/cockpit driving.

Full canonical design lives in **`GAME_DESIGN.md`**. Full technical
architecture lives in **`ARCHITECTURE.md`**. Phased delivery plan and
acceptance gates live in **`ROADMAP.md`**. These three documents are
canonical — this file is a summary and operating checklist, not a
replacement for them.

## Non-negotiable design facts

- Multiplayer (2–6, player-hosted/listen-server) is foundational, never
  retrofitted.
- No permanent player classes. Any garage player can do any physical
  garage/pit task.
- One human drives; exactly one non-driving player is Crew Chief per
  on-track session (role rotates between sessions).
- The Crew Chief tablet is a **physical object inside the Unreal world**.
  There is no external app, second screen, website, or QR pairing —
  ever, unless the user explicitly instructs otherwise.
- Driver has deliberately limited info; Crew Chief has privileged
  telemetry and must relay it verbally. Discord is the canonical voice
  solution; do not build in-game voice chat unless specifically
  instructed.
- Gameplay should be physical, not menu-driven, wherever practical.
- Comedy emerges from systems and player actions, not scripted jokes.

See `GAME_DESIGN.md` for the full weekend loop, championship calendar,
BRASS (the fictional governing body), damage, bodging, economy,
progression, and explicit out-of-scope items.

## Development workflow

- Cameron is the owner and primary playtester.
- ChatGPT acts as lead designer / technical director / phase controller.
- Claude Code is the primary local implementation agent.

## Rules for Claude

1. Inspect before editing.
2. Keep changes scoped to the current requested task.
3. Do not opportunistically build future roadmap features.
4. Preserve working systems unless a change is necessary.
5. Prefer reusable, data-driven systems over one-off hacks (see
   `ARCHITECTURE.md`).
6. Treat multiplayer/replication as foundational; server/host stays
   authoritative for important gameplay state. Never trust client-side
   state for authoritative actions.
7. Maintain keyboard/mouse AND gamepad support at all times, via Unreal
   Enhanced Input.
8. Build/test after meaningful C++ changes where practical.
9. Read compiler errors and attempt to correct the implementation.
10. Clearly report anything requiring manual Unreal Editor testing.
11. Never claim something was tested if it was not.
12. Never silently change canonical design — flag conflicts with
    `GAME_DESIGN.md`, `ARCHITECTURE.md`, or `ROADMAP.md` instead of
    reinterpreting them.
13. Do not introduce external companion apps for the Crew Chief.
14. Do not add in-game voice chat unless specifically instructed.
15. Avoid committing automatically unless explicitly asked.
16. Do not push to remote unless explicitly asked.
17. Do not delete Unreal assets unless explicitly permitted.
18. Avoid editing binary `.uasset`/`.umap` files outside supported Unreal
    workflows.
19. Avoid unnecessary plugins or new dependencies without explaining why.
20. Keep the repository buildable at phase gates (see `ROADMAP.md`).

## Roadmap at a glance

Phase 0 Foundation → Phase 1 Networked Player Sandbox → Phase 2 Replicated
Physics Interaction → Phase 3 Pit Garage Vertical Slice → Phase 4
Driveable Shitbox → Phase 5 Complete Race Weekend → Phase 6
Damage/Repair/Chaos → Phase 7 Bodge/Friendslop Polish → Phase 8 Crew Chief
Tablet → Phase 9 Championship/Economy → Phase 10 Content Production →
Phase 11 Frontend/Online Friends → Phase 12 Art/Audio/UX → Phase 13
Balance/Optimisation/QA → Phase 14 Friends Build → Phase 15 Commercial
Release Readiness (if later wanted).

Each phase must end with a **working, testable build** demonstrating that
phase's required functionality before moving on. Full objectives,
deliverables, and acceptance tests per phase are in `ROADMAP.md`.

# PIT CREW — Technical Architecture

This document describes technical architecture principles for PitCrew. It
is a design reference, not an implementation plan — it does not create
source files and should not be treated as authorization to do so. See
`GAME_DESIGN.md` for gameplay design and `ROADMAP.md` for phased delivery.

## Principles

1. Multiplayer and replication are foundational, never retrofitted.
2. The server/host is authoritative for all important gameplay state.
   Client-side state is never trusted for authoritative actions (physics
   pickup ownership, damage, race results, economy, championship
   progression).
3. C++ carries core/networked gameplay systems. Blueprint and Data Assets
   carry content configuration, presentation, and tuning — not core
   architecture.
4. Avoid unnecessary plugins and external dependencies. Any new dependency
   must be explained.
5. Shared, data-driven systems over one-off per-object/per-vehicle code
   (e.g. one vehicle class configured by data assets, not six vehicle
   classes).
6. Keep the repository buildable at every phase gate.

## Engine & Platform

- Unreal Engine 5.8.x, C++ primary, Windows PC target.
- Player-hosted/listen-server networking model initially. Dedicated server
  support is not precluded by this choice but is not an initial goal.
- Steam/EOS or equivalent session backend is a Phase 11 (Frontend/Online
  Friends) concern, not decided here.

## Proposed High-Level Module / Class Responsibilities

This is a proposed shape for early phases, to be refined as each phase is
actually implemented — not a commitment to create these files now.

### Core gameplay module (C++)

- **PitCrewCharacter** — first-person player pawn: movement, look,
  interaction trace/query, inventory-of-one-held-item, ragdoll/impact
  reaction hook.
- **PitCrewPlayerController** — Enhanced Input binding, input-to-action
  routing, driver/Crew Chief context switching.
- **PitCrewPlayerState** — per-player session data: current role (driver /
  Crew Chief / crew), championship-persistent identity.
- **Interactable interface/component** — grab, carry, drop, throw contract
  implemented by physical props (wheels, jacks, wheel guns, fuel
  containers, tools, panels). Server-authoritative attach/detach and
  physics handoff.
- **VehicleBase** — shared drivable vehicle class (Chaos Vehicles or
  equivalent), configured by a Vehicle Data Asset rather than subclassed
  per car.
- **DamageComponent** — modular damage state (tyres, suspension, brakes,
  bodywork, cooling, engine, fuel) attached to vehicles; replicated;
  read by driving feel, Crew Chief telemetry, and repair interactions.
- **PitStopSubsystem / PitBoxActor** — pit box detection, jack/wheel/fuel
  state machine, unsafe-release and infringement detection, hookup to
  BRASS penalty system.
- **RaceSessionSubsystem** — session type (practice/qualifying/race)
  state, lap/sector timing, grid, results, AI rival car management.
- **CrewChiefTabletActor/Component** — in-world interactable tablet;
  server-authoritative privileged data channel to the assigned Crew Chief
  only (see below).
- **ChampionshipSubsystem** — season state, calendar progression, points,
  standings; persists via the save architecture.
- **EconomySubsystem** — money, income/costs, purchases, fines, emergency
  finance.

### Content/config layer (Blueprint & Data Assets)

- Individual vehicle variants (stats, mesh/material refs, audio) as Data
  Assets consumed by `VehicleBase`.
- Individual tool/prop variants as Data Assets consumed by the
  Interactable framework.
- Track configuration (waypoints, pit box transforms, start grid,
  circuit-specific hazards) as level data + Data Assets, not C++ per
  track.
- Visual presentation, VFX/audio hookups, HUD widget layout.
- Circuit-specific characteristic tuning (weather bias, grip, hazards)
  as data consumed by shared systems.

Core architecture must not become dependent on large Blueprint-only
systems — Blueprint composes and configures; C++ owns the rules.

## Networking Authority Rules

- The host/server owns and validates: physics object ownership/attach
  state, damage state, pit stop legality and outcomes, race timing and
  results, penalties, economy transactions, championship state, Crew
  Chief role assignment.
- Clients request actions (grab, attach wheel, tighten nut, start pit
  sequence); the server validates and replicates the resulting state.
- Crew Chief privileged data is delivered server-to-client only to the
  connection currently holding the Crew Chief role for that session; it is
  not broadcast to all clients and is not derivable from client-only
  state.
- Physics interactions (grab/carry/drop/throw, wheel fitting, jacking)
  must use server-authoritative ownership handoff to avoid desync and
  cheating, consistent with Unreal's physics replication/ownership model.

## Enhanced Input Architecture

- One shared Input Mapping Context per major control scheme (on-foot,
  driving, tablet/UI), swapped based on player state (on foot vs. in
  vehicle vs. using tablet).
- Keyboard/mouse and gamepad both bound from the start; no gamepad-only or
  KBM-only actions in core gameplay.
- Input Actions are defined once and reused across contexts where the
  semantic action is the same (e.g. "Interact"); context determines which
  mapping context is active, not duplicate actions.
- All actions intended for remapping are declared through the standard
  Enhanced Input asset flow so a future remapping UI (Phase 11) can
  enumerate and rebind them without core code changes.

## Save Architecture Concept

- Host owns the persistent Championship save (single source of truth;
  matches the player-hosted model).
- Save data is a serialized snapshot of `ChampionshipSubsystem` and
  `EconomySubsystem` state: calendar position, points/standings, money,
  owned equipment/upgrades, vehicle condition carried between rounds.
- Save/load points are between weekend-loop stages (round boundaries),
  not mid-session, to avoid needing to serialize live physics/race state.
- Use Unreal's `USaveGame` object flow; no external database or backend
  dependency for the friends-build target.

## Crew Chief Tablet Architecture Concept

- The tablet is a physical, grabbable/placeable Interactable actor in the
  world — not a UI overlay independent of world presence.
- Its screen is an in-world-rendered UI (e.g. render target or 3D widget
  component) so other players can see it exists and see the Crew Chief
  physically holding/using it; no second-screen or external process is
  ever involved.
- Data displayed comes from a server-authoritative telemetry channel
  (timing, gaps, fuel, temps, tyre state, damage, weather, pit-window,
  penalties, BRASS messages) pushed only to the client currently holding
  Crew Chief role.
- Role assignment (who is Crew Chief) is host/server-authoritative session
  state, re-evaluated at session boundaries (practice/qualifying/race),
  matching the design rule that Crew Chief is temporary and rotatable.
- The driver's HUD and other players' views intentionally do not receive
  this feed, enforcing the information asymmetry that drives the
  verbal-relay gameplay.

## C++ vs Blueprint/Data Asset Boundary — Summary

| Concern | Owner |
|---|---|
| Replication, authority, validation | C++ |
| Interaction/physics rules | C++ |
| Vehicle/damage/race/economy/championship logic | C++ |
| Session/save state | C++ |
| Individual vehicle stats & appearance | Data Asset |
| Individual tool/prop variants | Data Asset |
| Track layout & circuit tuning | Data Asset / level data |
| HUD/tablet visual layout, VFX, audio hookups | Blueprint/UMG |
| One-off cosmetic behaviour | Blueprint |

## Testing / Build Philosophy

- Build and, where practical, test after meaningful C++ changes.
- Compiler errors are read and corrected by the implementing agent before
  reporting work as done.
- Anything requiring manual testing in the Unreal Editor (PIE multiplayer,
  gamepad input, physics feel, visual/audio results) is explicitly called
  out as needing manual verification — never claimed as tested if it was
  not.
- Each roadmap phase must end with a working, testable build demonstrating
  that phase's required functionality before the next phase begins (see
  `ROADMAP.md` Phase Gate Rule).

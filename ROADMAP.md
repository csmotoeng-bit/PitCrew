# PIT CREW — Roadmap

See `CLAUDE.md` for operating rules, `GAME_DESIGN.md` for canonical design,
and `ARCHITECTURE.md` for technical architecture.

## Phase Gate Rule

Do not progress merely because code exists. **Each phase must finish with
a working, testable build demonstrating that phase's required
functionality** before the next phase begins.

---

### Phase 0 — Foundation

**Objective**: A clean, buildable project skeleton with tooling and
canonical documentation in place.

**Deliverables**: UE5/Visual Studio project; C++ project structure;
Git/Git LFS; GitHub remote; Claude Code set up; canonical docs
(`CLAUDE.md`, `GAME_DESIGN.md`, `ARCHITECTURE.md`, `ROADMAP.md`); clean
project architecture.

**Acceptance test**: Project opens and compiles cleanly in Unreal Editor;
repo is version-controlled with LFS configured for binary assets;
canonical docs exist and are current.

---

### Phase 1 — Networked Player Sandbox

**Objective**: Prove the multiplayer-first foundation with a controllable
first-person player.

**Deliverables**: listen-server multiplayer; first-person player pawn;
Enhanced Input; keyboard/mouse and gamepad support; spawn/join/disconnect
flow; debug map.

**Acceptance test**: Two or more clients can join a listen-server session,
each spawn as a first-person player, move independently, and see each
other; a client can disconnect/reconnect without breaking the session.

---

### Phase 2 — Replicated Physics Interaction

**Objective**: A general, server-authoritative framework for physically
handling objects.

**Deliverables**: interactable framework; grab; carry; drop; throw;
physics authority handling; basic impact/ragdoll reaction.

**Acceptance test**: Any connected player can grab, carry, drop, and throw
a physics object with correct server-authoritative ownership and no
desync between clients; a player impact produces a visible
stumble/ragdoll reaction.

---

### Phase 3 — Pit Garage Vertical Slice

**Objective**: The first full physical gameplay loop — a pit stop.

**Deliverables**: physical wheel object; wheel gun; jack; wheel
removal/refit; basic fuel; pit box; timed pit-stop loop.

**Acceptance test**: A full pit stop (jack, remove wheels, fit
replacement wheels, tighten, fuel, lower, release) can be performed
cooperatively by multiple players in a multiplayer session and is
timed/tracked.

---

### Phase 4 — Driveable Shitbox

**Objective**: A drivable vehicle using the shared vehicle architecture.

**Deliverables**: accessible replicated car; enter/exit; driving
cameras/input; basic tyre/fuel/temp state; tiny test circuit.

**Acceptance test**: A player can enter, drive, and exit a networked
vehicle around a small test circuit with working first-person and
third-person cameras and responsive KBM/gamepad driving input.

---

### Phase 5 — Complete Race Weekend

**Objective**: A full timed race-weekend structure with AI opposition.

**Deliverables**: laps/timing/grid; practice; qualifying; race; AI rival
cars; mandatory stop; results.

**Acceptance test**: A full weekend (practice → qualifying → race →
results) can be played start to finish in multiplayer with AI rivals on
track and a mandatory pit stop enforced.

---

### Phase 6 — Damage / Repair / Chaos

**Objective**: Consequential, readable vehicle damage tied to physical
repair work.

**Deliverables**: modular damage (punctures, bodywork, cooling, engine,
brakes, suspension); repairs; penalties.

**Acceptance test**: Damage sustained on track visibly affects driving,
is reported correctly, and can be physically repaired in the garage by
players; unrepaired or incorrectly repaired damage has a driving
consequence.

---

### Phase 7 — Bodge / Friendslop Polish

**Objective**: The signature bodge mechanic and comedy-forward physical
polish.

**Deliverables**: bodged repairs (fast/cheap/risky vs. proper repair);
player impacts/ragdolls; runaway equipment; stronger comedy interactions;
pings; Discord remains the voice solution.

**Acceptance test**: A bodged repair can be chosen as an alternative to a
proper repair, has a real chance of failing later in a session, and
players can ping/communicate physical world state without relying on any
new external tool.

---

### Phase 8 — Crew Chief Tablet

**Objective**: Deliver the core information-asymmetry mechanic via the
in-world tablet.

**Deliverables**: session role assignment; physical in-game tablet;
privileged live telemetry (timing/gaps, fuel, temps, tyres, damage,
weather, pit-window/mandatory-stop info, BRASS messages); driver remains
information-limited.

**Acceptance test**: One non-driving player is assigned Crew Chief per
session and, using only the in-world tablet, receives privileged
telemetry that the driver and other players do not see; the Crew Chief
can set the tablet down and perform normal garage tasks.

---

### Phase 9 — Championship / Economy

**Objective**: Persistent season progression and financial pressure.

**Deliverables**: host save; eight-round season; points; standings;
money; bills; fines; sponsors; purchases; upgrades.

**Acceptance test**: A championship can be started, saved after a round,
reloaded by the host, and resumed with correct standings and finances
carried forward across multiple rounds.

---

### Phase 10 — Content Production

**Objective**: Populate the season with its full content set.

**Deliverables**: eight fictional circuits; vehicles; teams; sponsors;
brands; random events; weather; track-specific characteristics.

**Acceptance test**: All eight calendar circuits are playable end to end
in a championship, each with distinguishable track-specific
characteristics as specified in `GAME_DESIGN.md`.

---

### Phase 11 — Frontend / Online Friends

**Objective**: A real front door for a group of friends to actually play
together.

**Deliverables**: main menu; host/join; lobby; ready state; online
sessions/invites; reconnect handling; settings; remapping; gamepad UI.

**Acceptance test**: A group of friends can find/host a session, join via
invite, ready up, reconnect after a drop, and rebind controls entirely
through the UI on both KBM and gamepad without developer intervention.

---

### Phase 12 — Art / Audio / UX

**Objective**: Coherent presentation across the whole game.

**Deliverables**: coherent stylised art; characters; vehicle variants;
animations; VFX; HUD; sound; comedy presentation; accessibility.

**Acceptance test**: A full weekend loop presents a consistent visual and
audio style with working HUD, and passes a basic accessibility check
(readable text, controller navigation, remappable controls).

---

### Phase 13 — Balance / Optimisation / QA

**Objective**: Make the full game reliable and well-tuned.

**Deliverables**: full championship tests; network edge cases; economy
tuning; performance; save compatibility; crash testing; packaging.

**Acceptance test**: A full eight-round championship completes without
crashes across a full multiplayer group, performance holds at target
settings, and save files remain compatible across a full run.

---

### Phase 14 — Friends Build

**Objective**: A distributable build friends can actually install and
play together over the internet.

**Deliverables**: Windows package; clean-PC testing; multiple
controllers; internet multiplayer testing; version/build system;
distributable mates build.

**Acceptance test**: A packaged build installs and runs on a clean
Windows PC with no dev environment, supports multiple controllers, and a
group can play a full session together over the internet.

---

### Phase 15 — Commercial Release Readiness (if later wanted)

**Objective**: Clear the non-gameplay bar for a commercial release,
if pursued.

**Deliverables**: IP/trademark review; store/legal/privacy; credits and
licences; achievements if wanted; commercial distribution work.

**Acceptance test**: All fictional naming/branding has passed IP/trademark
review, store/legal/privacy requirements are satisfied, and the build
meets platform submission requirements.

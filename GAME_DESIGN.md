# PIT CREW: British Shitbox Championship — Game Design

This document is the canonical game design reference. See `CLAUDE.md` for
operating rules, `ARCHITECTURE.md` for technical design, and `ROADMAP.md`
for phased delivery.

## Premise

Players run a catastrophically underfunded racing team through an
eight-round season of the fictional British Shitbox Championship (BSC), an
affectionate pisstake of real British/European motorsport. Names used
during development (Dongington Park, Brands Snatch, Poogeout, etc.) remain
subject to final IP/trademark review before any commercial release.

This is not a serious racing simulator. The primary experience is friends,
physical interaction, garage chaos, short racing sessions, damage, pit
stops, limited money, bad decisions, and emergent comedy — grounded in
enough real motorsport knowledge that the parody feels informed.

## Players & Roles

- 2–6 players, player-hosted/listen-server multiplayer, foundational from
  day one.
- No permanent player classes or mechanic roles. Any garage player can
  perform any physical garage/pit task.
- Exactly one human player drives during an on-track session.
- Exactly one non-driving player is assigned **Crew Chief** during each
  on-track session. This is a temporary session assignment and can rotate
  between practice, qualifying, race, or later rounds.
- Comedy should emerge primarily from player actions and system
  interactions, not scripted jokes.

## Crew Chief

- The Crew Chief uses a **physical in-game tablet** that exists and is
  interacted with entirely inside the Unreal world. No external app,
  second-screen tool, website, phone companion, or QR pairing exists or is
  planned.
- The Crew Chief receives privileged information: timing, lap/sector data,
  position, gaps, fuel estimates, temperatures, tyre state/warnings,
  damage/fault alerts, weather, pit-window information, mandatory-stop
  status, penalties, BRASS/race-control messages, and strategy-relevant
  alerts.
- The driver has deliberately limited in-car information. Other garage
  players do not automatically see the full Crew Chief feed.
- The Crew Chief must verbally relay information to the driver and crew.
  Discord is the canonical voice solution for the first finished friends
  build; in-game voice chat is not a launch requirement.
- The Crew Chief can put the tablet down at any time and perform normal
  physical garage tasks like any other player.

## Championship

- Main mode: an eight-round Championship with a persistent season save
  owned by the host.
- A full championship can be completed in one long evening, or saved and
  resumed between rounds.
- Calendar:
  1. **Brands Snatch** — compact season opener; traffic, awkward pit entry,
     wet-weather risk.
  2. **Snotterton** — flat and windy; long straights, fuel and cooling
     stress.
  3. **Thruston** — very fast; tyre/brake stress, punishes poor repairs.
  4. **Outin Park** — narrow and hilly; crests, grass, suspension damage,
     changeable weather.
  5. **Cockhill** — Scottish-style; cold, rain, cramped paddock.
  6. **Cadbad Park** — narrow old-school circuit; kerbs, bumps, frequent
     bodywork damage.
  7. **Sliverstone** — huge polished professional venue; the broke team
     looks completely out of place.
  8. **Dongington Park** — championship finale; mixed weather, bigger
     crowds, championship pressure.

These are fictional circuits inspired by motorsport archetypes, not direct
replicas of licensed real circuits.

## Weekend Loop

Target duration: roughly 20–30 minutes once tuned.

1. **Arrive / Set Up** — unload, organise tools/tyres/fuel/spares, prepare
   garage.
2. **Scrutineering** — BRASS checks the car; player may need to correct
   faults.
3. **Practice** — short session; driver learns track; Crew Chief begins
   monitoring; team identifies problems.
4. **Qualifying** — short session; determines grid position.
5. **Pre-Race** — tyres, fuel, light setup choices, repairs.
6. **Race** — roughly 5–8 minutes when tuned; human driver, AI rival cars;
   garage crew remains active; Crew Chief manages information.
7. **Pit Stop** — mandatory stop and/or damage repair; physical crew work.
8. **Results / Stewards** — classification, penalties, race-control
   decisions.
9. **Finance** — prize money, sponsor objectives, fuel, tyres, repairs,
   fines.
10. **Workshop / Progression** — repair, buy, upgrade, choose sponsor,
    proceed to next event.

## BRASS

The fictional governing body is the **British Racing Administration &
Sporting Stewardship (BRASS)**, handling scrutineering, race control,
penalties, investigations, technical directives, flags/notices, pit-stop
requirements, and stewarding. Tone parodies inconsistent/overbearing
motorsport bureaucracy.

## Physical Interaction

Core on-foot actions: walk, look, sprint, jump, interact, grab, carry,
drop, throw, primary tool use, secondary/context tool use, ping, use Crew
Chief tablet, enter/exit vehicle.

Gameplay should be physical rather than menu-driven wherever practical. If
a player needs a wheel, jack, wheel gun, fuel container, or tool, that
object should physically exist in the garage. Example physical objects:
wheels, tyres, wheel guns, jacks, toolboxes, fuel containers, body panels,
batteries, fire extinguishers, spare parts, garage equipment.

Players can lose, drop, and mishandle equipment where appropriate. Player
impacts can cause stumble/ragdoll comedy without gore. All physics
interactions must be multiplayer-authoritative and designed for
replication from the start.

## Pit Stop System

The first major vertical slice:

car enters pit box → jack car → remove wheels → carry old wheels away →
obtain replacement wheels → fit replacement wheels → tighten with wheel gun
→ fuel if required → repair if required → lower car → release.

Unsafe or incorrect actions create consequences, e.g.: loose wheel, missing
wheel nut, unsafe release, wrong tyre, unfinished repair, fuel mishap,
equipment left in pit lane.

## Vehicle Philosophy

Accessible, arcade-ish driving with enough depth that a better driver is
faster. Automatic gearbox by default; manual may be optional.

Core concepts: throttle, braking, steering, believable grip, weight
transfer, understeer, oversteer, kerbs, wet grip.

Explicitly **out of scope initially**: professional simulation, advanced
tyre thermodynamics, complex aerodynamic modelling, damper engineering
simulation, detailed setup engineering.

## Damage

Modular/readable damage, not BeamNG-level simulation. Systems: four
tyres/wheels, suspension/steering, brakes, front/rear bodywork,
cooling/radiator, engine health/temperature, fuel.

Damage should affect driving, produce Crew Chief warnings, and produce
physical garage repair work.

## Bodging

A signature mechanic with two broad repair approaches:

- **Proper Repair** — slower, more expensive, more reliable.
- **Bodge** — fast, cheap, questionable reliability (e.g. BodgeTape, cable
  ties, hammer, generic brackets).

A bodged component may survive or may fail later. This must create
meaningful risk/reward, not just cosmetic animation.

## Economy

The player team should nearly always feel slightly broke.

- **Income**: prize money, sponsor objectives, bonuses.
- **Costs**: entry fees, fuel, tyres, repairs, replacement parts, fines,
  upgrades.

Bad weekends should create stories rather than immediately destroy the
campaign. Emergency finance/debt (e.g. fictional lender **Manky Finance**)
can prevent hard campaign failure.

## Progression

Start with: battered trailer, cheap jack, cheap wheel gun, few spares,
basic garage, one shitbox, folding-table-level organisation.

Progress toward: proper transporter, better tools, faster/more reliable
equipment, tyre racks, more spares, better garage presentation, better
workshop facilities.

The operation becomes visually more professional. The players do not.

Avoid RPG-style character statistics such as "+5 mechanic skill."

## Car World

All player cars share underlying architecture with data-driven
differences (weight, power, durability, fuel usage, handling, repair cost,
appearance, audio). Do not hard-code separate systems per vehicle.

Parody brand/model concepts may include: Poogeout, Frod, Fauxhall, Renalt,
Honday, Toymota (e.g. "Frod Focarse").

## Fictional World Examples

World-building flavour, not all required for the prototype.

- **Brands**: Pireleaky, Mishalot, Dunflop, Goodish Year, Bridgerock,
  Brimbo, Boshed, BodgeTape, UggaDugga, Probably Straight Engineering,
  Definitely Genuine Parts Ltd.
- **Teams**: Piston Broke Racing, Team Understeer, Slightly Bent
  Motorsport, Apex Avoidance Racing, Two Men & A Van Motorsport, Probably
  Fine Racing, Scrapyard GP, Last Minute Motorsport, Budget Performance
  Solutions, Spannerworks Racing, Maximum Attack Minimum Talent.
- **Sponsors**: Dodgy Dave's Used Motors, Manky Finance, Warm Lager,
  ParcelEventually, Fast-ish Fit, BodgeTape, Greg's Plumbing & Motorsport,
  Big Kev's Skip Hire.

## Game Modes

- **Championship** — main persistent game (see above).
- **Quick Weekend** — one isolated event without persistent economy.
- **Garage Sandbox** — practice driving, interactions, and pit stops.

## Controls Philosophy

Full keyboard/mouse and Xbox-style gamepad support from the beginning,
using Unreal Enhanced Input. All final UI must be controller-navigable and
gameplay controls remappable. Perspective is first person on foot;
driving supports first-person/cockpit view with an optional third-person
chase camera.

## Explicit Out-of-Scope Items

- External companion apps, second-screen apps, websites, or QR pairing for
  the Crew Chief.
- In-game voice chat (Discord is canonical for the first friends build).
- Professional racing simulation depth (tyre thermodynamics, aero
  modelling, damper engineering, detailed setup engineering).
- BeamNG-level damage simulation.
- RPG-style character/mechanic statistics.
- Permanent player classes or mechanic roles.
- Direct replicas of licensed real-world circuits or brands (parody only,
  subject to IP review before commercial release).

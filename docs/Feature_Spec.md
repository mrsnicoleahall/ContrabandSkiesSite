# Contraband Skies

## Ludos-Ready Feature Spec

### 1. High Concept
`Contraband Skies` is a persistent-match noir extraction shooter set on a massive stylized island-city warzone. Players use high-speed flight rigs for traversal, scouting, repositioning, and evasion, but all meaningful combat, looting, and extraction happen on the ground. The defining loop is that only extracted inventory is kept, while death creates a recoverable corpse drop in the same live world.

The major long-term progression system is a slow, PvE-achievable weapon crafting path. Crafted weapons are `bio-bound` to the player who made them, cannot be permanently stolen by others, and can eventually be insured through another long-grind system so they return after loss. This creates a second progression lane for players who prefer patient PvE play over high-risk PvP.

### 2. Product Pillars

#### 2.1 Persistent Extraction Warzone
- One always-on shared map instead of short isolated matches
- Players drop in and out of the same live ecosystem
- Loot, NPC activity, corpse caches, extraction pressure, and contracts continue whether or not a specific player is online

#### 2.2 Airborne Superhero Mobility
- Fast, satisfying omnidirectional flight
- High verticality and long sightlines
- Dive, climb, boost, disengage, and reposition as core expressions of skill
- Flight is traversal and evasion, not a firing platform

#### 2.3 High-Stakes Loss With Recovery
- Anything carried in-session is at risk until extraction
- Death creates a corpse beacon and recoverable drop
- Players can re-enter the same live world and attempt a comeback

#### 2.4 Grounded Gunfight Identity
- Guns are the primary weapon category
- Players cannot shoot while airborne
- Looting, extraction, and most interaction states require touching down
- Ground units can kill airborne players, making flight a vulnerable movement choice rather than safe dominance

#### 2.5 Contraband Identity
- Cosmetics can be looted from enemy players
- Cosmetics are only converted to permanent ownership if extracted
- Trophy hunting and revenge loops create memorable social drama

#### 2.6 Slow-Burn PvE Mastery
- A full crafting path exists that can be completed through PvE alone
- Crafted weapons are bio-bound and prestige-bearing
- Insurance is earned, not bought instantly
- PvE players can progress meaningfully without being forced into PvP, but the path is intentionally long and demanding

### 3. Target Experience
- Visual tone: hyper-realistic comic-book noir
- Art direction: black-and-white, hard contrast, deep shadows, wet streets, heavy silhouettes, with very selective pops of color such as crimson, gold, electric blue, or toxic green used sparingly for emphasis
- Reference energy: Sin City mood with Dick Tracy-style color punctuation
- Map structure: three distinct full-scale maps rather than one single environment
- Map feel: each map should preserve the same noir comic-book grammar while delivering a different traversal and combat rhythm
- Camera target: third-person, over-the-shoulder action game
- Systems feel: extraction-shooter stakes with traversal flight but grounded shootouts
- Emotional cadence:
  - freedom while traveling
  - tension while looting
  - fear while carrying high-value contraband
  - triumph when extracting
  - desperation when racing back to a corpse
  - long-term pride when finally forging a bio-bound weapon

### 4. Core Gameplay Loop

#### 4.1 Immediate Loop
1. Deploy into the persistent world
2. Choose a goal: loot, contract, hunt, gather PvE materials, reclaim corpse, or extract
3. Traverse rapidly across the island using land routes and tactical flight repositioning
4. Touch down and fight NPCs and/or players with guns
5. Gather loot, crafting materials, contraband, and objective items on foot
6. Decide whether to press deeper or extract now
7. If killed, respawn into the same live world and try to recover the drop
8. If extracted, move carried rewards into permanent stash

#### 4.2 Long-Term Loop
1. Build stash wealth through extraction
2. Farm rare PvE components for weapon research and fabrication
3. Unlock advanced crafting stations and recipes
4. Forge bio-bound weapons
5. Grind insurance attunement to protect those weapons
6. Expand build diversity and prestige loadouts over weeks, not days

### 5. Match Structure
- Persistent shared-server world
- Players can join at any time
- Match can soft-reset on long cadence, such as every 24 to 72 hours, weekly world events, or season-based restructures
- During a world cycle:
  - resource nodes replenish
  - AI factions roam and defend zones
  - contracts rotate
  - corpse drops expire
  - event zones pulse on and off

#### 5.1 Map Set
The game should ship with three distinct maps:

- `Urban`
  - dense city core
  - rooftops, alleys, elevated rail, neon avenues, towers, plazas, warehouses
  - strongest vertical escape play and rooftop ambush potential

- `Rural`
  - open farmland, ridgelines, creek beds, timber edges, mills, quarries
  - longer sightlines, more exposure during flight, stronger marksman pressure

- `Suburbs`
  - housing blocks, school zones, strip malls, drainage channels, cul-de-sacs, backyard routes
  - medium sightlines, lots of flanking lanes, strong house-to-house grounded gunfights

#### 5.2 Map Design Goals
- All three maps must support the same extraction, corpse-recovery, crafting, and contract systems
- Each map should create a different answer to the question: when is it safe to fly and where is it smart to land?
- Urban should favor rooftop repositioning
- Rural should punish exposed movement
- Suburbs should emphasize ambushes, lane control, and close- to mid-range pressure

### 6. Player Movement

#### 6.1 Flight Kit
- `Cruise`: baseline directional movement while airborne
- `Boost`: high-speed burst consuming flight energy
- `Climb`: vertical gain for scouting, escape, and rooftop access
- `Dive`: rapid descent for fast repositioning and forced landings
- `Brake`: responsive deceleration to avoid clumsy overshoot
- `Touchdown`: explicit return to ground for combat, looting, and extraction

#### 6.2 Movement Design Goals
- Easy to control immediately
- Deep mastery in approach angles, disengagement, rooftop routing, and evasive movement
- Strong sense of momentum without losing readability
- Supports both PvP aggression and PvE farming routes
- Flight should create exposure, not safety, because grounded enemies can shoot at airborne targets

#### 6.3 Energy System
- Boosting drains energy
- Normal travel regenerates energy
- Diving can improve movement efficiency
- Exhaustion should weaken pursuit without making the player helpless

### 7. Combat

#### 7.1 Core Combat Model
- Strictly third-person, over-the-shoulder combat presentation
- Shields plus health
- Guns are the primary weapon family and central skill expression
- Combat occurs on the ground only
- Mid-range engagements dominate
- Positioning, cover, landing choice, and sightline control are as important as raw DPS

#### 7.1.1 Camera Rules
- The shipping game should be built as a full third-person action shooter
- Default camera should sit behind and slightly above the player character
- Flight should transition the camera into a traversal-friendly chase view, but not into a top-down perspective
- Combat readability should come from silhouette, muzzle flash, contrast lighting, and strong hit reactions, not abstract overhead clarity

#### 7.2 Weapon Families
- Pulse carbines
- Rail rifles / marksman weapons
- Close-range shotguns
- SMGs and pressure weapons
- Later expansion:
  - status weapons
  - shield breakers
  - utility launchers

#### 7.3 Combat Goals
- Fast enough to feel dangerous
- Not so fast that corpse recovery becomes pointless
- Air mobility should create repositioning opportunities, not airborne fire superiority
- Players in the air should be vulnerable to anti-air pressure from grounded fighters

### 8. Inventory, Stash, and Loss Rules

#### 8.1 Permanent Stash
- Stores extracted loot and currencies
- Stores crafting materials
- Stores recipe unlocks and long-term progression items
- Stores bio-bound weapons

#### 8.2 Active Run Inventory
- Holds current-session loot
- Lost on death unless recovered from corpse
- Only banked on successful extraction
- Loot must be physically collected while grounded

#### 8.3 Death Rules
- On death, all non-protected active-run items drop at corpse
- Corpse creates a beacon in the live world
- Owner can redeploy and recover it
- Others can loot it first

#### 8.4 Protected Item Rule
- Bio-bound crafted weapons are tied to the owner
- If not insured, death causes them to become unusable in the field and return to a recovery queue or fabrication cooldown state rather than becoming permanently lootable by others
- Other players may salvage temporary field residue or attached modules, but cannot steal permanent ownership of the bound chassis

### 9. Extraction Rules
- Extraction happens at zones, pads, or called uplinks
- Extraction takes time and can be contested
- Successful extraction moves carried items into permanent stash
- Failed extraction means carried items remain at risk
- Extraction must begin while grounded

Extraction should be:
- visible
- noisy
- high pressure
- worth defending or ambushing

### 10. PvP Systems

#### 10.1 Player Looting
- Fallen players drop their run inventory
- Players may recover their own corpse loot if they return in time

#### 10.2 Cosmetic Contraband
- Cosmetics can be looted as contraband
- Contraband is not permanent until extraction
- Cosmetic theft should be framed as stolen pattern data, illicit suit wraps, or contraband identity keys rather than deleting ownership from the original player

#### 10.3 PvP Risk Profile
- PvP is high-reward and accelerates loot gains
- PvE route remains viable but much slower for long-term mastery
- This preserves both predator and survivor playstyles

### 11. PvE Systems

PvE is not filler. It is a full progression lane and the backbone of the crafting system.

#### 11.1 PvE Factions
- `Scav Raiders`: baseline armed roamers, camp defenders, convoy escorts
- `Blacksite Security`: tougher tech guardians defending research zones
- `Mutated Wildforms` or `Bioforms`: optional organic threats for rare biochemical components
- `Stormbound Drones`: airborne machine enemies that patrol dangerous skies
- `Rogue Harvesters`: extraction-focused AI that compete over resource sites

#### 11.2 PvE Activity Types
- Patrols
- Convoys
- Nest clears
- Relay defense events
- Vault breaches
- Corrupted storm anomalies
- World bosses or elite faction captains

#### 11.3 PvE Role In Crafting
PvE activities are the primary source of advanced crafting inputs. This is what allows a player to eventually earn powerful crafted gear without needing PvP kills.

PvE should supply:
- common crafting parts
- refined metals
- bio-reactive tissues
- weapon memory cores
- relay crystals
- attunement enzymes
- insurance sigils / policy tokens
- recipe fragments

#### 11.4 PvE Progression Philosophy
- Everything required for crafting can be earned through PvE alone
- The pacing must be intentionally slow
- PvP is a faster riskier accelerator, not a hard gate

### 12. Crafting System

This is a core differentiator and should be detailed clearly for implementation.

#### 12.1 Crafting Fantasy
Players are not just finding guns. They are slowly assembling personally attuned weapons through rare materials, research, and fabrication. Crafted weapons should feel earned, named, and meaningful.

#### 12.2 Crafting Goals
- Give PvE-focused players a deep endgame
- Create long-term attachment to gear
- Prevent economy collapse from endless looted gun churn
- Support prestige and identity
- Reward persistence over raw aggression

#### 12.3 Crafting Structure
Crafting happens in layers:

1. `Material Acquisition`
- Gather common and rare components from PvE, contracts, salvage, and extraction

2. `Research Unlocks`
- Find blueprint fragments, faction schematics, blacksite data, or bio-signature patterns

3. `Fabrication`
- Use unlocked recipes plus large material costs to build a weapon frame

4. `Bio-Binding`
- Weapon is imprinted to the crafter’s account
- This final step makes it personally owned and non-transferable

5. `Insurance Attunement`
- Separate progression path that can protect the bound weapon from permanent field loss

#### 12.4 Crafting Materials

##### Common Inputs
- Alloy scraps
- Flux cells
- Polymer wraps
- Circuit shards
- Standard weapon cores

##### Rare Inputs
- Blacksite relay crystals
- Adaptive servo bundles
- Living tissue catalysts
- Phase-stabilized rails
- Neural imprint gel
- Flight capacitor clusters

##### Exotic Inputs
- Elite boss trophies
- Hazard-zone mutagen glands
- Prototype memory lattices
- Attunement seeds
- Sovereign-grade chassis fragments

#### 12.5 Crafting Stations
- `Field Bench`: basic salvage and component refinement
- `Workshop`: full intermediate weapon part fabrication
- `Blacksite Forge`: advanced chassis and high-tier bio-binding
- `Insurance Shrine` or `Attunement Lab`: long-cycle insurance imprinting

These stations can exist either:
- as safe-hub meta progression screens
- or as world locations requiring risk to access

Hybrid recommendation:
- Research and final crafting in safe meta hubs
- special materials only obtainable from dangerous world locations

#### 12.6 Recipe Unlocks
Recipes should not all be store-bought. Unlock sources should include:
- elite PvE drops
- multi-step contracts
- faction reputation
- blacksite data extractions
- world boss completions
- salvage analysis milestones

#### 12.7 Weapon Tiers
- `Fieldmade`: early crafted, low prestige, useful
- `Specialized`: mid-tier role-defining weapons
- `Signature`: long-grind build-around weapons
- `Legacy`: very rare prestige projects with major time investment

#### 12.8 Bio-Bound Weapons
Crafted weapons are bio-bound to the owner.

Bio-bound means:
- permanently linked to player account
- cannot be traded freely
- cannot be permanently stolen by another player
- may require recovery, cooldown, or repair after death
- can be improved over time through mastery and re-attunement

This prevents the crafted-gear economy from feeling hopelessly punitive while preserving extraction stakes.

#### 12.9 Why Bio-Bound Matters
- Supports a slow-grind PvE fantasy
- Preserves emotional attachment
- Avoids catastrophic loss after months of work
- Lets the game have dangerous death rules without making long-term crafting feel irrational

### 13. Crafted Weapon Loss, Recovery, and Insurance

This system should be one of the most carefully tuned pieces of the game.

#### 13.1 Uninsured Crafted Weapon Loss
If a player dies carrying a bio-bound weapon that is not insured:
- the weapon drops as a damaged bound shell
- enemy players may loot temporary salvage or attached mods
- the owner loses access to the weapon for a recovery period
- the weapon returns to the owner only after one of the following:
  - the owner recovers the shell from the corpse and extracts
  - the owner pays a heavy repair cost
  - the weapon completes a long restoration timer

This keeps death meaningful while avoiding outright deletion of months of grind.

#### 13.2 Insured Crafted Weapon Loss
If a bio-bound weapon is insured:
- death still hurts
- the owner may lose field loot, ammo, temporary mods, and run gains
- but the insured core weapon returns after a reduced restoration timer or insurance claim process

#### 13.3 Insurance Philosophy
Insurance should not be a cash shop convenience or cheap reset.
It must itself be a long-term progression goal.

Insurance is designed to:
- reward commitment
- reduce fear for endgame loadouts
- create another meaningful grind for PvE players
- preserve high stakes because full protection is difficult to earn

#### 13.4 Insurance Acquisition
Insurance is earned through:
- faction reputation
- rare insurance tokens from elite PvE
- attunement research
- repeated successful extractions with crafted gear
- completion of protection contracts

#### 13.5 Insurance Levels

##### Level 1: Partial Attunement
- weapon can be recovered faster after loss
- restoration cost reduced

##### Level 2: Core Preservation
- weapon returns automatically after death with a meaningful timer
- attached mods may still be lost

##### Level 3: Legacy Policy
- highest-tier protection
- only for top-end long-grind builds
- should take a very long time to unlock

#### 13.6 Insurance Costs
Insurance progression should consume:
- insurance sigils
- rare policy tokens
- repeated extraction milestones
- station time or cooldown time
- large amounts of PvE-derived materials

### 14. PvE Elements Required For Crafting and Insurance

To support the slow grind, the world needs reliable but demanding PvE systems.

#### 14.1 Resource Sites
- relay towers for crystal harvesting
- blacksite labs for advanced schematics
- infested nests for bio-components
- crashed military carriers for chassis parts
- storm anomaly zones for attunement reagents

#### 14.2 Elite PvE Content
- named captains
- biome-specific mini-bosses
- armored convoys
- defense events with escalating waves
- stormbound world bosses

#### 14.3 PvE-Only Advancement Path
A solo or co-op PvE player should be able to:
- farm materials
- unlock schematics
- forge a bio-bound weapon
- eventually insure it

But this should take:
- many successful runs
- multiple rare drops
- careful extraction discipline
- significant repetition across high-danger PvE content

### 15. Contracts and Quest Lines

#### 15.1 Contract Givers
- quartermasters
- brokers
- relay saints
- blacksite defectors
- scavenger scientists

#### 15.2 Contract Categories
- `Hunt`: kill enemy factions or dangerous flyers
- `Salvage`: extract materials or data
- `Recovery`: retrieve your own corpse cache or a lost shipment
- `Defense`: protect relay or drill sites
- `Research`: gather rare PvE ingredients for crafting
- `Attunement`: complete steps needed for bio-binding or insurance

#### 15.3 Quest Chain Purpose
Quest lines should:
- onboard players into the crafting loop
- gradually teach extraction risk
- introduce factions and bosses
- unlock higher crafting tiers and insurance systems

### 16. Economy

#### 16.1 Currency Types
- soft currency from loot and contracts
- crafting materials
- faction reputation
- schematic fragments
- insurance tokens
- contraband cosmetic imprints

#### 16.2 Economy Goals
- extracted value matters
- materials matter more than pure cash for endgame crafting
- PvE and PvP both feed progression
- top-tier gear takes genuine commitment

### 17. Progression Model

#### 17.1 Short-Term Progression
- better stash
- more money
- more loot
- more contracts

#### 17.2 Mid-Term Progression
- better recipes
- stronger crafted modules
- improved flight rig options
- faction rank

#### 17.3 Long-Term Progression
- signature bio-bound weapons
- insurance unlocks
- high-status cosmetics
- elite contract chains
- legacy-tier crafting projects

### 18. Balancing Goals

#### 18.1 PvP vs PvE
- PvP should accelerate gains
- PvE should guarantee eventual progress
- PvE-only route must be viable but intentionally slow

#### 18.2 Crafted Gear Power
- crafted gear should feel special
- it should not invalidate found gear completely
- early and mid crafted weapons should be specialization tools, not pure stat sticks
- top-end crafted weapons can be aspirational, but counterplay must remain

#### 18.3 Death Penalty
- punishing enough to preserve extraction stakes
- not so punishing that players refuse to use crafted gear at all

### 19. MVP Recommendation For Ludos

#### 19.1 Must-Have
- persistent shared map
- flight traversal
- PvP combat
- hostile PvE factions
- corpse recovery
- extraction banking
- stash vs active-run inventory
- cosmetic contraband
- contracts
- first-pass crafting
- first-pass bio-bound weapons
- first-pass insurance progression

#### 19.2 First Crafting MVP Scope
- 2 crafted weapon families
- 3 crafting tiers
- 6 to 8 material types
- 3 PvE activity types tied to rare components
- 1 insurance path with 2 protection tiers

#### 19.3 Post-MVP Expansion
- more boss archetypes
- faction quest chains
- co-op raid-like PvE events
- player social hubs
- guild/faction insurance projects

### 20. Key Ludos Build Notes

#### 20.1 What Makes This Game Different
- persistent extraction world
- superhero flight
- corpse recovery loop
- contraband cosmetics
- slow-burn PvE crafting path
- bio-bound weapons with grindable insurance

#### 20.2 What Must Feel Great Early
- traversal
- extraction tension
- corpse recovery drama
- first meaningful crafting milestone

#### 20.3 What Must Sustain Retention
- world events
- faction contracts
- material chase
- bio-bound weapon projects
- insurance progression

## Hand-Off Summary
If Ludos needs the shortest possible summary:

Build a persistent airborne extraction shooter where players can fully progress through PvE if they are patient. The long-term goal is crafting rare bio-bound weapons from extracted materials. Those weapons cannot be permanently stolen, but using them in the field is still risky unless the player completes a second long-grind insurance system. PvP should be faster and more volatile, but PvE alone must eventually get a player to top-end crafted gear over a much longer timeline.

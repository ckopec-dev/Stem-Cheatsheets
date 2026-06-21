# State Management in Large Multiplayer Games

## Introduction

One of the hardest problems in large multiplayer games is keeping thousands—or even millions—of players synchronized while maintaining performance, fairness, and low latency.

State management is the system responsible for:

* Tracking the game world
* Processing player actions
* Synchronizing changes
* Recovering from failures
* Preventing cheating
* Scaling across multiple servers

Examples include:

* World of Warcraft
* EVE Online
* Fortnite
* League of Legends
* Minecraft

Despite their differences, they all solve the same core problem:

**How do you maintain a consistent game world for many players simultaneously?**

---

# What Is Game State?

Game state is all information required to describe the world at a given moment.

Example:

```json
{
  "playerId": 42,
  "position": {
    "x": 125,
    "y": 87
  },
  "health": 95,
  "inventory": [
    "Sword",
    "Potion"
  ]
}
```

For a large MMO, state may include:

* Millions of player records
* NPCs
* Quests
* Guilds
* Chat channels
* Economy data
* Combat information
* World objects

---

# Authoritative Server Model

Modern multiplayer games almost always use an authoritative server.

## Bad Model: Client Authority

```text
Player Client
     |
     | "I hit the dragon"
     |
     V
Server accepts it
```

A hacked client could simply send:

```text
Damage = 999999
```

Cheating becomes trivial.

---

## Good Model: Server Authority

```text
Client
   |
   | "I attacked"
   V

Server
   |
   | Calculates result
   V

All clients updated
```

The server decides:

* Position
* Damage
* Health
* Loot
* Physics
* Economy

Clients only request actions.

---

# State Ownership

Large games divide ownership.

```text
World State
├── Players
├── NPCs
├── Buildings
├── Inventory
└── Economy
```

Each subsystem owns its data.

Example:

```text
Combat Service
    owns:
        damage
        abilities

Inventory Service
    owns:
        items

Guild Service
    owns:
        guilds
```

This prevents multiple systems from modifying the same data simultaneously.

---

# The Main Game Loop

A server usually runs a simulation loop.

```text
while (running)
{
    ProcessMessages();
    UpdateWorld();
    ResolveCombat();
    SendUpdates();
}
```

This repeats many times per second.

Typical tick rates:

| Game Type | Tick Rate |
| --------- | --------- |
| MMO       | 10–30 Hz  |
| MOBA      | 20–30 Hz  |
| FPS       | 30–128 Hz |

---

# Tick-Based Simulation

Instead of processing actions instantly, many games process them during ticks.

Example:

```text
Tick 100
Player moves

Tick 101
Enemy attacks

Tick 102
Damage applied
```

Benefits:

* Deterministic behavior
* Easier synchronization
* Better scalability

---

# State Replication

Players don't need the entire world.

Instead, the server replicates relevant state.

Example:

```text
Server
   |
   +--> Player A
   +--> Player B
   +--> Player C
```

Player A may only receive nearby entities.

---

## Area of Interest (AOI)

Large games use proximity filtering.

```text
World Map

[Player A]

NPC1
NPC2
NPC3

Far Away:
NPC5000
NPC5001
```

Player A receives:

```text
NPC1
NPC2
NPC3
```

Not:

```text
NPC5000
NPC5001
```

This dramatically reduces bandwidth.

---

# Delta Updates

Sending complete state every frame is impossible.

Instead:

```text
Position:
100,100
```

Changes to:

```text
101,100
```

Only the difference is sent.

```json
{
  "entity": 42,
  "x": 101
}
```

Benefits:

* Lower bandwidth
* Lower CPU
* Lower latency

---

# Entity Component Systems

Many modern games store state using an ECS architecture.

```text
Entity
  |
  +-- Position
  +-- Health
  +-- Inventory
```

Example:

```text
Entity 500

PositionComponent
HealthComponent
ManaComponent
```

Advantages:

* Cache friendly
* Scalable
* Easy parallelization

Popular engines using ECS concepts:

* Unity
* Unreal Engine

---

# Sharding

A single machine cannot host millions of players.

Games divide the world into shards.

```text
Shard 1
  Players 1-10000

Shard 2
  Players 10001-20000

Shard 3
  Players 20001-30000
```

Each shard runs independently.

Example:

```text
Server A
  Europe

Server B
  North America

Server C
  Asia
```

---

# Zone Servers

Many MMOs further divide shards.

```text
World
 ├── Forest
 ├── Desert
 ├── City
 └── Dungeon
```

Each zone runs on a separate server.

```text
Forest Server
Desert Server
City Server
```

When a player crosses a boundary:

```text
Forest -> City
```

Ownership transfers.

---

# Distributed State

Large games often use microservices.

```text
Login Service
Character Service
Inventory Service
Guild Service
Chat Service
Combat Service
```

No single server owns everything.

Instead:

```text
Inventory Service
   owns items

Guild Service
   owns guilds
```

Communication occurs through messages.

---

# Event-Driven Architecture

Instead of direct calls:

```text
Combat -> Inventory
```

Games often publish events.

```text
PlayerKilledDragon
```

Subscribers react.

```text
Loot Service
Quest Service
Achievement Service
```

Benefits:

* Loose coupling
* Easier scaling
* Easier maintenance

---

# Event Sourcing

Some large systems store every change.

Instead of:

```text
Gold = 500
```

Store:

```text
+100
-50
+200
```

History:

```text
PlayerEarnedGold
PlayerBoughtSword
PlayerSoldItem
```

Current state is reconstructed from events.

Benefits:

* Auditing
* Debugging
* Rollbacks

Costs:

* More storage
* More complexity

---

# Persistence Layer

State must survive crashes.

Typically:

```text
Memory
   |
   V
Database
```

Common pattern:

```text
Memory = active state

Database = durable state
```

Player logs out:

```text
Save Character
```

Server crash:

```text
Restore Character
```

---

# Handling Concurrency

Thousands of players may interact simultaneously.

Example:

```text
Two players loot the same chest
```

Without protection:

```text
Both receive item
```

Solutions:

### Locks

```text
Lock Chest
Grant Loot
Unlock Chest
```

### Optimistic Concurrency

```text
Version 10

Update if still Version 10
```

If changed:

```text
Retry
```

---

# Interest Management

Games must decide:

```text
Who needs this update?
```

Example:

```text
Player attacks monster
```

Send to:

* Nearby players

Do not send to:

* Entire world

This saves enormous bandwidth.

---

# Handling Latency

Internet latency cannot be eliminated.

Games hide it using:

## Client Prediction

Player presses:

```text
Move Forward
```

Client immediately moves.

Server later confirms.

---

## Reconciliation

If the prediction was wrong:

```text
Server Position != Client Position
```

Client corrects itself.

---

## Interpolation

Other players are rendered between updates.

```text
Server:
100
110
120
```

Client draws:

```text
101
102
103
...
```

Motion appears smooth.

---

# Scaling to Massive Worlds

Large MMOs often combine:

```text
Load Balancers
+
Zone Servers
+
Distributed Databases
+
Message Queues
+
Caching Layers
```

Example architecture:

```text
Clients
   |
Load Balancer
   |
Gateway Servers
   |
Game Servers
   |
Message Bus
   |
Services
   |
Databases
```

---

# Example MMO Architecture

```text
                Clients
                   |
             Gateway Layer
                   |
         --------------------
         |                  |
    Zone Server       Zone Server
         |                  |
         --------------------
                   |
              Event Bus
                   |
      --------------------------
      |      |       |         |
 Inventory Guild  Chat  Economy
 Service   Service Service Service
      |
   Database
```

---

# Common Mistakes

### 1. Sending Full State

Bad:

```text
Send all entities
every frame
```

Good:

```text
Send only changes
```

---

### 2. Client Authority

Bad:

```text
Client calculates damage
```

Good:

```text
Server calculates damage
```

---

### 3. Global Locks

Bad:

```text
Lock entire world
```

Good:

```text
Lock individual objects
```

---

### 4. Shared Database Everywhere

Bad:

```text
Every service updates every table
```

Good:

```text
Service owns its data
```

---

# Real-World Design Patterns

Many large multiplayer games use a combination of:

| Pattern                 | Purpose              |
| ----------------------- | -------------------- |
| Authoritative Server    | Anti-cheat           |
| Tick Simulation         | Consistency          |
| ECS                     | Performance          |
| AOI/Interest Management | Bandwidth reduction  |
| Delta Replication       | Network optimization |
| Event Bus               | Decoupling           |
| Sharding                | Horizontal scaling   |
| Zone Servers            | Load distribution    |
| Client Prediction       | Responsiveness       |
| Persistence Layer       | Recovery             |

---

# Summary

Large multiplayer games manage state by treating the server as the source of truth, running a tick-based simulation, replicating only relevant changes to nearby players, and distributing responsibility across specialized services. As player counts grow, techniques such as sharding, zone servers, interest management, event-driven messaging, and distributed persistence allow the world to scale while remaining responsive and consistent.

The key principle is simple:

**The server owns the truth; clients receive a carefully filtered, continuously updated view of that truth.**

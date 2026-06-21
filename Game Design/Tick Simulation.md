# Tick-Based Simulation Explained

A tick-based simulation is the foundation of most multiplayer games because it provides a consistent way for the server to process player actions and update the game world.

Think of the server as a giant clock.

Instead of processing events continuously, it updates the world at fixed intervals called **ticks**.

```text
Time ----->

Tick 1    Tick 2    Tick 3    Tick 4
  |          |         |         |
  V          V         V         V

Update    Update    Update    Update
World     World     World     World
```

If a game runs at 20 ticks per second:

```text
1000 ms / 20 = 50 ms
```

A tick occurs every 50 milliseconds.

---

# Why Use Ticks?

Without ticks:

```text
Player A attack arrives at 12.001

Player B attack arrives at 12.002

Player C attack arrives at 12.005
```

The order depends on network timing.

This can lead to inconsistent behavior.

With ticks:

```text
Tick 100

Process:
  Attack A
  Attack B
  Attack C

Resolve combat

Send updates
```

Everyone sees the same result.

---

# The Basic Tick Loop

A simplified game server:

```csharp
while (running)
{
    ReadNetworkMessages();

    UpdatePlayers();

    UpdateNPCs();

    ResolveCombat();

    SendStateUpdates();

    WaitUntilNextTick();
}
```

Every tick follows the same sequence.

---

# What Happens During a Tick?

Imagine a 50ms tick.

### Step 1: Collect Inputs

Players send actions.

```text
Player A -> Move North

Player B -> Cast Fireball

Player C -> Attack Goblin
```

The server stores them.

```csharp
inputQueue.Add(moveMessage);
inputQueue.Add(fireballMessage);
inputQueue.Add(attackMessage);
```

---

### Step 2: Process Inputs

The server validates requests.

```csharp
if (player.IsStunned)
{
    RejectCast();
}
```

```csharp
if (distance > weaponRange)
{
    RejectAttack();
}
```

Only legal actions proceed.

---

### Step 3: Simulate World

The server updates all entities.

Example:

```csharp
foreach (var npc in npcs)
{
    npc.Update();
}
```

Goblins move.

Patrol routes advance.

Cooldowns decrease.

Health regeneration occurs.

---

### Step 4: Resolve Interactions

Combat calculations happen.

```csharp
damage = CalculateDamage(attacker, target);

target.Health -= damage;
```

If health reaches zero:

```csharp
target.Die();
```

Loot drops.

Experience awarded.

Events generated.

---

### Step 5: Generate Changes

The server determines what changed.

Example:

```text
Player A moved
Goblin lost 15 HP
Chest opened
```

These become update packets.

---

### Step 6: Send Updates

Only nearby players receive updates.

```text
Player A receives:
  Goblin HP changed

Player Z across map:
  Receives nothing
```

---

# Tick Example

Suppose:

```text
Tick Rate = 10 Hz
```

Meaning:

```text
1 tick every 100 ms
```

At time:

```text
0 ms
```

Players send:

```text
A -> Attack

B -> Heal

C -> Move
```

Nothing happens immediately.

At:

```text
100 ms
```

Tick 1 executes.

```text
Process Attack
Process Heal
Process Move

Update State

Send Results
```

---

# Deterministic Simulation

One huge benefit is determinism.

Given:

```text
Same state
Same inputs
```

You get:

```text
Same results
```

Every time.

Example:

```text
Goblin HP = 100

Player attacks for 25

Result = 75
```

Always.

This makes debugging much easier.

---

# Fixed Time Step vs Variable Time Step

## Variable Time Step

A common beginner mistake:

```csharp
while(true)
{
    float deltaTime = GetElapsedTime();

    Update(deltaTime);
}
```

Sometimes:

```text
16 ms
```

Sometimes:

```text
25 ms
```

Sometimes:

```text
40 ms
```

Physics becomes inconsistent.

---

## Fixed Time Step

Instead:

```csharp
const float TickLength = 0.05f;
```

Always:

```text
50 ms
```

per simulation step.

Benefits:

* Predictable
* Easier networking
* Easier replay systems
* Easier debugging

---

# Movement Example

Player speed:

```text
10 units/sec
```

Tick rate:

```text
20 ticks/sec
```

Movement per tick:

```text
10 / 20 = 0.5 units
```

Every tick:

```csharp
player.X += 0.5f;
```

Tick sequence:

```text
Tick 1 -> X = 0.5

Tick 2 -> X = 1.0

Tick 3 -> X = 1.5
```

Very predictable.

---

# Input Buffering

Messages arrive between ticks.

Example:

```text
Tick 100
```

At:

```text
1005 ms
```

Player attacks.

At:

```text
1040 ms
```

Player moves.

Both are queued.

```text
Tick 101
```

Processes them together.

```csharp
ConcurrentQueue<PlayerInput>
```

is commonly used.

---

# Tick Numbers

Every update is assigned a tick number.

```text
Tick 5000

Player position:
  X=100
  Y=200
```

Later:

```text
Tick 5001
```

This helps synchronize clients.

Clients can say:

```text
I predicted movement at tick 5001
```

The server can verify it.

---

# Client Prediction

Waiting for every tick feels laggy.

Suppose:

```text
Ping = 100 ms
```

Player presses:

```text
Move Forward
```

If the client waits:

```text
100 ms to server
+
50 ms tick
+
100 ms back
```

Movement appears after:

```text
250 ms
```

Feels terrible.

Instead:

```text
Client moves immediately
```

and sends:

```text
MoveForward @ Tick 500
```

to server.

---

# Reconciliation

Sometimes prediction is wrong.

Client thinks:

```text
Position = 100
```

Server says:

```text
Position = 98
```

Client corrects:

```text
Position = 98
```

and replays remaining inputs.

This is called:

```text
Client Reconciliation
```

---

# Lag Compensation

FPS games often rewind state.

Example:

```text
Player shoots
```

Server receives shot:

```text
150 ms later
```

Server looks at:

```text
Historical Tick
```

and checks where the target was when the shot was fired.

This prevents:

```text
"I clearly hit him!"
```

situations.

---

# Tick Rates in Different Games

| Genre         | Typical Tick Rate |
| ------------- | ----------------- |
| MMO           | 10-30 Hz          |
| RTS           | 10-30 Hz          |
| MOBA          | 20-30 Hz          |
| Battle Royale | 20-60 Hz          |
| FPS           | 30-128 Hz         |

Higher tick rates:

```text
More accurate
```

but require:

```text
More CPU
More bandwidth
More memory
```

---

# A Real MMO Tick

A typical MMO tick might process:

```text
50,000 player inputs

20,000 NPC updates

10,000 combat events

5,000 inventory changes

2,000 quest updates
```

all within:

```text
50 ms
```

before the next tick starts.

If the server takes:

```text
60 ms
```

to process a 50 ms tick, it falls behind.

This is called **tick lag** or **server lag**.

---

# Example Architecture

```text
Tick 1000

1. Read network packets

2. Process player inputs

3. Update movement

4. Update NPC AI

5. Resolve combat

6. Apply buffs/debuffs

7. Generate events

8. Save critical changes

9. Build update packets

10. Send updates
```

Then:

```text
Tick 1001
```

begins immediately.

---

# Key Insight

A tick-based server is essentially a giant deterministic simulation that advances the world in small, fixed increments:

```text
Current State
      +
Player Inputs
      +
Game Rules
      =
Next State
```

Every tick transforms the world from one valid state into the next. This approach makes synchronization, anti-cheat, debugging, replay systems, and large-scale multiplayer networking much easier than trying to update the world continuously.

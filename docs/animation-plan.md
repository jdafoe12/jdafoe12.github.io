# Homepage Background Animation

## Concept

25 nodes cycle through three structural forms — the same nodes, rearranged — while a
signal propagates through each. The shapes are not decorative: they are the three
fundamental connected structures that underlie the work on this site.

On page load, the starting mode is chosen at random from the three.

---

## Three Modes

### 1. Axiom System  *(bottom → top)*

Nodes form a directed acyclic graph (DAG) in five tiers:

```
         [22]  [23]  [24]          ← apex theorems
       [17] [18] [19] [20] [21]
     [11] [12] [13] [14] [15] [16]
    [5]  [6]  [7]  [8]  [9]  [10]
       [0]  [1]  [2]  [3]  [4]    ← axioms
```

- Tier 0 (5 nodes): axioms — foundational, unprovable within the system
- Tiers 1–3 (6, 6, 5 nodes): derived theorems
- Tier 4 (3 nodes): apex — highest-order results

**Behaviour**: edges and proof events are generated fresh each cycle (`generateMathProof`).
Axiom nodes start invisible and stagger in 300 ms apart. All theorem nodes immediately
materialise as faint ghost rings — they exist but are unproven. A random subset (10–16)
of theorems is then proved sequentially:

- Theorems are always proved in strict tier order (lower tiers first).
- Each proof picks 1–4 premises from already-proved lower-tier nodes, weighted strongly
  toward the immediately adjacent tier (`weight = 1 / 5^dist`).
- An amber dot travels from each premise to the theorem over `SIGNAL_STEP`; on arrival
  the ghost ring collapses and the node flashes amber then settles to blue.
- Not all theorems need to be proved before the mode transitions.
- Proof events are spaced 1.2–3 s apart.

Axiom mode nodes start at `bright = 0` (invisible) and are only revealed as they are
proved. Edges are invisible (`base = 0`) until a signal travels them, then persist.

---

### 2. Neural Network  *(left → right)*

Nodes reflow into five vertical layers:

```
Input   Hidden-1   Hidden-2   Hidden-3   Output
 [0]      [3]        [10]       [17]      [22]
 [1]      [4]        [11]       [18]      [23]
 [2]      [5]        [12]       [19]      [24]
          [6]        [13]       [20]
          [7]        [14]       [21]
          [8]        [15]
          [9]        [16]
```

Layer sizes: 3 → 7 → 7 → 5 → 3. Edges are sparse (~3–5 connections per node to the
adjacent layer).

**Behaviour**: forward passes loop indefinitely. Each pass sweeps activation dots left to
right, layer by layer (~9 s per pass at `SIGNAL_STEP = 1800 ms`). When a pass
completes, if at least `NEURAL_TOTAL = 30 s` of wall time has elapsed since the mode
started, the mode holds then transitions; otherwise nodes and edges dim back to resting
state over 1.2 s and another pass fires immediately. The number of passes is not fixed —
it depends on how many whole passes fit into 30 s of real time. Because elapsed time is
checked only after a pass completes, the active period can exceed 30 s by up to one pass.

All nodes and edges are visible at rest (nodes at `NODE_DIM = 0.3` brightness, edges
at 9% opacity) so the full structure is always legible.

---

### 3. Distributed Network  *(no hierarchy)*

Nodes scatter to a seeded quasi-random arrangement (5 × 5 jittered grid, fixed seed).
Each node connects to its 4 nearest neighbours; the result is a connected, decentralised
graph with no privileged centre.

**Behaviour**: gossip propagation with randomised TTL. During the first 80% of the hold
period, spontaneous messages are initiated at random nodes. The first starts after a
random 0.2–1.0 s delay; subsequent starts are spaced 0.32–3.2 s apart:

- Each message is born with a TTL drawn uniformly from {1, 2, 3}; the first outgoing hop
  carries TTL − 1.
- The initiating node flashes and forwards to 1–N of its neighbours (uniform over count).
- When a hop with TTL > 0 arrives before the hold ends, its destination flashes and
  forwards to 1–N eligible neighbours (excluding the sender), passing TTL − 1.
- A hop with TTL = 0 still animates to its destination, but terminates without flashing
  that destination. A hop arriving after the hold ends also causes no flash or forwarding.
- Multiple independent message chains run concurrently; the same edge may carry
  different messages at different times.
- Touched nodes flash amber and fade back to resting brightness; they are not
  permanently "activated."
- Each hop takes a random 65–135% of `SIGNAL_STEP` (1.17–2.43 s) and uses
  `easeInOutSine`, giving the distributed signals varied, non-constant motion.

The distributed mode hold period is 2.5× longer than the other modes (`DIST_HOLD =
HOLD_DUR × 2.5 ≈ 20 s`) to allow enough gossip activity to be visible.

All nodes and edges are visible at rest (same as neural).

---

## Timing

| Constant      | Value  | Meaning                                          |
|---------------|--------|--------------------------------------------------|
| `SIGNAL_STEP` | 1800 ms | Exact axiom/neural hop time; base for distributed hops |
| `HOLD_DUR`    | 8000 ms | Hold after signal completes (axiom + neural)    |
| `DIST_HOLD`   | 20000 ms | Hold for distributed mode                      |
| `TRANS_DUR`   | 5000 ms | Duration of node position morph to next mode   |
| `NODE_DIM`    | 0.3    | Resting brightness for neural/distributed nodes  |

Approximate durations per mode cycle:

| Mode        | Active signal   | Hold   | Morph  | Total    |
|-------------|-----------------|--------|--------|----------|
| Axiom       | 20–40 s         | 8 s    | 5 s    | ~35–55 s |
| Neural      | ≥30 s (N passes)| 8 s    | 5 s    | ~43 s+   |
| Distributed | 20 s (gossip)   | —      | 5 s    | ~25 s    |

The animation is intentionally slow and ambient — it is a background, not a centrepiece.
During the morph transition, all amber (node pulse halos and traveling signal dots) is
suppressed via a global `inTransition` flag so only blue-grey nodes and edges are visible
as they move.

---

## Visual Language

### Colors (Nord palette)

| Role        | Nord swatch  | Hex       | RGB            |
|-------------|--------------|-----------|----------------|
| Dim node    | Nord1        | `#3B4252` | 59, 66, 82     |
| Active node | Nord10       | `#5E81AC` | 94, 129, 172   |
| Signal/amber| Nord13       | `#EBCB8B` | 235, 203, 139  |
| Background  | Nord0        | `#2E3440` | 46, 52, 64     |
| Text        | Nord6        | `#ECEFF4` | 236, 239, 244  |

Node color interpolates `DIM → ACTIVE` via `bright`, then `→ SIGNAL` via `pulse`.

### Nodes

| State            | Brightness | Radius                    |
|------------------|------------|---------------------------|
| Rest (neural/dist)| `NODE_DIM = 0.3` | 3.95 px            |
| Fully active     | 1.0        | 5 px                      |
| Signal peak      | 1.0 + pulse| up to 5.8 px + amber halo |
| Axiom rest       | 0 (invisible) | —                      |
| Ghost ring       | —          | 3.5 px unfilled stroke, 25% opacity |

### Edges

| State             | Appearance                                          |
|-------------------|-----------------------------------------------------|
| Rest (neural/dist)| Nord10 at 9% opacity, 0.5 px (`base = 1`)          |
| Active (proved)   | Nord10 at 29% opacity, 1 px (`act = 1`)             |
| Axiom rest        | invisible (`base = 0`)                              |
| Signal in transit | lit trail (Nord10 at 28%, 1 px) + amber dot at head |

### Signal dot (traveling)

While a signal is in transit along an edge:
- A **lit trail** (Nord10, 28% opacity, 1 px) runs from the source to the dot.
- An **amber dot** (Nord13, 30% opacity, 2.5 px radius) sits at the head.
- A **radial glow** (8 px radius, amber, 6% → 0%) halos the dot.
- Axiom and neural travel takes exactly `SIGNAL_STEP` (1800 ms) at constant speed.
  Distributed travel is randomised to 65–135% of that duration and eased with
  `easeInOutSine`.
- All amber is hidden during mode morphs (`inTransition = true`).

Pulsing nodes also receive a radial amber halo. Its radius is four times the current
node radius and its centre opacity is `pulse × 0.04` (1.6% at the implemented peak
`pulse = 0.4`).

---

## Architecture

### Three cycle functions

| Function        | Mode        | Notes                                              |
|-----------------|-------------|----------------------------------------------------|
| `runMathCycle`  | Axiom       | Calls `generateMathProof()` each cycle; dynamic edges |
| `runCycle`      | Neural      | One sweep per call; `complete` loops or transitions |
| `runDistCycle`  | Distributed | Pre-generates all gossip events into one timeline  |

`runMathCycle` and `runDistCycle` build a single `anime.timeline()` whose `complete`
callback advances `modeIdx` and calls `startMode`. `runCycle` builds one sweep timeline
per call; its `complete` checks elapsed wall time and either fires a reset timeline (which
calls `runCycle` again) or fires a hold+morph timeline (which calls `startMode`).

### Rendering — canvas draw loop

`frame()` runs every `requestAnimationFrame`. It is a **pure renderer** with no logic:
it reads `nodes[i].{x, y, bright, pulse, ghost}` and `edgeObjs[ei].{act, head, dir, base}`
(all written by anime.js) and draws accordingly. During `inTransition`, `pulse` is
treated as 0 and signal dots are skipped.

### State objects

- **`NodeObj`** — `{ x, y, bright, pulse, ghost }`. Positions are normalised [0, 1]
  and converted to pixels at draw time (resize is free). `bright` and `pulse` drive
  color and size; `ghost` drives the axiom ghost ring.
- **`EdgeObj`** — `{ act, head, dir, base }`. `base` (0 or 1) sets rest visibility.
  `dir` (1 = a→b, −1 = b→a) is set imperatively at transit start; `head` (0→1) and
  `act` (0→1) are tweened by anime.js.
- **`renderEdges`** — parallel index array to `edgeObjs`; swapped to the new mode's
  edges only after the transition completes.
- **`inTransition`** — global boolean; set `true` in the morph tween's `begin`
  callback, cleared at the top of `startMode`.
- **`NODE_DIM = 0.3`** — resting brightness for neural/distributed nodes; set in
  `complete` callbacks when the incoming mode is not axiom.

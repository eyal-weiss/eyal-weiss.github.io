---
title: "Nobody is in charge of the flock"
date: 2026-09-29
draft: false
summary: "A flock with no leader, three local rules, and one knob that flips a crowd from chaos to a single moving body — and what that says about planning for teams of robots."
---

Watch a murmuration of starlings at dusk and it is hard not to believe that someone is conducting. Thousands of birds bend, split, and fold back together as if the whole cloud were a single animal with a single mind. A school of fish does the same when a predator appears: the entire school turns at once.

But there is no conductor. No bird sees the whole flock, and no fish has a plan for the school. Each individual reacts only to the few neighbours around it, and the shape of the group emerges from those small, local decisions.

This idea is the heart of **[A mind of many](https://wonderlattice.com/#room=flock)**, one of the rooms in [Wonderlattice](https://wonderlattice.com), my educational side project of hands-on math experiments. This post is about what that room shows, why the behaviour it produces is so surprising, and why I, as someone who works on planning for robots, find it more than just pretty.

## Three rules and no leader

The room is a simplified version of **Boids**, a model introduced by Craig Reynolds in 1987 to animate flocks for computer graphics. Every moving mark on the screen follows three tendencies, and looks only at neighbours within a small sensing radius:

1. **Keep some space:** steer away from neighbours that are too close.
2. **Match direction:** steer toward the average heading of your neighbours.
3. **Stay together:** steer toward the average position of your neighbours.

That is the entire algorithm. Nothing in it mentions a flock, a leader, or a destination. There are three sliders in the room, one per rule, and your finger or cursor can add an outside attraction or repulsion. Turn on "Show one neighbourhood" and you can [look through one bird's eyes](https://wonderlattice.com/#room=flock&neighbors=true): a circle marks what it can sense, and lines point to the handful of neighbours it is actually listening to.

What you get, with the default settings, is a flock: marks that start scattered in random directions and, within seconds, organize into groups that sweep across the screen together.

## One knob, two worlds

The room also shows a number called **direction agreement**. It averages every individual's heading and measures the length of the result: close to 100% means everyone is pointing the same way, close to 0% means the headings cancel out. (Physicists call this kind of measure an *order parameter*.)

Here is the surprising part. I ran the room's own model headlessly, keeping the default "Keep some space" and "Stay together" settings, and varied only the "Match direction" slider, which goes from 0 to 2. After a minute of simulated time, averaged over five random starts:

| Match direction | Direction agreement |
|---|---|
| 0 | 6% |
| 0.05 | 9% |
| 0.1 | 39% |
| 0.2 | 97% |
| 0.4 and above | ~100% |

Almost all of the change happens between 0.05 and 0.2, in the first few percent of the slider. Below that, you get a restless crowd that never commits to anything. Above it, you get a single body moving as one. In between, it depends on the random start: some runs organize and some do not.

<div style="display: flex; flex-wrap: wrap; align-items: flex-start; gap: 12px; margin-bottom: var(--content-gap);">
<img src="/images/blog/flock-align-0.05.svg" alt="The flock at Match direction 0.05: birds scattered across the frame, pointing every which way (direction agreement 9%)." loading="lazy" style="flex: 1 1 280px; min-width: 0; height: auto; margin: 0;">
<img src="/images/blog/flock-align-0.2.svg" alt="The same flock at Match direction 0.2: the birds have gathered into one band, all flying the same way (direction agreement 99%)." loading="lazy" style="flex: 1 1 280px; min-width: 0; height: auto; margin: 0;">
</div>

*The same 130 birds from the same random start, after a minute of simulated time in the room's own model. Only "Match direction" differs. Each mark points where its bird is heading, and its faint tail traces roughly the last second of flight.*

You can see this yourself by comparing [the flock at 0.05](https://wonderlattice.com/#room=flock&align=0.05) with [the flock at 0.2](https://wonderlattice.com/#room=flock&align=0.2). The individuals are identical. Their rules are identical, except for how much weight each one gives to a single consideration. The collective behaviour is not a little different; it is a different world.

This is not a quirk of the room. In 1995, Tamás Vicsek and colleagues studied an even simpler model, in which particles move at constant speed and adopt the average direction of their neighbours plus some random noise. When the noise is high or the crowd is sparse, the particles wander in all directions. But once the noise drops below a critical level (or the density rises above one), the system goes through a **phase transition**: the particles spontaneously pick a common direction and move off together, even though nothing in their rules favours any particular direction.

## Real flocks, real rules

Models like these are simplifications, and the room says so openly: it captures some visual qualities of flocks and schools, not every decision a real bird makes. But the core idea has held up remarkably well against real data.

A striking example came in 2008, when Michele Ballerini and colleagues reconstructed the 3D positions of individual starlings in murmurations over Rome. They found that each bird interacts with roughly **six or seven nearest neighbours**, regardless of how far away those neighbours are, rather than with everyone within a fixed distance. That small detail turns out to matter: it helps explain why a flock stays cohesive even as it stretches, thins, and compresses. The local rule shapes the global behaviour.

## Why a planning researcher cares

My research is about planning, including planning for teams of robots, and this is where the flock stops being just beautiful for me.

When many agents share a space, whether they are warehouse robots, drones, or autonomous cars, someone has to decide how each of them moves. Sometimes that someone is a **centralized** planner that sees everyone and computes a joint plan. Sometimes it is **distributed**: each robot runs its own decision rule using only what it can sense or hear from nearby, much like a bird in a flock. The lesson of the room applies to both: small changes to the decision-making algorithm can have dramatic effects on the collective behaviour.

A few examples from the field:

- **Reciprocity in collision avoidance.** A classic way for a robot to avoid others is to pick a velocity that would not collide with them if they kept moving as they are. It works well for one robot among moving obstacles. But when every robot uses the same rule, each one assumes the others will not react, they all dodge at once, and then they all dodge back, producing oscillations. The fix, known as *Reciprocal Velocity Obstacles* (van den Berg, Lin, and Manocha, 2008), was conceptually tiny: each robot takes on half of the responsibility for avoiding a collision. That one change largely removes the oscillation and gives smooth, cooperative motion.
- **Priorities in decoupled planning.** A common way to scale up multi-robot planning is to plan robots one at a time, each avoiding the paths of those already planned. The order in which robots are planned looks like an implementation detail, yet it can make the difference between a team that flows smoothly and one that gets stuck, even when a solution exists.
- **Objectives in centralized planning.** Even a planner with a perfect view of everyone must be told what to optimize. Minimizing the time until the last robot finishes and minimizing the total time of all robots sound similar, but they can lead to very different team behaviour. In an [earlier post](/blog/2026-03-23-warehouse-rearrangement/), I wrote about how simply changing *what* a warehouse planner plans for, the packages instead of the robots, changes what the whole system can achieve.

In all of these, as in the flock, the behaviour of the group lives *between* the individuals. You cannot fully predict it by looking at one robot's rule in isolation, and a tweak that seems harmless locally can reorganize everything globally. That is both a warning and an opportunity: if collective behaviour is that sensitive to the rules, then choosing the rules carefully is one of the most powerful levers we have.

## Try it yourself

The best way to understand this is to play with it. Open **[A mind of many](https://wonderlattice.com/#room=flock)** and try a few experiments:

- Slowly turn "Match direction" down toward zero and watch for the moment the flock loses its sense of direction.
- Set "Match direction" to zero and turn "Stay together" up. Can a crowd stay together without agreeing where to go?
- Drag your finger through a moving flock with "Repel" selected, and watch it split and heal.

The room is one of twenty-seven in [Wonderlattice](https://wonderlattice.com), a free collection of small experiments with big mathematical ideas, from shapes and chance to puzzles, engineering, and signals, for curious minds of any age and with no background needed. It is available in five languages (English, Spanish, French, Hebrew, and Portuguese), with no accounts, ads, or tracking. The code is [open source](https://github.com/eyal-weiss/wonderlattice), and contributions, including translations, are welcome.

---

**Further reading:**
- C. W. Reynolds, "Flocks, Herds, and Schools: A Distributed Behavioral Model," *Computer Graphics (Proc. SIGGRAPH '87)*, 21(4), 1987. ([Reynolds's Boids page](https://www.red3d.com/cwr/boids/))
- T. Vicsek, A. Czirók, E. Ben-Jacob, I. Cohen, and O. Shochet, "Novel Type of Phase Transition in a System of Self-Driven Particles," *Physical Review Letters*, 75(6), 1995.
- M. Ballerini et al., "Interaction ruling animal collective behavior depends on topological rather than metric distance: Evidence from a field study," *PNAS*, 2008.
- J. van den Berg, M. Lin, and D. Manocha, "Reciprocal Velocity Obstacles for Real-Time Multi-Agent Navigation," *IEEE International Conference on Robotics and Automation (ICRA)*, 2008.

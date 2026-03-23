---
title: "What if warehouse robots planned around the packages, not themselves?"
date: 2026-03-23
draft: false
summary: "A simple change in perspective — planning paths for items instead of robots — leads to provably optimal warehouse rearrangement and up to 2x faster completion times."
---

Picture a warehouse floor. Dozens of robots scurry between shelves, picking up packages and delivering them to new locations. The warehouse layout needs to change — maybe seasonal inventory is rotating, or a new batch of products just arrived and the shelves need reshuffling. Every robot must coordinate with every other robot to avoid collisions, and the goal is simple: finish the rearrangement as fast as possible.

This is the **Multi-Agent Warehouse Rearrangement (MAWR)** problem, and it turns out that the way you *think* about it determines how well you can solve it.

## The natural approach (and its limitation)

The intuitive way to tackle this is **agent-centric**: assign each robot a task ("Robot 3, take the red package from shelf A to shelf D"), plan a collision-free path for each robot, and send them on their way. This is essentially what existing methods do — they treat it as a variant of multi-agent path finding, where the robots are the main characters and the packages are along for the ride.

This works, and it's fast. But it leaves performance on the table. Why? Because once you assign a task to a specific robot, you've locked in a commitment. Robot 3 *must* carry the red package the entire way, even if Robot 7 happens to be passing right by the halfway point and could easily take over.

## Flipping the script

In our [recent paper](https://ojs.aaai.org/index.php/SOCS/article/view/35985/38140) (which received the **Best Paper Award at SoCS 2025**), my co-authors Yaakov Sherma, Oren Salzman, and I proposed a surprisingly simple shift in perspective: **plan for the packages, not the robots.**

Instead of asking *"where should each robot go?"*, we ask *"how should each package move?"*

First, we compute optimal, collision-free paths for the packages themselves — as if the packages were the agents. Then, we figure out which robots should execute each movement using a network-flow algorithm. If some package movement can't be carried out by any available robot, we feed that information back and adjust the package paths accordingly.

This is the core of our algorithm, **NAT-CBS** (Non-Atomic Task Conflict-Based Search).

![Agent-centric vs Obstacle-centric planning](/images/agent-vs-obstacle-centric.svg)

## Why "non-atomic" matters

The word *non-atomic* captures the key advantage. In traditional approaches, a task is **atomic**: one robot picks up a package and carries it all the way to its destination. In our approach, tasks are **non-atomic**: Robot 1 might carry a package halfway across the warehouse, set it down, and Robot 2 — who happens to be nearby — picks it up and finishes the job.

This isn't just a theoretical nicety. Consider a small example: a 3×3 grid with four colored packages and two robots. The packages need to be shuffled to new positions. An agent-centric approach finds a solution in 11 timesteps. Our obstacle-centric approach finds the optimal solution in just 8 timesteps — a 27% improvement — precisely because it allows robots to hand off packages mid-transit.

## Does it actually work?

We tested NAT-CBS against the state-of-the-art method (MAPF-DECOMP) across hundreds of randomly generated warehouse instances on 8×8 and 15×20 grid maps.

The results were striking. NAT-CBS consistently produces better plans — in many instances, the competing method's makespan was **1.5× to 3× worse** than optimal. The gap grows with problem complexity: the more packages that need moving, the more the agent-centric approach struggles with suboptimal task assignments, while the obstacle-centric view continues to find efficient coordinated solutions.

The trade-off is runtime. NAT-CBS is significantly slower because it's solving a harder problem — it *guarantees* optimality rather than settling for a quick heuristic answer. Think of it as the difference between finding *a* route through traffic versus finding *the fastest* route. The latter takes more computation but can save significant time in execution.

## The bigger picture

What I find most interesting about this work is the meta-lesson: sometimes the best way to solve a coordination problem is to stop focusing on the actors and start focusing on what needs to happen. The robots are interchangeable — any robot can move any package. By planning around the packages (the things that *actually* need to reach specific locations), we unlock a more natural and efficient way to coordinate the whole system.

This kind of perspective shift — from who does the work to what work needs doing — likely extends beyond warehouses to other multi-agent coordination domains. And with growing interest in bounded-suboptimal variants that trade a small amount of optimality for dramatically faster computation, practical deployment may not be far off.

*Y. Sherma, E. Weiss, O. Salzman. "From Agent Centric to Obstacle Centric Planning: A Makespan-Optimal Algorithm for the Multi-Agent Warehouse Rearrangement Problem." SoCS 2025. [Paper](https://ojs.aaai.org/index.php/SOCS/article/view/35985/38140) · [Code](https://github.com/CRL-Technion/wh-rearrangement)*

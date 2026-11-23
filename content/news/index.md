---
title: "News"
layout: "single"
url: "/news/"
summary: "News from Eyal Weiss, postdoc at the CMU Robotics Institute: papers, awards, projects and workshops."
ShowToc: false
---

**News: announcements and ongoing projects**

- Our paper *"From Agent Centric to Obstacle Centric Planning: A Makespan-Optimal Algorithm for the Multi-Agent Warehouse Rearrangement Problem"* (with Y. Sherma, H. Peer and O. Salzman) was accepted for publication in the **Artificial Intelligence Journal (AIJ)** 🎉 It extends our SoCS 2025 paper, which won the **Best Paper Award**. The final version will be out in the next few months; until then, check out the [conference paper](https://ojs.aaai.org/index.php/SOCS/article/view/35985/38140) and the [blog post](/blog/2026-03-23-warehouse-rearrangement/) on its main idea: plan for the packages, not the robots.

- New arXiv preprint 🎉 *"Small Is Not Dead: Distance-Aware Cluster Revival for e-means"*. e-means is a recent mini-batch k-means algorithm, now part of the FAISS library, that periodically removes small clusters and reuses their centers. Size alone cannot tell a redundant cluster from one that covers a rare, isolated group of points, so the paper adds a distance-based condition: a small cluster is removed only if its center is close to another center. On data with small isolated groups, this cuts the test error by roughly 10% to 80%; on the e-means paper's own benchmarks, the results are identical to FAISS. Check it out on [arXiv](https://arxiv.org/abs/2611.18238); the code is on [GitHub](https://github.com/eyal-weiss/emeans-distance-revival).

- Our paper *"Generalizing Multi-Objective Search via Objective-Aggregation Functions"* (with H. Peer, R. Alterovitz and O. Salzman) was accepted to **IEEE Robotics and Automation Letters (RA-L)** 🎉 Check it out on [IEEE Xplore](https://doi.org/10.1109/LRA.2026.3741789) or [arXiv](https://arxiv.org/abs/2509.22085)!

- Launched **[Wonderlattice](https://wonderlattice.com)** 🎉, an educational side project: a free collection of small, hands-on experiments with big mathematical ideas, for curious minds of any age, with no background needed. It has thirteen "rooms", each built around one surprise: draw flowers with two turning arms, roll three dice that beat each other in a circle, weave houndstooth on a tiny loom, send a picture through a storm of flipped bits, and more. Every room comes with an optional explanation (with sources) and a visiting mathematician. No scores, accounts, ads or tracking, just play. The code is open source on [GitHub](https://github.com/eyal-weiss/wonderlattice), and ideas, fixes and translations are welcome!

- Started a new position as a postdoctoral researcher at the [Robotics Institute](https://www.ri.cmu.edu/), Carnegie Mellon University, affiliated with the [Search-based Planning Laboratory (SBPL)](https://www.ri.cmu.edu/robotics-groups/search-based-planning-laboratory/) and the [ARCS Lab](https://arcs-group.github.io/) 🎉

- Working on a very exciting research project that analyzes LLMs using... I'll share once it's ready :)

- Submitted my first paper in the field of Graph Neural Networks (technique inspired by search ideas). Check out the [preprint](https://arxiv.org/abs/2603.24291) and the [blog post](/blog/2026-03-31-per-edge-routing-gnns/).

- Submitted a cool & semi-educational paper to IROS 2026. 

- Opened a blog 🥳 The idea is to reflect ongoing developments in the field of robotics, through my POV, in simple language. Once in a while I will also write about other (non-robotics-related) technical aspects of my work (AI, planning, search, control).

- Submitted several grant proposals. Further details will be released upon notification. Update: Toyota research grant approved :)

- Physical integration of our group's motion planner with our prototype snake-like robot was successfully achieved.

- Started work on extending our 2025 SoCS paper *"From Agent Centric to Obstacle Centric Planning: A Makespan-Optimal Algorithm for the Multi-Agent Warehouse Rearrangement Problem"* in a few natural sub-optimal directions.

- Extension of our 2025 IJCAI paper *"Bidirectional Search while Ensuring Meet-In-The-Middle via Effective and Efficient-to-Compute Termination Conditions"* is almost done 😊. It extends both theory to provide better explanations to our results and empiric evaluation with additional domains, metrics and another ablation study.

- Submitted a really cool paper to ICRA 2026! More details will be released soon 😀 Update: ICRA ->  WAFR.

- Working on a high-performance motion planner that unifies many proven techniques: laziness, sampling-based, incremental computation, anytime search scheme (and more).

- Our joint (cross-faculty collaboration between computer science, mechanical engineering and civil engineering) project on *"Autonomous Multi-Stable Robot (MSR) for Search and Exploration"* was renewed for another year 🥳. This project revolves around developing a physical MSR, which resembles a snake-like robot, and various capabilities for it -- actuation, sensing, state estimation, motion planning and more. It is challenging and brings a lot of fun!

**Workshop organization:**

- Co-organized RIPL Workshop at ICAPS 2026 (July, Montréal)
- Co-organized HSDIP Workshop at ICAPS 2025 (November, Melbourne)
- Co-organized RDDPS Workshop at ICAPS 2024 (June, Banff)
- Co-organized RDDPS Workshop at ICAPS 2023 (July, Prague)

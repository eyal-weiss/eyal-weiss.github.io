---
title: "When AI agents program robots, they tailor rather than invent"
date: 2026-10-08
draft: false
images: ["/images/blog/agents-tailor-share.png"]
summary: "Coding agents given a simulator and $20 now write robot programs that beat planners built by experts. I looked inside 112 of those programs: no new algorithms, just known methods tuned to the scene, the robot, and the agent's own habits."
---

Give an AI agent a robot task, a simulator, and twenty dollars. Can it write a program that solves the task better than one built by human experts? A few years ago the question would not have been worth asking.

A recent paper from Tom Silver's group at Princeton, "[Coding Agents for Generalized Task and Motion Planning Problems](https://arxiv.org/abs/2609.30233)," finds that it often can. This post explains what they did. It then describes what I found when I opened the programs the agents wrote and asked whether the agents had invented new algorithms or done something else.

## Deciding what to do and how to move

Ask a robot to fetch a cup from the back of a crowded cupboard, and it faces two kinds of decisions at once. The first kind is about **what** to do: which objects to move out of the way first, where to put them, whether to use a tool. The second is about **how** to move: the exact path of the arm, so that it doesn't knock anything over. The two are tangled. A plan that sounds fine ("move the bowl first") may turn out to need an arm motion that is impossible. Roboticists call this problem *task and motion planning*.

The classic way to solve it relies heavily on human engineering. For each kind of problem, experts write down a vocabulary: which actions exist, when each one is allowed, and how to choose exact grasps and positions. A general-purpose planner then searches through that vocabulary for every new problem. This works, but building the vocabulary takes a lot of expert time, and the search can be slow.

## The experiment

The authors tried something different. They took off-the-shelf coding agents, the same tools programmers now use to write software (Claude Code from Anthropic and Codex from OpenAI), and gave each one a simulated robot task. The 28 tasks include pushing a T-shaped block into place, digging an object out of clutter, scooping balls with a tool, sorting blocks into bins, and throwing cubes into a bin.

Each agent got:

- a short text description of the task;
- a simulator it could run but not look inside;
- Python with two standard maths libraries;
- a budget of $20.

There was no vocabulary, no planner, and no robotics toolkit. Within that budget, the agent could experiment as it liked: try actions, watch what happened, write tests, and revise its code. At the end it handed over a single program. That program was then frozen and tested on 100 new versions of the task that it had never seen, with no AI involved at that stage.

The results are impressive. On the 16 tasks where a hand-built expert planner was available, the planner solved 47% of problems. **The agents' programs solved between 56% and 95%**, depending on the agent. They were also fast: the two strongest agents' programs needed 0.5 to 2 seconds per problem on average, while the planners needed 29.

The agents' logs read like the notebook of a careful engineer. In one task, the agent could not observe where the robot's hand was. So it grasped a cube and used it as a marker: it moved the arm around, watched where the cube went, and fitted six numbers that describe the robot's geometry. This cut its prediction error from about 4 centimetres to under 2 millimetres. The agents also found moves the authors did not expect. One rotated a scoop and grabbed it from the other side, so that it could scoop far more balls at once. Another ignored a sweeping tool and swept the cubes with the robot's gripper instead. [The project page](https://agenticgentamp.github.io/) has videos.

One caveat: these are simplified simulations in which the robot knows exactly where everything is. Real robots also have to see, and to cope with noisy sensors. But the core difficulty, deciding what to do and how to move at the same time, is the real one.

## Inside the programs: known methods, finely tuned

The authors are careful about what they claim is new. Their claim is about surprising *behaviours* like the ones above, not about new planning algorithms. I wanted to answer a different question: when an agent writes a program that beats an expert planner, is the program itself a new algorithm?

The authors published the agents' final programs, so I went through them with the help of Claude (fittingly, an AI reading AI-written code). That makes 112 programs (28 tasks, four agent setups, one run each), about 47,000 lines of code. I rated each program on a four-level scale:

| Level | Meaning | Programs |
|---|---|---|
| Textbook | A known algorithm used as is | 7 |
| Tailored | Known algorithms plus task-specific tweaks, tuned numbers, or scripted steps | 99 |
| Novel combination | Known pieces combined in a way that is not obvious and is specific to the problem | 6 |
| New algorithm | A general idea that is not in the research literature and could transfer to other problems | **0** |

**None of the 112 programs contained a new algorithm.** Six were clever combinations, but every piece of them already exists in the literature. The best of them digs an object out of clutter. It finds the route that bumps into the fewest movable obstacles, then clears just those obstacles away, deciding where to dump each one. It is a neat solution, and it is also very close to two problems that robotics researchers have long studied, known as *navigation among movable obstacles* and *minimum constraint removal*. Similarly, two of the programs for pushing the T-shaped block used a model of sliding and sticking contact that comes from the pushing research of the early 1990s.

What the agents do well is something else: they *tailor*. A typical program combines known techniques with three kinds of constants:

- **Known techniques.** Dijkstra's shortest-path algorithm from 1959. RRT-Connect, a classic path planner from 2000 that grows random trees. Standard ways to compute arm angles from a desired hand position. Textbook physics models.
- **Constants about the scene.** In one task, the goal is invisible to the robot. The programs simply wrote down the goal locations they had found by probing, either as fixed coordinates or as a rule that happens to fit the layouts they saw. Another program contained a map of obstacles that it had measured by bumping around.
- **Constants about the robot.** Fitted measurements of where the arm is mounted, and calibration tables for throwing: how fast to swing the arm so that a cube lands a given distance away. It is a bit like a basketball player who has learned how hard to shoot from each spot on the court.
- **Constants about the agent.** Even when two agents built the same physics model, each tuned its own numbers, sometimes to four decimal places. Each agent also has its own style. Claude's programs are long (about 600 lines each, as a median) and search through many candidates, retrying when something fails. Codex's programs are short (about 150 to 200 lines) and rely on measured constants and direct formulas.

<img src="/images/blog/agents-tailor-figure.svg" alt="Three panels. Left, a shelf of textbook techniques: Dijkstra 1959, RRT-Connect 2000, a pushing model from 1992, inverse kinematics, and beam search. Middle, a coding agent that probes a black-box simulator on a 20-dollar budget. Right, the program it writes: calls to textbook techniques plus constants about the scene, the robot, and the agent. Footer: 112 programs, 0 new algorithms." loading="lazy">

*A typical agent-written program, sketched. The calls on top are schematic, but the three constants are real values from three different published programs: a goal region from a navigation task cluttered with chairs, the fitted mount of the arm from the throwing task, and a constant in a model for pushing the T-shaped block.*

**In short: known methods, tuned to this scene, this robot, and this agent's own habits. Not a new algorithm, but a very good fit.**

Separate runs, sometimes by agents from different companies, also kept arriving at the same solutions: the same pushing model, the same throwing tables, the same way of dividing jobs between two rovers. *That is what you would expect if they draw on the same shared stock of knowledge rather than inventing on the spot.*

This pattern is not unique to robotics. When experts examined research documents written by AI systems, they judged about a quarter of them to be paraphrased from, or heavily borrowed from, existing papers without credit (Gupta and Pruthi, 2025). Another evaluation found that an "AI scientist" system labelled well-established techniques as new (Beel and colleagues, 2025).

A caveat about my own analysis: I looked at one run per agent and task, and the ratings are judgement calls. Still, the picture was consistent across all 28 tasks.

None of this takes away from the result. *Choosing the right tools for the job, adjusting them, and measuring the numbers that make them work is exactly what a good engineer does.* The agents do it in hours, for twenty dollars, separately for each task.

## Invent new tools, and let agents use them freely

A large part of robotics research has been about the "best variant". You take a known algorithm, add a rule of thumb here and a shortcut there, tune it on a family of problems, and show that it beats the previous variant. **This study suggests that such contributions are losing their relevance.** An agent will now produce a variant tailored not just to your family of problems but to the exact problem in front of it, constants included, for twenty dollars.

I think two kinds of work remain valuable, and are becoming more so.

The first is **core algorithmic innovation**: genuinely new ideas, as Dijkstra's algorithm and random-tree planners were in their time. Such ideas take considerable time, effort, and thought. *An agent spending a $20 budget on a given problem has every reason to reach for what already works, and it is unlikely to stumble on a new idea of that kind by accident.* And every new idea that researchers add to the shared toolbox becomes one more tool the agents can pick up. AI can help with discovery too, but in a different mode: Google DeepMind's [AlphaEvolve](https://arxiv.org/abs/2506.13131) runs a long, dedicated search that keeps changing programs and keeping the best ones, with the explicit goal of finding something new. It has found, for example, a way to multiply two 4×4 complex-valued matrices with 48 multiplications, improving on a 1969 record of 49. That is a powerful way to leverage AI for core discoveries, and a topic for a future post.

The second is **general, capable systems that give agents as much freedom as possible**. The paper gives evidence for both halves of this.

On the capability side: with only basic maths libraries, none of the programs I examined included a real motion planner for the robot arm in any of the 3D tasks. They made do with workarounds, like lifting objects high above everything and retrying when stuck. In a second setting, the agents were also given the simulator's source code. This told them more about each task and handed them ready-made tools: solvers for arm angles, collision checkers, and motion planners. Success rose from 74% to 84% for one agent and from 86% to 95% for the other.

On the freedom side: the hand-built planners, which require an expert to describe every task in a fixed vocabulary, were beaten by agents that were given no vocabulary at all.

*The lesson is to build fast, reliable, general machinery, and to expose it so that agents are free to use it however they see fit.* That means planners, solvers, and simulators (the kind of [planning in microseconds](/blog/2025-12-31-motion-planning-robots/) I wrote about earlier), offered as tools rather than locked inside a rigid framework.

**AI agents are not about to replace the people who invent new tools. They are taking over the work of fine-tuning the old ones, and they do it best when we give them good tools and the freedom to use them.**

---

**Further reading:**
- M. Merler, B. Li, J. Roy, Y. Liang, Q. Wang, Y. Huang, and T. Silver, "Coding Agents for Generalized Task and Motion Planning Problems," arXiv:2609.30233, 2026. ([Project page](https://agenticgentamp.github.io/), with videos and the generated programs)
- M. Stilman and J. Kuffner, "Navigation Among Movable Obstacles: Real-Time Reasoning in Complex Environments," *International Journal of Humanoid Robotics*, 2(4), 2005.
- K. Hauser, "The Minimum Constraint Removal Problem with Three Robotics Applications," *The International Journal of Robotics Research*, 33(1), 2014.
- S. Goyal, A. Ruina, and J. Papadopoulos, "Planar Sliding with Dry Friction, Part 1: Limit Surface and Moment Function," *Wear*, 143(2), 1991.
- K. M. Lynch, H. Maekawa, and K. Tanie, "Manipulation and Active Sensing by Pushing Using Tactile Feedback," *IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)*, 1992.
- E. W. Dijkstra, "A Note on Two Problems in Connexion with Graphs," *Numerische Mathematik*, 1, 1959.
- J. J. Kuffner and S. M. LaValle, "RRT-Connect: An Efficient Approach to Single-Query Path Planning," *IEEE International Conference on Robotics and Automation (ICRA)*, 2000.
- T. Gupta and D. Pruthi, "All That Glitters is Not Novel: Plagiarism in AI Generated Research," *Annual Meeting of the Association for Computational Linguistics (ACL)*, 2025.
- J. Beel, M.-Y. Kan, and M. Baumgart, "Evaluating Sakana's AI Scientist: Bold Claims, Mixed Results, and a Promising Future?," *SIGIR Forum*, 2025.
- A. Novikov et al., "AlphaEvolve: A Coding Agent for Scientific and Algorithmic Discovery," arXiv:2506.13131, 2025.

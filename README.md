# Bee-Inspired Multi-Agent Q-Learning in NetLogo

A comparative experiment evaluating four tabular Q-learning architectures for agents navigating a fixed maze.

> **Scope:** This README reports observations from the E1, E2, E3, and E3b models in the recorded experiment. Results are descriptive and apply to the tested maze and parameter settings; they are not claims of universal superiority.

## Architectures

| Model | Learning organization | Main distinction |
|---|---|---|
| **E1 — Independent Q-learning** | Each bee maintains its own state-action Q-table. | No direct sharing of learned Q-values. |
| **E2 — Shared Q-values** | Bees use a common set of patch/environment-level Q-values. | Experience from one bee can influence other bees through the shared values. |
| **E3 — Scout–forager with broadcast** | Bees have individual Q-tables. One scout uses a higher exploration rate; foragers use a lower rate. Scout knowledge is broadcast after the relevant goal-discovery/episode event. | Combines role-specific exploration and knowledge transfer. |
| **E3b — Scout–forager without broadcast** | Uses the role-specific exploration arrangement, with broadcast disabled. | No-broadcast comparison for E3. |

The recorded exploration rates were **ε = 0.1 for E1/E2**, and **ε = 0.3 for the E3/E3b scout and ε = 0.1 for their foragers**. Therefore, E1 versus E3b changes more than communication alone. E3 versus E3b is the intended broadcast comparison, provided the two source files differ only in the broadcast mechanism.

## Experimental setup

| Setting | Recorded value |
|---|---|
| Environment | Fixed 11 × 11 maze |
| Start | (-5, 0) |
| Goal | (5, -3) |
| Actions | Up, down, left, right |
| Goal reward | +5 |
| Invalid/collision move | -1 |
| Valid non-goal move | 0 |
| Learning rate, α | 0.5 |
| Discount factor, γ | 0.9 |
| Maximum actions per episode | 100 |
| Colony sizes | 1, 5, 10, 20, 50 bees |
| Random seeds | 10 seeds per configuration |
| Training checkpoints | 1,000; 5,000; 10,000; 20,000; 30,000; 40,000; 50,000 episodes |

The experiment contains **4 architectures × 5 colony sizes × 10 seeds × 7 checkpoints = 1,400 observations**. The recorded master dataset contains 1,400 rows, with no duplicate architecture/colony-size/seed/checkpoint combinations and no missing key fields.

A checkpoint of *N* episodes represents *N* training episodes per bee in the recorded convention. Consequently, larger colonies have more aggregate bee-episodes at the same nominal checkpoint; cross-colony comparisons should be interpreted with this in mind.

## Evaluation metrics

- **Success rate:** fraction of evaluated bees that reach the goal under greedy evaluation.
- **Optimal-path rate:** fraction of evaluated bees that reach the goal in exactly 29 actions.
- **Mean successful steps:** mean path length among successful evaluations; undefined when there are no successes.
- **Path efficiency:** mean of 29 / steps among successful evaluations.
- **Strict full convergence:** every bee in a seed/configuration is both successful and optimal at that checkpoint.

The 29-action route is the reference shortest path recorded for this maze, but it still requires independent verification against the exact final wall layout. Success and optimality are separate metrics: reaching the goal does not necessarily mean the bee followed the shortest path.

## Observed mean success rates

The tables below report the **mean of the ten seed-level success rates** at each checkpoint. Values are percentages and are descriptive; confidence intervals and statistical significance tests have not been applied here.

### 5 bees

| Training episodes | E1 | E2 | E3 | E3b |
|---:|---:|---:|---:|---:|
| 1,000 | 0.0% | 0.0% | 0.0% | 0.0% |
| 5,000 | 0.0% | 80.0% | 0.0% | 0.0% |
| 10,000 | 2.0% | 100.0% | 6.0% | 6.0% |
| 20,000 | 42.0% | 100.0% | 70.0% | 44.0% |
| 30,000 | 74.0% | 100.0% | 94.0% | 92.0% |
| 40,000 | 96.0% | 100.0% | 100.0% | 94.0% |
| 50,000 | 98.0% | 100.0% | 100.0% | 98.0% |

### 10 bees

| Training episodes | E1 | E2 | E3 | E3b |
|---:|---:|---:|---:|---:|
| 1,000 | 0.0% | 0.0% | 0.0% | 0.0% |
| 5,000 | 0.0% | 100.0% | 0.0% | 0.0% |
| 10,000 | 3.0% | 100.0% | 11.0% | 8.0% |
| 20,000 | 46.0% | 100.0% | 68.0% | 45.0% |
| 30,000 | 79.0% | 100.0% | 87.0% | 75.0% |
| 40,000 | 95.0% | 100.0% | 95.0% | 90.0% |
| 50,000 | 96.0% | 100.0% | 97.0% | 98.0% |

### 20 bees

| Training episodes | E1 | E2 | E3 | E3b |
|---:|---:|---:|---:|---:|
| 1,000 | 0.0% | 60.5% | 0.0% | 0.0% |
| 5,000 | 0.5% | 100.0% | 0.5% | 0.5% |
| 10,000 | 8.0% | 100.0% | 8.5% | 9.0% |
| 20,000 | 53.5% | 100.0% | 55.5% | 44.5% |
| 30,000 | 82.0% | 100.0% | 89.0% | 79.5% |
| 40,000 | 92.5% | 100.0% | 98.0% | 91.5% |
| 50,000 | 97.5% | 100.0% | 99.0% | 98.0% |

### 50 bees

| Training episodes | E1 | E2 | E3 | E3b |
|---:|---:|---:|---:|---:|
| 1,000 | 0.0% | 100.0% | 0.0% | 0.0% |
| 5,000 | 0.2% | 100.0% | 0.4% | 0.0% |
| 10,000 | 7.8% | 100.0% | 13.0% | 7.2% |
| 20,000 | 44.4% | 100.0% | 64.2% | 46.8% |
| 30,000 | 78.0% | 100.0% | 92.0% | 79.4% |
| 40,000 | 92.8% | 100.0% | 97.4% | 93.4% |
| 50,000 | 97.4% | 100.0% | 99.2% | 96.4% |

## Strict full-convergence observations

A seed counts as fully converged only when **all bees are successful and all follow the 29-action reference route**. The entries below are the number of seeds meeting that criterion out of 10 at each checkpoint.

| Bees | Episodes | E1 | E2 | E3 | E3b |
|---:|---:|---:|---:|---:|---:|
| 5 | 1,000 | 0 | 0 | 0 | 0 |
| 5 | 5,000 | 0 | 8 | 0 | 0 |
| 5 | 10,000 | 0 | 10 | 0 | 0 |
| 5 | 20,000 | 0 | 10 | 3 | 0 |
| 5 | 30,000 | 2 | 10 | 8 | 5 |
| 5 | 40,000 | 8 | 10 | 10 | 8 |
| 5 | 50,000 | 9 | 10 | 10 | 9 |
| 10 | 1,000 | 0 | 0 | 0 | 0 |
| 10 | 5,000 | 0 | 10 | 0 | 0 |
| 10 | 10,000 | 0 | 10 | 0 | 0 |
| 10 | 20,000 | 0 | 10 | 2 | 0 |
| 10 | 30,000 | 1 | 10 | 5 | 0 |
| 10 | 40,000 | 6 | 10 | 6 | 2 |
| 10 | 50,000 | 6 | 10 | 7 | 8 |
| 20 | 1,000 | 0 | 5 | 0 | 0 |
| 20 | 5,000 | 0 | 10 | 0 | 0 |
| 20 | 10,000 | 0 | 10 | 0 | 0 |
| 20 | 20,000 | 0 | 10 | 1 | 0 |
| 20 | 30,000 | 0 | 10 | 3 | 0 |
| 20 | 40,000 | 2 | 10 | 7 | 0 |
| 20 | 50,000 | 5 | 10 | 8 | 5 |
| 50 | 1,000 | 0 | 9 | 0 | 0 |
| 50 | 5,000 | 0 | 10 | 0 | 0 |
| 50 | 10,000 | 0 | 10 | 0 | 0 |
| 50 | 20,000 | 0 | 10 | 1 | 0 |
| 50 | 30,000 | 0 | 10 | 5 | 0 |
| 50 | 40,000 | 0 | 10 | 7 | 0 |
| 50 | 50,000 | 4 | 10 | 8 | 0 |

These tables show strict full convergence for the listed colony sizes and checkpoints. They do not establish whether a configuration that has not converged by 50,000 episodes would never converge.

## Main observations from E1–E3b

### 1. E2 reached high success earliest in this maze

- E2 achieved **100% mean success by 5,000 episodes** for colony sizes 10, 20, and 50.
- With 5 bees, E2 reached **80% at 5,000 episodes** and **100% at 10,000 episodes**.
- With 20 bees, E2 had **60.5% mean success at 1,000 episodes**; with 50 bees, it had **100% at 1,000 episodes**.
- E2 was the only architecture to meet strict full convergence in all 10 seeds by 5,000 episodes for colony sizes 10 and 20. For 50 bees, 9/10 seeds met the criterion at 1,000 episodes and 10/10 by 5,000.

The pattern is consistent with experience pooling through shared Q-values in this environment. It does not demonstrate that shared Q-learning is universally superior.

### 2. E3 often improved intermediate learning compared with E1 and E3b

- With 5 bees at 20,000 episodes, mean success was **70% for E3**, compared with **42% for E1** and **44% for E3b**.
- With 50 bees at 20,000 episodes, mean success was **64.2% for E3**, compared with **44.4% for E1** and **46.8% for E3b**.
- At 30,000 episodes, E3 reached **94%** with 5 bees and **92%** with 50 bees.
- The size and timing of E3's intermediate advantage varied with colony size and checkpoint.

### 3. The E3–E3b difference was most visible during parts of training

At 5 bees, E3 was ahead of E3b by 26 percentage points at 20,000 episodes (70% versus 44%), but by 30,000 episodes the gap was 2 points (94% versus 92%). By 50,000 episodes, their mean success rates were 100% and 98%, respectively. This suggests that any broadcast-related benefit may be more visible during intermediate learning than at the final checkpoint in this maze. The results alone do not establish causation; that interpretation depends on confirming that broadcast is the only implementation difference and analysing paired seed-level results.

### 4. E1 and E3b generally caught up later, but the timing varied

For colony sizes 5–50, E1 and E3b had low mean success at early checkpoints and substantially higher mean success by 30,000–50,000 episodes. Their final success rates were often close to E3's, although exact outcomes varied by colony size. A final checkpoint alone therefore hides differences in the learning trajectory.

### 5. Success rate and optimal-path rate answer different questions

A bee can reach the goal without taking the reference shortest route. The strict convergence counts are therefore more demanding than the mean success-rate tables. These observations should not be interpreted as equivalent measures of policy quality.

### 6. Single-bee comparisons need care

E3 and E3b use the scout exploration rate (ε = 0.3) even with a colony size of one, while E1 and E2 use ε = 0.1. The single-bee comparison is therefore not a clean test of communication or role specialization: there are no other bees to receive a broadcast, and exploration rates differ.

## Limitations of these observations

- The experiment tested **one fixed maze**. Performance on other maze layouts, obstacle densities, or start/goal pairs has not been established.
- There are **10 random seeds per configuration**. The results shown here are descriptive means and counts, not statistical-significance claims.
- The seed/configuration is the appropriate replication unit for comparisons; bees in the same colony should not be treated as independent experimental replications.
- At a fixed nominal checkpoint, larger colonies accumulate more total bee-episodes. Colony-size comparisons are not equal-total-experience comparisons.
- E3 changes both exploration policy and knowledge transfer relative to E1. E3 versus E3b is the intended broadcast ablation, conditional on source-code equivalence.
- The recorded broadcast merge rule inserts missing entries and retains the larger value for overlapping entries. This is a specific value-merging choice, not a generic communication method.
- The recorded 29-action shortest path must be independently checked against the final maze geometry before treating it as a verified optimum.
- No claims are made about real bee behaviour, physical-robot deployment, guaranteed collision avoidance, or universal multi-agent reinforcement-learning performance.

## Data record

The frozen master dataset is recorded as `bee_q_learning_master_dataset.csv` and contains 1,400 rows and 13 columns. The dataset tracks architecture, colony size, seed, checkpoint, successful/optimal bee counts and rates, mean successful steps, mean path efficiency, evaluation collisions, and total bee-episodes. Mean successful steps and path efficiency are undefined when no bee succeeds; those values should remain missing rather than be interpreted as zero.

---

*This README documents the current observations from E1, E2, E3, and E3b. Numerical results should be regenerated from the frozen master dataset if the dataset or model code changes.*

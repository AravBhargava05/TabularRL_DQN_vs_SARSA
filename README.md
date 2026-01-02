# Q-Learning vs SARSA (Tabular RL) — Risk vs Reward

This project compares two classic **tabular reinforcement learning** methods—**Q-Learning** and **SARSA**—on a small gridworld-style task where the agent must choose between a **safe route** and a **faster but risky route**. The point isn’t “getting the best score,” it’s showing how *different update rules* produce *different behaviors* under risk.

This is the same tradeoff you see in real robots between **aggressive policies** that exploit and **conservative policies** that stay safe under uncertainty.

---

## What this shows (in one sentence)

**Off-policy Q-Learning tends to learn riskier, reward-maximizing behavior, while on-policy SARSA tends to learn safer behavior because it accounts for its own exploration.**

---

## Algorithms (what’s different)

### Q-Learning (off-policy)
Q-Learning updates toward the best possible next action (even if the agent won’t actually take it during exploration):

\[
Q(s,a) \leftarrow Q(s,a) + \alpha\left[r + \gamma \max_{a'} Q(s',a') - Q(s,a)\right]
\]

**Interpretation:** “Assume I’ll act optimally next step.”

This often produces **more aggressive** policies in risky environments.

---

### SARSA (on-policy)
SARSA updates toward the action the agent actually takes next (including exploration):

\[
Q(s,a) \leftarrow Q(s,a) + \alpha\left[r + \gamma Q(s',a') - Q(s,a)\right]
\]

**Interpretation:** “Update based on what I actually do.”

This often produces **safer** policies when the exploration policy could cause mistakes.

---

## Environment / Task

A gridworld-style environment where:

- There’s a **goal** with positive reward.
- There’s a **hazard region** (cliff-like) with a large negative penalty.
- The shortest route passes near hazards (fast but risky).
- A longer route avoids hazards (slower but safe).

You can think of this as:
- A drone choosing between a narrow corridor (fast) vs wide open path (safe)
- A mobile robot navigating near stairs/edges
- A manipulator reaching near obstacles

---

## Training setup

- **Tabular state-action values** (no neural networks)
- **ε-greedy exploration**
- Hyperparameters:
  - learning rate `α`
  - discount `γ`
  - exploration `ε`
- Training runs for N episodes and tracks:
  - episodic return
  - number of hazard falls / failures
  - learned policy visualization

---

## Results / Takeaways

**Observed behavior:**
- **Q-Learning** typically converges to a **shorter, riskier path** because it learns the optimal greedy target even while exploring.
- **SARSA** typically converges to a **safer path** because it “bakes in” the consequences of exploration into its updates.

**Why this matters for robotics:**
Real robots explore imperfectly (sensor noise, control error, drift). On-policy methods like SARSA can be more robust when:
- exploration is risky
- you care about safety constraints
- the robot’s behavior during training resembles deployment behavior

---

## How to run

> Replace with your actual command / file names.

```bash
# example
python tabular_rl.py --algo q_learning
python tabular_rl.py --algo sarsa

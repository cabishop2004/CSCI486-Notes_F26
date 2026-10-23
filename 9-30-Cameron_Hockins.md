# September 2026
**Cameron Hockins**

## Introduction

**What information did we have access to with MDPs?**
- All possible states + current state
- All possible actions
- Transition function
- Reward function

**Which of these are reasonable to assume we have?**
- Yes
- Yes
- No
- No

---

## Passive Reinforcement Learning

- What if we have states and actions, but not the transition or reward functions?
- **Problem:** what action to take?
  - Random action
  - → Fix a policy

### Direct Utility Estimation

- $U(s)$ is the average reward starting from state $s$.
- **Issue:** this ignores the relationship between states.

**Example:**

```text
(a) --> (b) <-- (c)
 2       10       8
```

We want to leverage the connection between states — it gives us more information with less moves/data.

### Adaptive Dynamic Programming

- **Idea:** estimate the transition function and reward function.
- Solve the MDP using last week's algorithms.
- When can we solve? How often do we update?
- **Issue:** each update is expensive (MDP), and we may be wasting computation on states that didn't change.

### Temporal Difference (TD) Learning

- **Idea:** learn from each experience as we go.
- Update $U(s)$ when we take an action from that state.

**How do we incorporate a single sample?**

Example: over 1 million trials you get 100 points per trial. You see states $A, B, C, D, X$ and get $-1{,}000{,}000$ points :( You've seen $A/B/C/D$ many times — how should you update your world model?

$$
\underbrace{U^\pi(s)}_{\text{new utility}} = \underbrace{U^\pi(s)}_{\text{old utility}} + \underbrace{\alpha}_{\text{learning rate (how much to correct)}} \Big(\text{reward}(s) + \gamma\, U^\pi(s') - U^\pi(s)\Big)
$$

where the term in parentheses is the *expected future reward* correction.

```text
TD-Learner pseudocode goes here
```

---

## Double Bandits

- World with two slot machines, two states: win or lose.
- Offline planning when we don't know the probability of each slot machine.
  - **Exploration:** act to get information about the world.
  - Use that information once you have it.
- **Regret:** even rational actions can lead to mistakes.
- **Sampling:** try things repeatedly.

---

## Active RL

- We know the states and actions.
- We don't know the transition function, reward function, or policy.

**What should we do?**
- Random action
- Do the best action — **exploitation**
- Do the actions we've done the least — **exploration**

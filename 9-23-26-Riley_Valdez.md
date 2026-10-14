# 9/23 Notes: MDPs, Bellman Equations & Value Iteration
 
---
 
## 1. Recall Grid World
 
|           | Col 1      | Col 2 | Col 3 | Col 4         |
|-----------|------------|-------|-------|---------------|
| **Row 3** | →          | →     | →     | **+1 (Gold)** |
| **Row 2** | ↑          | WALL  | ← or ↑| **-1 (Dead)** |
| **Row 1** | Start      | ←     | ↑     | ← or ↓        |
 
- Starts bottom-left at (1,1).
- **+1** and **-1** are terminal squares: landing on either ends the game.
- Walking into the wall or the edge of the board means you stay where you are.
- The arrows are the **optimal policy**: the best move from each square.
- Squares with two arrows are close calls where either move is reasonable. Near the -1, the agent has to weigh "short but risky" against "longer but safe."
  
### Transition Model
 
```
             ↑ 80%
             |
  10% ←  [   ↑   ]  → 10%
```
 
- **80%** you go where you meant to.
- **10%** you slip to the left, **10%** you slip to the right.
- You never slip backwards.
 
---
 
## 2. Vocabulary
 
| Symbol | Meaning |
|---|---|
| s | current state (square) |
| s' | the state you end up in |
| a | an action (up/down/left/right) |
| A(s) | all actions allowed in s |
| P(s' \| s, a) | chance that doing a in s lands you in s' |
| R(s) | reward for being in s (small negative = "cost of living") |
| U(s) | utility: how good s is **long-term** |
| γ (gamma) | discount factor: how much the future counts (0 to 1) |
| π*(s) | the optimal action in s |
  
---
 
## 3. Optimal Policy
 
$$
\pi^*(s) = \arg\max_{a \in A(s)} \sum_{s'} P(s' \mid s, a)\, U(s')
$$
 
- For each action, add up (chance of landing in s') × (how good s' is). That's the action's expected utility.
- **argmax** returns the action with the highest expected utility. 
---
 
## 4. Bellman Equations
 
$$
U(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a)\, U(s')
$$
 
**In words:** value of where I am = reward right now + discounted value of my best next move.

- Compare to a normal linear system:
```
  x = 2y + 3z
  y = x + 2z
```
- Not a normal linear system though so we use value iteration
 
  A linear system can be **solved in O(n³)**.
---
 
## 5. Value Iteration
 
1. Pick initial utilities for each state (usually all 0).
2. Update every state using U(s).
3. Update again, and keep going until the values stop changing (converge).
### The Bellman Update
 
$$
U_{i+1}(s) = R(s) + \gamma \max_{a \in A(s)} \sum_{s'} P(s' \mid s, a)\, U_i(s')
$$
 
- **U_{i+1}(s)** = the **new** utility of s
- **U_i(s')** = the **old** utility of the resulting state
  
---
 
## 6. Example 1: Deterministic Grid
 
  
Final utilities:
 
|           | Col 1   | Col 2 | Col 3   | Col 4 |
|-----------|---------|-------|---------|-------|
| **Row 3** | 0.7 →   | 0.8 → | 0.9 →   | **+1**|
| **Row 2** | 0.6 ↑   | WALL  | 0.8 ↑   | **-1**|
| **Row 1** | 0.5 →   | 0.6 → | 0.7 ↑   | 0.6 ← |
  
$$
U(s) = -0.1 + \max(\text{neighbor utilities})
$$
 
So every square is worth **0.1 less than its best neighbor**:
 
| Square | Best neighbor | Calculation | U(s) |
|---|---|---|---|
| (3,3) | +1 (right) | -0.1 + 1.0 | 0.9 |
| (2,3) | 0.9 (right) | -0.1 + 0.9 | 0.8 |
| (3,2) | 0.9 (up) | -0.1 + 0.9 | 0.8 |
| (1,3) | 0.8 (right) | -0.1 + 0.8 | 0.7 |
| (3,1) | 0.8 (up) | -0.1 + 0.8 | 0.7 |
| (1,2) | 0.7 (up) | -0.1 + 0.7 | 0.6 |
| (2,1) | 0.7 (right) | -0.1 + 0.7 | 0.6 |
| (4,1) | 0.7 (left) | -0.1 + 0.7 | 0.6 |
| (1,1) | 0.6 (right or up) | -0.1 + 0.6 | 0.5 |
 
**Ask for class notes to get drawing, it potrays better** 
 
---
 
## 7. Summary
 
- **Grid world + transition model** (80/10/10) = a world where actions can go wrong.
- **Optimal policy** π*(s) = the action with the highest expected utility (argmax).
- **Bellman equation**: U(s) = R(s) + γ · max expected neighbor utility.
- It's a **system of nonlinear equations** (because of max), so we can't just solve it in O(n³).
- **Value iteration**: start at 0, apply the **Bellman update** over and over until values converge.
- **Deterministic example**: each square = best neighbor − 0.1.
- **Stochastic example**: slipping lowers values near danger. (3,2) is worth 0.54 instead of 0.64. **Done in class on board**

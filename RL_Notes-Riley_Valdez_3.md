### Recall about Reinforcement Learning
- Have actions
- Have States
- Missing Transition Function
- Missing Reward Function
- Fix a policy instead of invent new one

##TD Learning
Upi(s) = Upi(s) _ gamma (R(s) + alpha (R(s) + gamma Upi(s') -Upi(s))
- one step from s to s' w/ reward R(s)

# Ex:
 Grid World

|           | Col 1      | Col 2 | Col 3 | Col 4         |
|-----------|------------|-------|-------|---------------|
| **Row A** | →          | →     | →     |     T1        |
| **Row B** | ↑          | WALL  | ↑     |    T2         |
| **Row C** |  →         |  →    | ↑     |    ↑          |

U(1C) = 0
U(2C) = 0
U(3c) = 9
U(4c) = 0
U(T2) = -1 Where T2 is an action where you get -1 reward, so this changes 4b to use the equation from TD Learning:

U(4b) = 0 +  alpha (-1 + gamma(0-0) ) where it becomes -1


## After more trials:
- Going straight to bad reward
- Alternative path to bad reward by turning right when the wall is on our left
- Going to good reward by starting right
- Almost meeting good reward, but falling into the bad reward

|           | Col 1      | Col 2 | Col 3 | Col 4         |
|-----------|------------|-------|-------|---------------|
| **Row A** | 0          | 0     | .3    |     1         |
| **Row B** | 0          | WALL  | -.5   |    -1         |
| **Row C** |  -.3       |  -.3  | -.3   |    -.7        |

Many trails later, when now alpha is = to 1/3, gamma is assumed to be 1:
Alpha is updated in different ways, and updated based on many turns, a good easy way to update alpha is to update it based on how many times states have been seen.

U(1c) = 0.3 + alpha(0 + gamma 0 - (-0.3)
U(1c) = 0.3 + 1/3(0 + gamma 0 - (-0.3) = -.2
Now going to 1b:
U(1B) = 0 + 1/3(0 + gamma 0-0) = 0
U(1A) = 0, because 1A to 2A updates almost nothing since both of their utilities are 0
Now going from 2A to 3A:
U(2A) = 0 + 1/3(0 + gamma (0.3 - 0)) = .1

U(3A) = 0.3 + 1/3(0 + gamma(1-0.3) = 17/30, about .57

*Note*: When updating TD learning, "update where we were", Terminal nodes must have "exit" action


# Active Reinforcement Learning:
Same as before but choose an action
- random
- greedy
- explorer
too much explotation: learn poor model of world
too much exploration: Learn world quickly but performs poorly in terms of reward

Epsilon-Greedy: w/p 1 - epsilon select best action
                w/p epsilon is more randomly
                epsilon is a paramter between 0 and 1 and tells us how greedy we should be.

1/t - greedy: Epsilon-greedy w/ epsilon = 1/t

Exploration function: f(u, n) = {big reward if n < N, or u}

## Summary: 
- MDPs allow us to model the real world which is random, or a stochastic world
- last week: plan in random world
- Passive RL: learn world from experience
- Active RL: Chooses policy to learn world and perform well -- Tries balancing both
-    Can be totally greedy, explorer, epsilon, or 1/t Greedy

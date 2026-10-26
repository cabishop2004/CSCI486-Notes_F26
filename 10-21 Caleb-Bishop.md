# Bayesian Networks & Sampling
**Class: 10/21/26**

## 1. Where We Are

Previous Bayesian Network topics:

- BN representation
- Inference in a BN
- Constructing a BN
- Best variable ordering: **causal ordering**

### Example Bayesian Network

```text
Fire       Robbery
   \       /
     Alarm
     /   \
   John  Mary
```

Interpretation:

- Fire can cause Alarm.
- Robbery can cause Alarm.
- Alarm can cause John to call.
- Alarm can cause Mary to call.

---

# 2. Approximate Inference

Exact inference may become expensive in large Bayesian networks.

Instead, we can use **sampling** to approximate probabilities.

## Sampling intuition

To estimate the area of an irregular shape inside a rectangle:

1. Randomly scatter points across the rectangle.
2. Count how many land inside the shape.
3. Calculate:

```text
points inside shape
------------------- × rectangle area
total points
```

Example:

```text
13 points inside
11 points outside

total = 24
```

So:

```text
estimated area = (13 / 24) × rectangle area
```

### Key idea

> More samples → generally better estimate.

We can use the same principle with probability distributions.

---

# 3. Sampling Bayesian Networks

Instead of calculating a probability exactly, generate many possible **worlds** from the Bayesian network.

A world is one complete assignment of values to all variables.

Example:

```text
Fire    = False
Robbery = False
Alarm   = False
John    = False
Mary    = True
```

The worlds must be generated according to the probabilities represented by the network.

---

# 4. Prior Sampling

**Prior sampling** generates complete worlds directly from the Bayesian network.

## Algorithm

### Step 1
Start with variables that have **no parents**.

Sample them from their prior distributions.

Example:

```text
P(Fire)    = 0.002
P(Robbery) = 0.001
```

### Step 2
Once a variable's parents have been assigned, sample that variable from its **conditional distribution**.

Example:

```text
Fire + Robbery
      ↓
    Alarm
```

After Fire and Robbery are known:

```text
sample Alarm from P(Alarm | Fire, Robbery)
```

### Step 3
Continue down the network.

After Alarm is assigned:

```text
sample John from P(John | Alarm)
sample Mary from P(Mary | Alarm)
```

### Step 4
Repeat until all variables are assigned.

Then start again and create another world.

### Result

Prior sampling creates samples from the BN's **joint distribution**.

---

# 5. Estimating Probabilities With Samples

For an ordinary probability:

```text
P(X)
≈
# samples where X is true
-------------------------
# total samples
```

For a conditional probability such as:

```text
P(Fire | John)
```

we care about samples where:

```text
John = True
```

and ask how many of those also have:

```text
Fire = True
```

---

# 6. Rejection Sampling

Rejection sampling uses prior sampling but removes worlds that disagree with the evidence.

## Algorithm

```text
1. Generate a world using prior sampling.
2. Check the evidence.
3. If the world disagrees with the evidence:
      reject it.
4. If it agrees:
      keep it.
5. Estimate probability using only accepted samples.
```

### Example

Query:

```text
P(Fire | John = True)
```

If a generated world contains:

```text
John = False
```

throw that world away.

Only worlds where:

```text
John = True
```

are used.

## Formula idea

```text
P(Query | Evidence)
≈
accepted samples matching Query
-------------------------------
all accepted samples
```

---

# 7. Main Problem With Rejection Sampling

If the evidence is rare:

```text
P(Evidence) ≈ very small
```

then almost all samples get rejected.

Example:

```text
10,000 worlds generated
9,990 disagree with evidence
10 usable worlds
```

That is extremely wasteful.

### Key weakness

> Rare evidence → lots of rejected samples → poor efficiency.

---

# 8. Likelihood Weighting

Likelihood weighting avoids rejecting samples just because they disagree with evidence.

Instead:

> **Force evidence variables to their observed values.**

Then sample the other variables normally.

## Basic algorithm

For each variable in causal order:

### If it is NOT evidence

Sample it normally:

```text
X ~ P(X | Parents(X))
```

### If it IS evidence

Set it directly to the observed value:

```text
X = evidence value
```

Do **not** randomly sample it.

Instead update the sample's weight:

```text
weight *= P(X = evidence | Parents(X))
```

---

# 9. Why We Need the Weight

We forced the evidence to occur.

That means every generated world agrees with the evidence, even if that evidence was unlikely.

So the weight records:

> How likely would this evidence actually have been in this world?

A very likely piece of evidence gives the world a larger weight.

An unlikely piece of evidence gives it a smaller weight.

---

# 10. Rejection Sampling vs. Likelihood Weighting

| Rejection Sampling | Likelihood Weighting |
|---|---|
| Samples evidence variables normally | Evidence variables are fixed |
| Rejects samples that disagree | Keeps the samples |
| Uses ordinary counts | Uses weighted counts |
| Can waste many samples | Usually makes better use of samples |
| Bad when evidence is rare | Designed to handle evidence more efficiently |

---

# 11. Three Things to Remember

### Prior Sampling

```text
Sample every variable according to the BN.
```

Produces samples from the joint distribution.

### Rejection Sampling

```text
Prior sample
→ reject anything inconsistent with evidence.
```

### Likelihood Weighting

```text
Fix evidence
→ sample everything else
→ weight each world by likelihood of evidence.
```

---

# 12. Fast Exam Review

**Why use sampling?**  
To approximate probabilities when exact inference is expensive.

**Why does prior sampling follow causal order?**  
A variable's parents need values before its conditional distribution can be used.

**What does rejection sampling reject?**  
Any sampled world inconsistent with the evidence.

**Biggest weakness of rejection sampling?**  
Rare evidence causes most samples to be discarded.

**What does likelihood weighting do with evidence?**  
Fixes evidence variables instead of sampling them.

**Why assign a weight?**  
Because the evidence was forced; the weight accounts for how probable that evidence actually was.

**What improves a sampling estimate?**  
Generally, more samples.

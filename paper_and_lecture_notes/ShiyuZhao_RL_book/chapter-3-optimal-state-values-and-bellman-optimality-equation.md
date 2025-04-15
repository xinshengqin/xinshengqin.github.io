---
title: Optimal State Values and Bellman Optimality Equation
layout: default
parent: Notes for Papers and Lectures
---

## Overview

This chapter introduces the **Bellman Optimality Equation (BOE)** and the concept of **optimal state values**, which form the mathematical foundation for defining and computing **optimal policies** in reinforcement learning. It builds upon the standard Bellman equation introduced in the previous chapter and sets the stage for **value iteration** (to be discussed in Chapter 4).

---

## Key Concepts

### Optimal Policy and Optimal State Value

- **Definition**: A policy $\pi^*$ is optimal if:

  $$
  v_{\pi^*}(s) \geq v_{\pi}(s), \quad \forall s \in \mathcal{S}, \forall \pi
  $$

- The **optimal state value function** $v^*$ is defined as:

  $$
  v^*(s) = \max_{\pi} v_{\pi}(s) = v_{\pi^*}(s)
  $$

- **Optimal policies** may not be unique, but $v^*$ is always unique.

---

## Methods/Algorithms

### Bellman Optimality Equation (BOE)

The **elementwise form** of the BOE:

$$
v(s) = \max_{\pi(s) \in \Pi(s)} \sum_{a \in \mathcal{A}} \pi(a|s) \left[ \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s'|s,a) v(s') \right]
$$

The part in square bracket is the action-value function:

$$
q(s, a) \doteq \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s'|s, a) v(s')
$$

Then:

$$
v(s) = \max_{a \in \mathcal{A}} q(s, a)
$$

### Matrix-Vector Form of BOE

$$
v = \max_{\pi \in \Pi} (r^\pi + \gamma P^\pi v)
$$

Define:

$$
f(v) \doteq \max_{\pi \in \Pi} (r^\pi + \gamma P^\pi v)
$$

Then BOE becomes:

$$
v = f(v)
$$
Later in the chapter the author shows how to solve for $v$ when $f(v)$ satisfies certain properties.

---


### Contraction Mapping Theorem

- If $f$ is a **contraction mapping**, then:
  - A unique fixed point $x^*$ exists.
  - Iterative updates $x_{k+1} = f(x_k)$ converge to $x^*$ exponentially.

### Application to BOE

- $f(v)$ is shown to be a contraction mapping under the $\|\cdot\|_\infty$ norm:

  $$
  \|f(v_1) - f(v_2)\|_\infty \leq \gamma \|v_1 - v_2\|_\infty
  $$

- Hence, the BOE:
  - Has a unique solution $v^*$
  - Can be solved iteratively:

    $$
    v_{k+1} = f(v_k)
    $$

### Solving Optimal Policy

There’s at least one deterministic optimal policy (**optimal greedy policy**).

Once $v^*$ is found, we can find such a policy by defining:

$$
\pi^*(a|s) = \begin{cases}
1, & \text{if } a = \arg\max_a q^*(s, a) \\
0, & \text{otherwise}
\end{cases}
$$

with:

$$
q^*(s, a) = \sum_{r} p(r|s,a) r + \gamma \sum_{s'} p(s'|s,a) v^*(s')
$$

---

## Important Observations
### Existence and Uniqueness

- The optimal value function $v^*$ always exists and is unique.
- There may be multiple (even infinite) optimal policies.
- At least one deterministic greedy policy is always optimal.

### Affine Invariance of Rewards

- Transforming all rewards via an affine map $r \mapsto \alpha r + \beta$ with $\alpha > 0$ does **not** change the optimal policy.
- It changes the optimal value function as:

  $$
  v' = \alpha v^* + \frac{\beta}{1 - \gamma} \cdot \mathbf{1}
  $$

### Effect of Discount Factor $\gamma$

- A higher $\gamma$ leads to **far-sighted** behavior.
- A lower $\gamma$ results in **short-sighted** decisions.
- At $\gamma = 0$, the agent maximizes only **immediate rewards**.

---

## Summary

- **BOE is central** to computing optimal policies and values.
- Solving the BOE yields both $v^*$ and $\pi^*$.
- The **contraction mapping theorem** provides both existence and an iterative solution method.
- The **value iteration algorithm**, discussed next, is based directly on these ideas.


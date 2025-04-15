---
title: State Values and Bellman Equation
layout: default
parent: Notes for Papers and Lectures
---

# Overview

This chapter lays the mathematical foundation for evaluating policies in reinforcement learning by introducing *state values* and the *Bellman equation*.
The main focus is on defining the return, understanding bootstrapping, and establishing a recursive relationship among state values. In turn, these ideas lead to policy evaluation methods—both via closed-form and iterative algorithms. The chapter also extends the discussion to *action values* and their relationship with state values, forming the basis for many advanced reinforcement learning algorithms.

---

# Key Concepts

## Returns

- **Definition:** The return, denoted by $G_t$, is the discounted sum of future rewards starting from time $t$.
  $$
  G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots
  $$

## State Value Function

- **Definition:** The state value function for a policy $\pi$ is defined as the expected return when starting in state $s$:
  $$
  v^\pi(s) = \mathbb{E}\bigl[G_t \mid S_t = s\bigr]
  $$
- **Dependence:** It depends on the state $s$, the policy $\pi$, and it is independent of the time index.
- **Bootstrapping:** The key idea is that the value of a state depends on the values of its successor states—a recursive dependency that is central to the Bellman equation.

## Bellman Equation

- **Purpose:** The Bellman equation formalizes the relationship between state values of successive states.
- **Derivation:** By rewriting the return as
  $$
  G_t = R_{t+1} + \gamma\, G_{t+1},
  $$
  and taking expectations, we get
  $$
  v^\pi(s) = \mathbb{E}\bigl[R_{t+1} \mid S_t=s\bigr] + \gamma\, \mathbb{E}\bigl[G_{t+1} \mid S_t=s\bigr].
  $$
- **Expanded Form:** Incorporating the underlying dynamics (action choices, reward, and state transitions), the Bellman equation becomes:
  $$
  v^\pi(s) = \sum_{a \in A(s)} \pi(a \mid s) \left[ \sum_{r \in \mathcal{R}} p(r\mid s,a) \, r + \gamma \sum_{s' \in \mathcal{S}} p(s'\mid s,a) \, v^\pi(s') \right], \text{for all } s\in \mathcal{S}
  $$

### Matrix-Vector Form

- **Compact Representation:** If states are indexed as $s_1, s_2, \dots, s_n$, define
  - Value vector: $\mathbf{v}^\pi = \begin{bmatrix} v^\pi(s_1) \\ \vdots \\ v^\pi(s_n) \end{bmatrix}$,
  - Reward vector: $\mathbf{r}^\pi = \begin{bmatrix} r^\pi(s_1) \\ \vdots \\ r^\pi(s_n) \end{bmatrix}$,
  - Transition matrix: $[P^\pi]_{ij} = p(s_j \mid s_i)$.
  
  Then the Bellman equation is written as:
$$
\mathbf{v}^\pi = \mathbf{r}^\pi + \gamma P^\pi \mathbf{v}^\pi.
$$

### Solving the Bellman Equations

#### **Closed-Form Solution**
This linear system can be solved as

$$
\mathbf{v}^\pi = (I - \gamma P^\pi)^{-1}\mathbf{r}^\pi.
$$

#### Iterative Solution

- **Algorithm:** Rather than inverting a matrix, state values can be computed iteratively:
  $$
  v_{k+1} = \mathbf{r}^\pi + \gamma P^\pi v_k,\quad k=0,1,2,\dots
  $$
- **Convergence:** Under standard conditions (with $\gamma < 1$), the sequence $\{v_k\}$ converges to $\mathbf{v}^\pi$.


## Action Value Function

- **Definition:** The action value function, denoted by $q^\pi(s,a)$, represents the expected return of taking action $a$ in state $s$ and then following policy $\pi$:
  $$
  q^\pi(s,a) = \mathbb{E}\bigl[G_t \mid S_t = s, A_t = a\bigr].
  $$
- **Relationship with State Value:** 
    - The state value is the expectation of the action values weighted by the policy:
  $$
  v^\pi(s) = \sum_{a \in A(s)} \pi(a\mid s) \, q^\pi(s,a).
  $$
    - Conversely, the action value can be expressed by state value as follows:
  $$
  q^\pi(s,a) = \sum_{r \in \mathcal{R}} p(r\mid s,a) \, r + \gamma \sum_{s' \in \mathcal{S}} p(s'\mid s,a) \, v^\pi(s').
  $$


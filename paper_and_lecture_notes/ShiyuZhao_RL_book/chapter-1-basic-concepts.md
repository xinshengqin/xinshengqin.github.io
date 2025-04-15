---
title: Basic Concepts
layout: default
parent: Notes for Papers and Lectures
---

This chapter lays the foundation for reinforcement learning by introducing its core components using intuitive grid world examples. It defines the key elements—**states, actions, transitions, policies, rewards, returns, and the framework of Markov Decision Processes (MDPs)**—and establishes the mathematical language used throughout the book.


## 1. Overview

The chapter introduces the basic concepts that underpin reinforcement learning. Using a grid world example, it explains how an agent (a robot) interacts with an environment through state transitions, takes actions according to a policy, and receives rewards. The goal is to build the necessary mathematical framework—centered on Markov Decision Processes—for analyzing and designing learning algorithms in later chapters.

---

## 2. Key Concepts

### 2.1 Grid World Example

- **Scenario:**  
  A robot navigates a $3 \times 3$ grid (states $s_1, s_2, \ldots, s_9$) with designated accessible, forbidden, and target cells.
- **Objective:**  
  The agent seeks a “good” policy that guides it from any starting state to the target while avoiding forbidden cells and unnecessary detours.

### 2.2 States and Actions

- **States:**  
  Each cell in the grid represents a state. The state space is defined as  
  $$
  S = \{s_1, s_2, \ldots, s_9\}.
  $$
- **Actions:**  
  The possible moves are upward ($a_1$), rightward ($a_2$), downward ($a_3$), leftward ($a_4$), and staying still ($a_5$). Thus, the action space is  
  $$
  A = \{a_1, a_2, a_3, a_4, a_5\}.
  $$
- **Action Constraints:**  
  Depending on the state (e.g., at the grid boundary), not all actions may be permissible.

### 2.3 State Transitions

- **Deterministic Transitions:**  
  For example, taking action $a_2$ in state $s_1$ results in  
  $$
  s_1 \xrightarrow{a_2} s_2.
  $$
- **Mathematical Representation:**  
  The state transition is modeled by conditional probabilities. For instance,  
  $$
  p(s_2 \mid s_1, a_2) = 1 \quad \text{and} \quad p(s_i \mid s_1, a_2) = 0 \quad \text{for } i \neq 2.
  $$
- **Special Cases:**  
  Actions leading outside the grid or into forbidden cells may result in the agent remaining in the same state.

### 2.4 Policy

- **Definition:**  
  A policy $\pi(a \mid s)$ defines the probability of taking action $a$ in state $s$.
- **Deterministic Policy:**  
  Example:  
  $$
  \pi(a_2 \mid s_1) = 1 \quad \text{and} \quad \pi(a \mid s_1) = 0 \quad \text{for } a \neq a_2.
  $$
- **Stochastic Policy:**  
  Actions may be chosen with different probabilities. For example, in state $s_1$:  
  $$
  \pi(a_2 \mid s_1) = 0.5, \quad \pi(a_3 \mid s_1) = 0.5.
  $$
- **Tabular Representation:**  
  Policies can be stored as tables where rows correspond to states and columns to actions.

### 2.5 Reward

- **Reward Function:**  
  The reward $r(s, a)$ provides feedback after an action. In the grid world, rewards are defined as:
  - $r_{\text{boundary}} = -1$ for attempting to exit the grid.
  - $r_{\text{forbidden}} = -1$ for entering a forbidden cell.
  - $r_{\text{target}} = +1$ for reaching the target.
  - $r_{\text{other}} = 0$ for all other transitions.
- **Purpose:**  
  Rewards guide the agent’s behavior, but the optimal strategy is based on the cumulative (or total) reward rather than immediate rewards alone.

### 2.6 Trajectories, Returns, and Episodes

- **Trajectory:**  
  A sequence of state-action-reward tuples, for example:  
  $$
  s_1 \xrightarrow{a_2} s_2 \xrightarrow{a_3} s_5 \xrightarrow{a_3} s_8 \xrightarrow{a_2} s_9.
  $$
- **Return:**  
  The total reward accumulated along a trajectory. For instance,  
  $$
  \text{Return} = 0 + 0 + 0 + 1 = 1.
  $$
- **Discounted Return:**  
  To handle infinite trajectories, a discount factor $\gamma \in (0,1)$ is introduced:
  $$
  \text{Discounted Return} = \sum_{t=0}^{\infty} \gamma^t r_t.
  $$
  For example, if rewards start accruing at time $t = 3$, this can be expressed as:
  $$
  \text{Discounted Return} = \gamma^3 \left(\frac{1}{1-\gamma}\right).
  $$
- **Episodes:**  
    -  Finite trajectories ending in terminal states (episodic tasks) or ongoing sequences (continuing tasks) are discussed. 
       - Such a finite trajectory is called a episode (or a trial).
    - The treatment of terminal states (e.g., as absorbing states or allowing exit and re-entry) is also explored.

### 2.7 Markov Decision Processes (MDPs)

- **Framework Components:**
  - **State Space ($S$):** The set of all possible states.
  - **Action Space ($A(s)$):** The set of allowable actions in each state.
  - **Reward Set ($R(s, a)$):** The set of possible rewards for each state-action pair.
- **Model Dynamics:**
  - **Transition Probability:**  
    $$
    p(s' \mid s, a), \quad \text{with} \quad \sum_{s'} p(s' \mid s, a) = 1.
    $$
  - **Reward Probability:**  
    $$
    p(r \mid s, a), \quad \text{with} \quad \sum_{r \in R(s,a)} p(r \mid s, a) = 1.
    $$
- **Policy:**  
  Defined as $\pi(a \mid s)$ with  
  $$
  \sum_{a \in A(s)} \pi(a \mid s) = 1.
  $$
- **Markov Property:**  
  The future state depends only on the current state and action, not on the past history:
  $$
  p(s_{t+1} \mid s_t, a_t, s_{t-1}, a_{t-1}, \ldots, s_0, a_0) = p(s_{t+1} \mid s_t, a_t),
  $$
  This also implies that the future reward only depends on the current state and action, not on the past history.
  $$
  p(r_{t+1} \mid s_t, a_t, s_{t-1}, a_{t-1}, \ldots, s_0, a_0) = p(r_{t+1} \mid s_t, a_t).
  $$
  This property is crucial for developing the Bellman equation in later chapters.



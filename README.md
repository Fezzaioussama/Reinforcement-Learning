# Reinforcement Learning Algorithms
## Overview

This repository provides implementations and explanations of key reinforcement learning algorithms: Value Iteration, SARSA (State-Action-Reward-State-Action), and Q-learning. Each algorithm is discussed in terms of its objectives, key components, and how it compares to others.
## Value Iteration
Objective

Value iteration is used to find the optimal value function and policy for a Markov Decision Process.
Algorithm

The algorithm iteratively updates the value function until it converges to the optimal values.
Update Rule

The value iteration update rule is given by:
V(s)←max⁡a(R(s,a)+γ∑s′P(s′∣s,a)⋅V(s′))V(s)←maxa​(R(s,a)+γ∑s′​P(s′∣s,a)⋅V(s′))
where:

    V(s)V(s) is the value of state ss.
    R(s,a)R(s,a) is the immediate reward of taking action aa in state ss.
    P(s′∣s,a)P(s′∣s,a) is the transition probability from state ss to state s′s′ given action aa.
    γγ is the discount factor.

Policy Extraction

Once the value function converges, an optimal policy can be derived by selecting actions that maximize the expected cumulative reward.
Comparison with SARSA and Q-learning
Model

Value iteration is a model-based approach, meaning it requires knowledge of the transition probabilities and rewards.
Policy

It directly computes the optimal policy during the process.
Exploration

Unlike SARSA and Q-learning, value iteration does not involve exploration since it relies on knowledge of the complete model.
## SARSA (State-Action-Reward-State-Action)
Policy used for updates

SARSA is an on-policy algorithm. This means that it updates its Q-values based on the current policy that is being followed. In other words, it considers the action taken in the current state and the next state according to the current policy.
Update Rule

The SARSA update rule is given by:
Q(s,a)←Q(s,a)+α[R+γQ(s′,a′)−Q(s,a)]Q(s,a)←Q(s,a)+α[R+γQ(s′,a′)−Q(s,a)]
Where:

    Q(s,a)Q(s,a) is the Q-value for the current state-action pair.
    αα is the learning rate.
    RR is the immediate reward.
    γγ is the discount factor.
    Q(s′,a′)Q(s′,a′) is the Q-value for the next state-action pair.

## Q-learning
Policy used for updates

Q-learning is an off-policy algorithm. This means that it updates its Q-values based on the optimal policy, not necessarily the one it is currently following. It considers the best action for the next state, regardless of the action taken in the current state.
Update Rule

The Q-learning update rule is given by:
Q(s,a)←Q(s,a)+α[R+γmax⁡aQ(s′,a)−Q(s,a)]Q(s,a)←Q(s,a)+α[R+γmaxa​Q(s′,a)−Q(s,a)]
Where:

    Q(s,a)Q(s,a) is the Q-value for the current state-action pair.
    αα is the learning rate.
    RR is the immediate reward.
    γγ is the discount factor.
    max⁡aQ(s′,a)maxa​Q(s′,a) is the maximum Q-value for the next state across all possible actions.

Key Differences
Policy Updates

    SARSA updates based on the current policy (on-policy).
    Q-learning updates based on the optimal policy (off-policy).

Action Selection

    In SARSA, the next action is selected based on the current policy.
    In Q-learning, the next action is selected based on the action with the maximum Q-value.

Convergence

    SARSA tends to be more conservative as it considers the current policy.
    Q-learning can be more aggressive in finding the optimal policy.

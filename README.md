# Reinforcement Learning Algorithms

> Value Iteration, SARSA, and Q-learning implemented on FrozenLake — three ways to solve the same problem, and what separates them.

All three are implemented with NumPy against OpenAI Gym's `FrozenLake-v1`: a
grid world where the agent crosses a frozen lake to reach a goal, and falling in
a hole ends the episode.

## Run it

```bash
pip install gym numpy
```

Open [`Reinforcement_Learning.ipynb`](Reinforcement_Learning.ipynb) and run top
to bottom.

## The three algorithms

| | Value Iteration | SARSA | Q-learning |
|---|---|---|---|
| **Needs a model?** | Yes — transitions and rewards | No | No |
| **Policy** | Off-policy (planning) | **On-policy** | **Off-policy** |
| **Explores?** | No | Yes (ε-greedy) | Yes (ε-greedy) |
| **Learns** | `V(s)`, then extracts π | `Q(s,a)` | `Q(s,a)` |
| **Temperament** | Exact | Conservative | Aggressive |

### Value Iteration

A planning method: it assumes you already know the transition probabilities and
rewards, and computes the optimal value function directly.

```
V(s) ← max_a [ R(s,a) + γ · Σ_s' P(s'|s,a) · V(s') ]
```

Iterate until `V` converges, then extract the policy by picking the action that
maximises expected return in each state. There's no exploration because there's
nothing to discover — the model is given.

That assumption is also its limitation. You rarely know `P(s'|s,a)` for a real
problem, which is what the other two methods are for.

### SARSA — on-policy

```
Q(s,a) ← Q(s,a) + α [ R + γ · Q(s',a') − Q(s,a) ]
```

`a'` is the action the policy **actually takes** in `s'`, chosen by the same
ε-greedy rule used to act. So SARSA evaluates the policy it's following,
exploration included.

The consequence: SARSA accounts for the cost of its own exploration. Near a
cliff edge, it learns that "walk along the edge" is risky *because* ε-greedy will
occasionally step off. It ends up with a safer, more conservative route.

### Q-learning — off-policy

```
Q(s,a) ← Q(s,a) + α [ R + γ · max_a Q(s',a) − Q(s,a) ]
```

The only change is `max_a Q(s',a)` in place of `Q(s',a')` — Q-learning updates
toward the **best** next action regardless of what it actually did. It learns
the optimal policy while following an exploratory one, which is what "off-policy"
means.

The consequence is the mirror image of SARSA: Q-learning converges to the
optimal path even if that path is dangerous under exploration, because it never
prices in the ε-greedy mistakes it will make along the way.

### Symbols

`α` learning rate · `γ` discount factor · `R` immediate reward ·
`s, a` current state and action · `s', a'` next state and action

## ε-greedy exploration

Both TD methods use the same `epsilon_greedy` helper: with probability ε take a
random action, otherwise take `argmax_a Q(s,a)`. This is the exploration/
exploitation trade-off in its simplest form — without exploration the agent
never discovers better routes; without exploitation it never uses what it
learned.

## The takeaway

SARSA and Q-learning differ by exactly one term in the update rule, and that one
term changes what they converge to. Value Iteration is the baseline that shows
what both are approximating when the model is unknown.

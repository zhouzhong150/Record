two Reinforcement learning features

- trial-and-error search
- delayed reward

Reinforcement learning formalized as the optimal control of incompletely-known Markov decision processes. 

Markov decision processes are intended to include three aspects

- sensation
- action
- goal

Machine learning paradigms

- supervised,  labeled data
- unsupervised, find structure hidden in unlabeled data
- reinforcement learning, to maximize a reward signal

subelements

- policy defines way of behaving
- reward signal defines goal of a problem, primary basis for altering the policy, good in an immediate sense
- value function, good in the long run
- model of the environment



Action choices are made based on value judgments, highest value instead of highest rewards



Methods for solving RL, model the env

- model-based  planning
- model-free trial-and-error



RL relies on the concept of state, which is (how the env is)

- input to the policy and value function
- input to and output from the model



focus on RL methods that learn while interacting with env.
$$
V(S_t) \leftarrow V(S_t) + \alpha[V(S_{t + 1}) - V(S_t)]
$$

## Bandit problems

use training information to evaluate the actions taken instead of instructing.

evaluating depends entirely on the action taken, whereas instructive feedback is independent of the action taken.

to explore or exploit depends on the precise values of the estimates, uncertainties, and the number of remaining steps.

- Greedy with highest estimated value
- $\epsilon$-greedy, with small probability $\epsilon$ to select randomly from among all the actions with equal probability

Stationary problem
$$
Q_{n + 1} &= \frac{1}{n}\sum_{i = 1}^n R_i = \frac{1}{n}(R_n + \sum_{i = 1}^{n - 1} R_i) \\
&= \frac{1}{n}(R_n + (n - 1)\frac{1}{n - 1} \sum_{i = 1}^{n - 1} R_i) = \frac{1}{n}(R_n + (n - 1)Q_n) \\
&= Q_n + \frac{1}{n}(R_n - Q_n)
$$

$$
\text{NewEstimate} \leftarrow \text{OldEstimate} + \text{StepSize}[\text{Target} - \text{OldEstimate}]
$$

Nonstationary problem

$\alpha$ constant
$$
Q_{n + 1} = Q_n + \alpha(R_n - Q_n)
$$

$$
Q_{n + 1} = Q_n + \alpha(R_n - Q_n) \\
= \alpha R_n + (1 - \alpha)Q_n \\
\alpha R_n + (1 - \alpha)(\alpha R_{n - 1} + (1 - \alpha)Q_{n - 1}) \\ 
\alpha R_n + (1 - \alpha)\alpha R_{n - 1} + (1 - \alpha)^2Q_{n - 1} \\\
= (1 - \alpha)^nQ_1 + \sum_{i = 1}^n\alpha(1 - \alpha)^{n - i}R_i
$$



### Upper confidence bound

$N_t(a)$ a 已经被选择的次数。根号内用于度量 uncertainty，选的越多，uncertainty 越低，括号内是上界。更难拓展到其他问题：处理 nonstationary problems and large state spaces 都不很难。
$$
A_t = \text{argmax}_a [Q_t(a) + c\sqrt{\frac{\ln t}{N_t(a)}}]
$$

### Associative Search

involves both trial-and-error learning to search for the best actions and association of these actions with the situations in which they are best. often now called contextual bandits

## Finite Markov decision processes

<img src="pics/RL.png">

Bellman equations

value functions

return
$$
G_t = R_{t + 1} + R_{t + 2} + \dots + R_{T}
$$
discounted  return, $0 \le \gamma \le 1$, called the discount rate
$$
G_t = R_{t + 1} + \gamma R_{t + 2} + \gamma^2 R_{t + 3} + \dots = \sum_{k = 0}^{\infty} \gamma^k R_{t + k + 1}
$$
If $\gamma < 1$, $G_t$ is bounded, If $\gamma = 0$, the agent is "myopic", only concerned with maximizing immediate rewards.
$$
G_t = R_{t + 1} + \gamma R_{t + 2} + \gamma^2 R_{t + 3} + \dots \\
= R_{t + 1} + \gamma(R_{t + 2} + \gamma R_{t + 3} + \dots) = R_{t + 1} + \gamma G_{t + 1}
$$
**Policy** $\pi(a | s)$ is the probability that $A_t = a$ if $S_t = s$

**Value function** 
$$
v_{\pi}(s) = E_{\pi}[G_t | S_t = s] = E_{\pi}[\sum_{k = 0}^{\infty} \gamma^k R_{t + k + 1} | S_t = s]
$$
$v_{\pi}$, the state-value function for policy $\pi$, value function of a state s under a policy $\pi$
$$
q_{\pi}(s, a) = E_{\pi}[G_t | S_t = s, A_t = a] = E_{\pi}[\sum_{k = 0}^{\infty} \gamma^k R_{t + k + 1} | S_t = s, A_t = a]
$$
$q_{\pi}$ the action-value function for policy $\pi$, the value of taking action a in state s under a policy $\pi$

**Having**
$$
P(X = x | Y = y, Z = z) = P(X = x, Y = y | Z = z) / P(Y = y | Z = z)
$$

#### Theorem

$S_t = s, S_{t + 1} = s', G_{t + 1} = g'$
$$
E_{\pi}[v_{\pi}(s')| S_t = s] = E_{\pi}[E_{\pi}[G_{t + 1}| S_{t + 1} = s'] | S_t = s] = E_{\pi}[G_{t + 1} | S_t = s]
$$

$$
E_{\pi}[G_{t + 1} | S_{t + 1} = s'] = \sum_{g'}g' \Probability (g' | s')
$$

$$
E_{\pi}[E_{\pi}[G_{t + 1}| S_{t + 1} = s'] | S_t = s] = E_{\pi}[\sum_{g'}g' \Probability (g' | s') | S_t = s] \\
= \sum_{s'}\sum_{g'}g'\Probability (g' | s')\Probability (s' | s) \\
= \sum_{s'}\sum_{g'}g'\Pr(g', s' | s) \\
= \sum_{g'}\sum_{s'}g'\Pr(g', s' | s) = \sum_{g'}\Pr(g' | s) = E[G_{t + 1} | S_{t} = s]
$$

furthermore, we have **Bellman equation**
$$
v_{\pi}(s) = E_{\pi}[G_t | S_t = s] \\
= E_{\pi}[R_{t + 1} + \gamma G_{t + 1} | S_t = s] \\
= E_{\pi}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1} = s') | S_t = s] \\
= \sum_a \pi(a | s)\sum_r p(r | s, a)r + \gamma \sum_a \pi(a | s)\sum_{s'}p(s' | s, a)v_{\pi}(s') \\
= \sum_a \pi(a | s)\sum_r \sum_{s'} p(r, s' | s, a)r + \gamma \sum_a \pi(a | s)\sum_{s'} \sum_r p(s', r | s, a)v_{\pi}(s') \\
= \sum_a \pi(a | s)\sum_{s'} \sum_r p(s', r | s, a)[r + \gamma v_{\pi}(s')]
$$



$\pi \ge \pi'$ iff $\forall s \in S, v_{\pi}(s) \ge v_{\pi'}(s)$

optimal policy, better than or equal to all other policies. denote all the optimal policies by $\pi_*$

optimal policy shares optimal state-value function, $v_*(s) = \text{max}_{\pi} v_{\pi}(s), \forall s \in S$, and optimal action-value function, $q_*(s, a) = \text{max}_{\pi}q_{\pi}(s, a), \forall s \in S, a \in A(s)$, and we have
$$
q_*(s, a) = E[R_{t + 1} + \gamma v_*(S_{t + 1}) | S_t = s, A_t = a]
$$
Bellman optimality equation
$$
v_*(s) = \text{max}_{a \in A(s)} q_{\pi_*}(s, a) \\
= \text{max}_a E_{\pi_*}[G_t | S_t = s, A_t = a] \\
= \text{max}_a E_{\pi_*}[R_{t + 1} + \gamma G_{t + 1} | S_t = s, A_t = a] \\
= \text{max}_a E[R_{t + 1} + \gamma v_*(S_{t + 1}) | S_t = s, A_t = a] \\
= \text{max}_a \sum_{s', r}p(s', r | s, a)[r + \gamma v_*(s')]
$$

$$
q_* (s, a) = E[R_{t + 1} + \gamma \text{max}_{a'}q_*(S_{t + 1}, a') | S_t = s, A_t = a] \\
= \sum_{s', r}p(s', r | s, a)[r + \gamma \text{max}_{a'}q_*(s', a')]
$$


Solving Bellman optimality equation for optimal policy, with three assumption

- accurately know the dynamics of the env
- have enough computational resources to complete the computation of the solution
- the Markov property


---

differ in several ways with respect to their efficiency and speed of convergence

### Dynamic programming

well developed mathematically, require a complete and accurate model of the env

### Monte Carlo methods

don't require a model of the env and are conceptually simple, but not suited for step-by-step incremental computation.

### Temporal difference learning

require no model, and fully incremental, are more complex to analyze.

## Reference

https://jmichaux.github.io/_notebook/2018-10-14-bellman/

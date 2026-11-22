---
tags:
  - "#TRUNK"
---
In Sueprvised Learning, we had a Teacher (labels). In Unsupervised Leraning, we had No Teacher (Self-discovery). In Reinforcement Learning, we have a Critic.

# Overview
- The Agent: The learner (the robot, the chess bot)
- The EnviromentL The world the agent lives in (the chess board)
- The Reward (or penalty): The feedback signal

Mechanism
1. The Agent observes the State ($S$) of the enviroment.
2. The agent takes an Action ($A$). 
3. The Enviroment gives the Agent a Reward ($R$) (postive or negative). 
4. The Agent updates it strategy (Policy) to maximise total future rewards

# Exploration vs Exploitation Dilemma
When an agent takes an action, it learns the outcome. Following this the next action the agent takes is based on previous learned outcome as well as weather it is explotative or an explorer.
- Explotation: The agent does what it knows yields a reward. It relies on current knowledge to maximise score
- Expolaration: The agent tries a random, unkown action to see what happes. It risks failure to potentially find a better strategy

If you only exploit you will only ever get a consistnet reward and never get a better reward, if you only explore you will only ever get random rewards and never settle on the best reward.

The Solution is Epsilon-Greedy ($\epsilon$-Greedy). We give the Agent a parameter called Epsilon ($\epsilon$). This represents the probability of exploring

- If $\epsilon$ = 0.1, the Agent exploits (uses best knowledge) 90% of the time and explores (acts randomly) 10% of the time
- Usually we start with high exploration ($\epsilon$ = 1.0) and slowly lower it ("decay") as the Agent gets smarter

# The Q-Table
The Agent remembers if a certain action at a certain state is good or bad through a spreadsheet called. a Q-Table

- Rows: Every possible State ($S$) the agent can be in
- Columns: Every possible Action ($A$) the agent can take
- Cell Value: The Quality ($Q$) of taking that Action in that State

The Goal: The goal of training is to fill this table with accurate numbers
- If $Q$(Edge of Cliff, Jump) = -100 the Agent learns not to jump
- If $Q$(Near Goal, Step Right) = +10, th eAgent learns to step right

When the Agent needs to make a decision (Exploitation), it simply looks at therow for its current State, scans across the clumns, and picks the Action with the highest number

## The Belmman Equation (The Update Rule)
This dictates how we update the numbers in the Q-table after an action is taken.

When an agent takes an action and sees a result, it updates the value of the previous state using this formula

$$Q_{new}(s,a) = Q_{old}(s,a) + \alpha \underbrace{[R + \gamma \cdot \max(Q_{future}) - Q_{old}(s,a)]}_{\text{Temporal Difference Error}}$$

1. $Q_{old}$: What we thought the value was before we tried it.
2. $\alpha$ (Alpha - Learning Rate): How fast we learn. (Low = stubborn; High = fickle).
3. $R$ (Reward): The immediate points we just got (e.g., -100 for falling).
4. $\gamma$ (Gamma - Discount Factor): How much we care about future rewards vs. immediate rewards. (0 = I want candy now; 0.9 = I will save for retirement).
5. $\max(Q_{future})$: The best possible score we think we can get from the next state we landed in.

The new value is a combination of the old value plus a correction based on the Reality (R) plus the promis of the future (max Q)

### Example Walkthrough
Scenario:
- Current State: You are standing next to the Goal
- Action: You step Right (into the Goal).
- Old Q-Value ($Q_{old}$): 0 (You have never done this before, so you know nothing).
- Reward ($R$): +10 (You hit the goal!).
- Learning Rate ($\alpha$): 0.5 (You learn moderately fast).
- Discount Factor ($\gamma$): 0.9.
- Future Value ($\max Q_{future}$): 0 (The game ends when you hit the goal, so there are no future moves).

$$Q_{new} = Q_{old} + \alpha \cdot (R + (\gamma \cdot \max Q_{future}) - Q_{old})$$

$$0 + 0.5 * (10 + (0.9 * 0) - 0) = 5$$

# Algorithms
RL algorithms generally fall into two categories:

1. Value-Based (like DQN): "I calculate the score of every action and pick the highest."
2. Policy-Based: "I don't calculate scores; I just learn a direct map of what to do."

## Value Based 
- [[DQN (Deep Q-Network)]]: The classic. Good for discrete actions (Left, Right, Jump). Used in retro games (Atari).

## Policy Based
- [[PPO (Proximal Policy Optimization)]]: 
- [[DDPG (Deep Deterministic Policy Gradient)]]: The current industry standard (used by OpenAI). It is stable and less prone to crashing during training than DQN. Used for training the AI in Dota 2 and robotics.Specifically designed for Continuous Control. If you are teaching a robot hand to grasp an egg (where you need precise pressure, not just "Open/Close"), you use this.

## Hybrid
- [[A3C (Asynchronous Advantage Actor-Critic)]]: Uses multiple "workers" (agents) playing in parallel environments to learn faster. Good for complex 3D environments (Doom, Quake).
- [[SAC (Soft Actor-Critic)]]: Optimizes for both reward and "Entropy" (randomness). This encourages the robot to succeed and be robust against errors. Excellent for real-world robotics.
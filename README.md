
# Boat Rental Optimization with Reinforcement Learning

## Project Overview

This project focuses on optimizing the maintenance schedule for a boat rental company using Reinforcement Learning (RL) methods. The aim is to maximize profit by determining optimal maintenance strategies for boats during peak and off-peak seasons.

## Problem Description

The boat rental company in Zeedorp, NL, rents out speedboats which need to be maintained periodically. Each boat has a condition rating from 1 (excellent) to 4 (bad). Boats in worse condition require more costly maintenance.

### Key Details:

- **Daily Rental Price:** €60
- **Maintenance Costs:**
  - Rating 2 or 3: €40
  - Rating 4: €100 (includes compensation for lost rental)

The transition probabilities for boat ratings are:

| i \ j | 1 | 2 | 3 | 4 |
|-------|---|---|---|---|
| 1 (Excellent) | 0.8 | 0.1 | 0.1 | 0 |
| 2 (Good) | 0 | 0.6 | 0.3 | 0.1 |
| 3 (Average) | 0 | 0 | 0.5 | 0.5 |
| 4 (Bad) | 0 | 0 | 0 | 1 |

## Project Goals

1. **Spring Break Optimization:**
   - Determine the optimal maintenance schedule for a 9-day spring break to maximize total profit.
   - Use a backward solution method starting from the last day and moving backwards.

2. **Summer Period Strategy:**
   - Develop a maintenance strategy for an indefinite summer period with a discount factor γ = 0.9 to maximize the discounted profit.
   - Implement RL methods suited for continuous and indefinite periods.

## Solution Approach

### Spring Break Optimization

The solution involves a backward induction approach to calculate the optimal maintenance schedule, ensuring the boat is rentable throughout the period and maximizing profit.

### Summer Period Optimization

For the ongoing summer period, we use RL techniques to find a strategy that maximizes the total discounted reward. This involves setting up an MDP with appropriate reward functions and discount factors.

## Code Implementation

This repository contains the Python code for the solution, which includes:

- **QAgent Class:** Implements the RL agent that makes decisions based on state-action values.
- **Model Definition:** Sets up the MDP and RL models for both the spring break and summer period optimizations.
- **Experimentation:** Tests and validates the models using simulated data.

### Code

Here's a sample of the code:

```python
import numpy as np
import random

class QAgent:
    def __init__(self, price=20, agents_n=3):
        self.slope = random.uniform(-1, 0)
        self.intercept = np.random.randint(50, 100)
        self.price = price  # Fixed price for all demands
        self.actions_n = 10  # Number of actions or demand points
        self.action_table = {}

    def generate_action_table(self, myactions, opponent_actions):
        action_table = {}

        opp1_demand = opponent_actions[0][0]
        opp2_demand = opponent_actions[0][1]

        for agent_action in myactions:
            action_table[agent_action] = {}
            for opp1_action in opp1_demand:
                for opp2_action in opp2_demand:
                    action_table[agent_action][(opp1_action, opp2_action)] = 0  # Initialize with None or any default value

        return action_table

    def update_action_table(self, k, action_table, current_action, opponent_current_action):
        opponent1_action = opponent_current_action[0]
        opponent2_action = opponent_current_action[1]
        action_table[k][current_action][(opponent1_action, opponent2_action)] += 1
        return action_table[k]

    def predict_opponents_actions(self, k, action_table, current_action):
        if current_action not in action_table[k]:
            return None, None  # Action does not exist

        max_value_opp1 = float('-inf')
        max_value_opp2 = float('-inf')
        max_action_opp1 = None
        max_action_opp2 = None

        for (opp1action, opp2action), value in action_table[k][current_action].items():
            if value > max_value_opp1:
                max_value_opp1 = value
                max_action_opp1 = opp1action
            if value > max_value_opp2:
                max_value_opp2 = value
                max_action_opp2 = opp2action

        max_tuple = (max_action_opp1, max_action_opp2)
        return max_tuple

    def choose_action(self, Q_table, myactions, current_state, time):
        epsilon = 0.1 ** (4 * time / 500000)
        if np.random.rand() < epsilon:
            return np.random.choice(myactions)
        else:
            return max(Q_table[current_state], key=Q_table[current_state].get)

    def get_state(self, current_action, opponent_action1, opponent_action2):
        state = (current_action, (opponent_action1, opponent_action2))
        return state

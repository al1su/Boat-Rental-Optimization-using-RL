
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



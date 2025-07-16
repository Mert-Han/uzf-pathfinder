# UAV Zoo Feeding Problem (UZF) — HyFlex Implementation

## Overview

This project implements a HyFlex-compatible solution for the UAV Zoo Feeding (UZF) problem — a combinatorial optimisation challenge where a single UAV must visit all animal enclosures in a zoo to feed the animals in the minimum possible time. The UZF problem is a variant of the Travelling Salesman Problem (TSP), with domain-specific constraints and objectives.

## Problem Description

- A set of animal enclosures with known coordinates
- A food preparation area serving as both the start and end point
- The objective is to minimise total travel time (measured using Euclidean distance with the ceiling operator)
- All enclosures must be visited exactly once

## Architecture

### Core Components

- **`UZFDomain`** — Implements the HyFlex `ProblemDomain` interface, managing solution evaluation, heuristic application, and instance loading.
- **`UZFInstance`** — Stores problem instance data, including enclosure locations and the food preparation area.
- **`UZFSolution`** — Represents a solution as a permutation of enclosure visits, with cloning support for solution manipulation.

### Solution Representation

- Solutions are encoded as integer arrays, representing the order of enclosure visits.

## Low-Level Heuristics

### Mutation Operators

- **Adjacent Swap** — Swaps adjacent elements, applied multiple times based on intensity.
- **Reinsertion** — Removes and reinserts elements at different positions for diversity.

### Local Search Operators

- **Next Descent** — First-improvement hill climbing with adjacent swap neighbourhoods.
- **Davis Hill Climbing** — Systematic exploration with random starting points.
- **Steepest Descent** — Best-improvement hill climbing over all neighbours.

### Crossover Operators

- **Partially Mapped Crossover (PMX)** — Maintains permutation validity while combining parents.
- **Cycle Crossover** — Preserves relative positions of elements from both parents.

## Hyper-Heuristic: `MERT_HH`

The `MERT_HH` hyper-heuristic uses a multi-armed bandit strategy with an exponential reheating temperature schedule.

### Performance Tracking

- **Adaptive Weights** — Heuristic weights increase by 1.15 on success, decrease by 0.95 on failure.
- **Multi-Armed Bandit Selection** — Uses Boltzmann selection with temperature-controlled probabilities.

### Temperature Management

- **Simulated Annealing** — Cooling by 0.99 per iteration, reheating by 1.5 on stagnation.
- **Stagnation Detection** — Triggers reheating after 100 non-improving iterations.
- **Minimum Temperature Threshold** — Prevents excessive cooling to maintain exploration.

### Crossover Handling

- **Dual-Parent Strategy** — Randomly selects the current solution or the best-known solution as the second parent.
- **Separate Weight Management** — Crossovers start with low weights to balance exploration.

## Performance Optimisation Techniques

- **Delta Evaluation** — Re-evaluates only changed parts of the solution.
- **Efficient Neighbourhood Generation** — Minimises unnecessary solution object creation.
- **Memory Management** — Leverages HyFlex’s solution memory system.
- **Exploration-Exploitation Balance** — Achieved via temperature-controlled heuristic selection.
- **Adaptive Learning** — Performance-based heuristic adaptation.
- **Stagnation Recovery** — Reheating boosts exploration when progress stalls.
- **Crossover Integration** — Combines information from current and best solutions for offspring generation.
- **Intensity and Depth Control** — Allows fine-tuned heuristic behaviour.
- **Randomised Starting Positions** — Improves neighbourhood coverage.
- **Solution Quality Preservation** — Retains both current and best solutions.

## Usage

1. Load a problem instance (IDs 0–6 for different sizes).
2. Configure the hyper-heuristic with time limits and random seeds.
3. Run the search process.
4. Visualise the result with the included `UAVView` component.

## Visualisation

The visualiser shows:

- Enclosure locations and the food preparation area
- The best tour found by the hyper-heuristic
- Visual indicators of solution quality

## Technical Requirements

- Java implementation following HyFlex API standards
- Fully compatible with the HyFlex framework
- Conforms to standard HyFlex evaluation and comparison methods
- Comprehensive error handling and validation included

## Performance Characteristics

- Fast convergence to high-quality solutions
- Robust across various problem instances
- Efficient balance of solution quality and computational resources
- Adaptive behaviour that improves over time

## Summary

This implementation combines established optimisation methods with innovative hyper-heuristic design. It achieves strong performance across diverse UZF instances, thanks to its adaptive learning, balanced exploration and exploitation, and effective problem-specific handling.

## Contact <a id="contact"></a>

*Mert-Han H* – `contact.merthanharry@gmail.com` 

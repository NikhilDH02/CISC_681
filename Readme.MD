# Train Yard Search Assignment - CISC 681
**Programming Assignment 1: Basic Blind/Heuristic Search Techniques**

## Overview
This project implements various search algorithms to solve the train yard maneuvering problem, where railroad cars must be assembled into a specific order using only prescribed track movements.

## Files Structure
```
train_yard_search/
├── README.md                 # This file
└── PR1.ipynb                # Complete implementation in Jupyter notebook
```

## System Requirements
- Python 3.7+ (Google Colab recommended)
- Recommended: High-RAM runtime for larger problems (YARD_1, YARD_2)

## Quick Start

### Option 1: Google Colab (Recommended)
1. Upload `PR1.ipynb` to Google Colab
2. Set runtime to high-RAM if available (Runtime → Change runtime type → High-RAM)
3. Run all cells in sequence

## Core Functions

### Problem 1: `possible_actions(yard, state)`
Returns all legal actions possible from given state in given yard.
```python
actions = possible_actions(YARD_3, INIT_STATE_3)
# Returns: [('RIGHT', 1, 2), ('RIGHT', 1, 3), ('LEFT', 2, 1), ('LEFT', 3, 1)]
```
### Problem 2: `result(action, state)` / `apply_move(action, state)`
Applies an action to a state, returning the resulting new state.
```python
new_state = apply_move(('LEFT', 2, 1), INIT_STATE_1)
# Returns: [['*', 'e'], [], [], ['b', 'c', 'a'], [], ['d']]
```
### Problem 3: `expand(state, yard)`
Returns all successor states reachable in one action from given state.
```python
successors = expand(INIT_STATE_1, YARD_1)
# Returns list of all possible next states
```
## Search Algorithms

### Problem 4: Breadth-First Search (BFS)
```python
solution = blind_search(yard, initial_state, goal_state, 'BFS')
```
- **Optimal**: Guarantees shortest path (minimum moves)
- **Complete**: Will find solution if one exists
- **Use case**: Small to medium problems where optimality is crucial

### Problem 6: A* Tree Search
```python
solution = heuristic_search(yard, initial_state, goal_state, heuristic_misplaced_cars)
```
- **Optimal**: With admissible heuristic
- **Efficient**: Uses heuristic to guide search
- **Memory**: Higher memory usage due to tree structure

### Problem 7: A* Graph Search
```python
solution = heuristic_graph_search(yard, initial_state, goal_state, heuristic_misplaced_cars)
```
- **Optimal**: With admissible heuristic
- **Most Efficient**: Avoids revisiting states
- **Recommended**: For large problems (YARD_1, YARD_2)

## Heuristics

### Heuristic 1: Misplaced Cars
Counts cars not in their final goal positions.
```python
h1_value = heuristic_misplaced_cars(state, goal_state)
```
- **Admissible**: Each misplaced car needs ≥1 move
- **Simple**: Fast to compute

### Heuristic 2: Sequence Distance  
Counts cars on wrong tracks relative to goal.
```python
h2_value = heuristic_sequence_distance(state, goal_state)
```
- **Admissible**: Cars on wrong tracks need ≥1 move each
- **Effective**: Better guidance for complex problems

## Running Custom Test Cases

### Testing on Predefined Yards
```python
# Run complete test suite (all problems)
main()

# Test individual functions
test_problem1_complete()
verify_assignment_examples()
run_complete_performance_analysis()
```

### Creating Custom Yards
Add a new cell in the notebook:
```python
# Define connectivity (track connections)
CUSTOM_YARD = [(1, 2), (1, 3), (2, 4)]

# Define states (list of cars on each track, indexed from 1)
CUSTOM_INIT = [['*'], ['a'], ['b'], []]    # Engine on track 1, car 'a' on track 2, etc.
CUSTOM_GOAL = [['*', 'a', 'b'], [], [], []] # All cars assembled on track 1

# Test your custom yard
solution, nodes = heuristic_graph_search(CUSTOM_YARD, CUSTOM_INIT, CUSTOM_GOAL, heuristic_misplaced_cars)
print("Solution path:", solution)
print("Nodes expanded:", nodes)
```

## Predefined Test Cases

### Simple Test Cases (Start Here!)
- **YARD_3**: 2 cars, 3 tracks - Basic connectivity test
- **YARD_4**: 3 cars, 4 tracks - Medium complexity  
- **YARD_5**: 3 cars, 4 tracks - Cars initially out of order

### Complex Test Cases
- **YARD_2**: 5 cars, 5 tracks - Large problem for performance testing
- **YARD_1**: 5 cars, 6 tracks - Most complex, requires graph search

## Performance Guidelines

### Recommended Algorithm by Problem Size:
- **Small (≤3 cars)**: Any algorithm works fine
- **Medium (4-5 cars, simple)**: BFS or A* Tree
- **Large (5+ cars, complex)**: A* Graph Search only

### Memory Usage Warnings:
- **YARD_1 with BFS**: Will exceed memory (skipped per assignment)
- **YARD_1 with A* Tree**: High memory usage (may skip)
- **YARD_2 with BFS**: ~185 seconds, 5M+ nodes

## Example Usage

### Complete Test Run
```python
# Run full test suite (as submitted)
main()

# Expected output includes:
# - Problem 1-3 verification tests
# - Algorithm performance comparisons  
# - Speedup measurements
# - Solution paths for each yard
```

### Individual Algorithm Testing

```
# Test BFS on simple problem
bfs_solution, nodes = blind_search(YARD_3, INIT_STATE_3, GOAL_STATE_3, 'BFS')
print("BFS solution:", bfs_solution)

# Test A* with heuristic on complex problem  
astar_solution, nodes = heuristic_graph_search(YARD_2, INIT_STATE_2, GOAL_STATE_2, 
                                               heuristic_misplaced_cars)
print("A* solution:", astar_solution)
```

## Performance Results

Based on our testing:

| Yard | Algorithm | Moves | Nodes | Time | Speedup |
|------|-----------|-------|-------|------|---------|
| YARD_3 | BFS | 2 | 4 | 0.0001s | 1x |
| YARD_3 | A* Graph | 2 | 3 | 0.0001s | 1.33x |
| YARD_4 | BFS | 4 | 64 | 0.0008s | 1x |
| YARD_4 | A* Graph | 4 | 5 | 0.0001s | 12.8x |
| YARD_5 | BFS | 6 | 1039 | 0.015s | 1x |
| YARD_5 | A* Graph | 6 | 26 | 0.0006s | 40x |
| YARD_2 | BFS | 14 | 5.3M | 161s | 1x |
| YARD_2 | A* Graph | 14 | 972 | 0.025s | 5461x |


## Assignment Verification

This implementation satisfies all assignment requirements:
- roblems 1-7 implemented and tested
- Required check-expect tests pass
- Performance analysis exceeds 2-3x speedup requirement  
- Handles all specified test yards
- Proper algorithm justifications provided
- Admissible heuristics with proofs

## Implementation Notes
- All code is contained in the single Jupyter notebook PR1.ipynb
- Functions are organized by problem number with clear documentation
- Comprehensive testing framework included
- Performance optimizations implemented for large problems
- Memory-efficient graph search for complex yards

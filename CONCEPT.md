# Conceptual Progress Report

## Recap of Project Proposal
**How does removing individual proteins from a human DNA-repair protein-protein interaction (PPI) network affect its overall connectivity?** 

This project uses Breadth-First Search (BFS), a graph traversal algorithm, to identify connected components in a human DNA-repair PPI network before and after individual proteins are removed. The goal is to identify proteins that may play an important role in maintaining network connectivity by comparing the number and size of connected components after each removal.

The dataset will consist of human DNA-repair proteins and their known physical interactions, represented as an undirected graph where proteins are nodes and interactions are edges.

## Inputs, Outputs, and Assumptions
Precisely define:
The expected input(s) to your algorithm (including data types and formats).
The expected output(s) (what you return or produce).
State key assumptions (e.g., “All sequences are uppercase A/C/G/T,” “Graph is simple and directed,” “Emission probabilities are normalized”).

### Inputs


### Outputs


### Assumptions


## Detailed Pseudocode
```
Provide pseudocode that:
Has clear function signatures or step headings.
Explicitly describes main loops, conditionals, and data structures.
Handles at least one non-trivial edge case (e.g., empty input, unexpected characters, disconnected graph).
Use code-style formatting (indented blocks, consistent naming) so that another student could implement it in Python or another language.
```

## Complexity and Bottlenecks
Analyze the time and space complexity of your algorithm in terms of relevant parameters (e.g., sequence length, number of reads, number of states).
Identify the most expensive parts of the algorithm and discuss:
When performance might become a problem.
Any ideas you have for mitigating performance issues (e.g., pruning, indexing, approximate methods)?

## Validation and Testing Plan
Describe how you plan to test your implementation:
At least one small, hand-crafted example where you know or can reason about the correct answer.
At least one synthetic or real dataset for stress testing.
Explain:
What results do you expect from these tests?
What would constitute evidence that the algorithm is behaving incorrectly?
Outline the kinds of automated tests you will implement (e.g., unit tests for subfunctions, end-to-end tests, property/invariant checks).

## Updated Pitfall and Risk Log
Revisit the pitfalls from Part 1:
Which ones still seem relevant?
Which new pitfalls have emerged as you wrote the pseudocode?
For each risk, add a brief note on how your design (or upcoming implementation) will address it

## Generative AI Disclosure

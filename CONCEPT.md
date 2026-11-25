# Conceptual Progress Report

## Recap of Project Proposal
**How does removing individual proteins from a human DNA-repair protein-protein interaction (PPI) network affect its overall connectivity?** 

This project uses Breadth-First Search (BFS), a graph traversal algorithm, to identify connected components in a human DNA-repair PPI network before and after individual proteins are removed. The goal is to identify proteins that may play an important role in maintaining network connectivity by comparing the number and size of connected components after each removal.

The dataset will consist of human DNA-repair proteins and their known physical interactions, represented as an undirected graph where proteins are nodes and interactions are edges.

## Inputs, Outputs, and Assumptions

### Inputs
* **Protein-Protein Interaction (PPI) dataset:** A file containing pairs of interacting human DNA-repair proteins, represented as an edge list.

### Outputs
* **Baseline connectivity:** The number of connected components and the size of the largest component before any proteins are removed.
* **Summary table:** A table comparing the number of connected components and size of the largest component before and after each protein removal to identify which proteins have the greatest effect on network connectivity.

### Assumptions
* The PPI network is undirected, because protein-protein interactions are not directional.
* Each protein has a unique identifier, and duplicate interactions will be removed.
* proteins interacting with themselves will be excluded.
* The graph is unweighted, meaning the algorithm only considers whether two proteins interact, not how strong or reliable that interaction is.
* Each protein will be removed one at a time from the original network, so the effect of each protein can be measured independently.

## Program Overview
| Function | Purpose |
|---|---|
| `build_graph(edge_list)` | Convert the PPI dataset into an adjacency list. |
| `bfs(graph, start)` | Find all proteins connected to a starting protein. |
| `find_components(graph)` | Use BFS to identify all connected components in the network. |
| `remove_protein(graph, protein)` | Create a modified network without the selected protein or its interactions. |
| `analyze_network(graph)` | Measure connectivity before and after removing each protein. |

## Detailed Pseudocode
```

FUNCTION build_graph(edge_list):
    Initialize an empty dictionary called graph

    FOR each (protein1, protein2) in edge_list:
        IF protein1 equals protein2:
            CONTINUE  // Skip self-loops

        IF protein1 is not in graph:
            graph[protein1] = empty set

        IF protein2 is not in graph:
            graph[protein2] = empty set

        Add protein2 to graph[protein1]
        Add protein1 to graph[protein2]

    RETURN graph


FUNCTION bfs(graph, start):
    Initialize visited = {start}
    Initialize queue = [start]

    WHILE queue is not empty:
        current = Remove first element from queue

        FOR each neighbor in graph[current]:
            IF neighbor is not in visited:
                Add neighbor to visited
                Add neighbor to end of queue

    RETURN visited


FUNCTION find_components(graph):
    Initialize visited = empty set
    Initialize components = empty list

    FOR each protein in graph:
        IF protein is not in visited:
            component = bfs(graph, protein)

            Add component to components
            Add all proteins in component to visited

    RETURN components


FUNCTION remove_protein(graph, protein):
    Initialize new_graph as a copy of graph
    Copy each neighbor set so the original is unchanged

    IF protein is not in new_graph:
        RETURN new_graph

    FOR each neighbor in new_graph[protein]:
        Remove protein from new_graph[neighbor]

    Remove protein from new_graph

    RETURN new_graph


FUNCTION analyze_network(graph):
    baseline_components = find_components(graph)
    baseline_count = Number of baseline_components

    IF baseline_count equals 0:
        baseline_largest = 0
    ELSE:
        baseline_largest = Maximum size of baseline_components

    Initialize results = empty list

    FOR each protein in graph:
        modified_graph = remove_protein(graph, protein)
        components = find_components(modified_graph)

        component_count = Number of components

        IF component_count equals 0:
            largest_component = 0
        ELSE:
            largest_component = Maximum size of components

        change_count = component_count - baseline_count
        change_largest = largest_component - baseline_largest

        Add to results:
            protein
            component_count
            largest_component
            change_count
            change_largest

    Create a summary table from results,including baseline values for comparison

    RETURN baseline_count, baseline_largest, results
```

## Complexity and Bottlenecks
### Time Complexity
Let **V** represent the number of proteins (vertices) and **E** represent the number of interactions (edges) in the PPI network.

- **Graph construction:** O(V + E), since the program creates an adjacency list containing the proteins and their interactions.
- **BFS:** O(V + E), since each protein and interaction is visited at most once during a complete traversal.
- **Finding connected components:** O(V + E), since BFS is used to explore each component until all proteins have been visited.
- **Protein removal:** O(V + E) per removal, since the graph is copied before removing the selected protein and its interactions.
- **Full network analysis:** O(V(V + E)), since connectivity is recalculated after removing each of the V proteins.

### Space Complexity
The adjacency list requires O(V + E) space to store the network. BFS also requires O(V) space for the queue and visited set. Creating a copy of the graph for each protein removal requires an additional O(V + E) space. Overall, the working space complexity is **O(V + E)**, excluding the final results table.

### Bottlenecks and Potential Improvements
The most computationally expensive part of the project will be repeatedly copying the graph and running the connected-component analysis after each protein removal. This may become slow for large PPI networks containing thousands of proteins and interactions.

To improve performance, the graph will be stored as an adjacency list rather than an adjacency matrix, which is more efficient for sparse networks. Using sets to track visited proteins will also allow fast membership checks during BFS. If performance becomes an issue, we could avoid copying the entire graph by temporarily excluding the selected protein during traversal instead.

## Validation and Testing Plan

### Small Hand-Crafted Data Example
I will first test the algorithm using a small graph with five proteins and four interactions:
```
A — B — C
    |
    D — E
```
The original network has one connected component containing all five proteins. After removing protein B, the network should split into three connected components: {A}, {C}, and {D, E}. The largest component should decrease from five proteins to two.

I will also test removing protein A, which should leave the remaining four proteins connected. These examples will help verify that the algorithm correctly identifies connected components and measures changes in connectivity.

### Stress Testing with Real PPI Data
I will test the algorithm using real human DNA-repair protein interaction data, identifying DNA-repair proteins through Reactome and obtaining their physical interactions from BioGRID. The dataset will be used to construct a larger network and evaluate how the algorithm performs with real protein interactions.

I expect the algorithm to identify connected components and measure changes in connectivity after each protein removal. Since the expected number and size of connected components are not known beforehand, I will compare my results with those produced by NetworkX to verify their accuracy. Differences between the two implementations would suggest an error in my algorithm. I will also measure runtime to see how performance changes with network size.

### Automated Testing
I plan to implement unit tests for the individual functions and end-to-end tests for the full analysis.

- **Graph construction:** Verify that interactions are added correctly and duplicate edges and self-loops are excluded.
- **BFS:** Check that all reachable proteins are visited without including proteins from disconnected components.
- **Connected components:** Test graphs with multiple components, isolated proteins, and empty graphs.
- **Protein removal:** Verify that the selected protein and its interactions are removed without changing the original graph.
- **Full analysis:** Compare the program's results against manually calculated values and confirm that the number of remaining proteins decreases by exactly one after each removal.

## Updated Pitfall and Risk Log
Several risks identified in Part 1 have been addressed through the algorithm design and pseudocode, while others will need to be considered during implementation and testing.

| Pitfall / Risk | Status / Plan to Address |
|---|---|
| **Incomplete protein removal:** Removing a protein without removing its interactions could produce incorrect results. | Addressed in the pseudocode by removing the protein from all neighboring adjacency lists. |
| **Accumulating removals:** Changes to the original graph could affect subsequent removal tests. | Addressed in the pseudocode by creating a separate graph copy for each protein removal. |
| **Incomplete interaction data:** Missing interactions could affect network connectivity results. | Still relevant. Results will be interpreted based on recorded interactions rather than the complete biological network. |
| **Runtime growth:** Repeating BFS for every protein may become computationally expensive. | Still relevant. I will measure runtime on the real dataset and consider avoiding graph copies if necessary. |
| **Empty graphs and isolated proteins (New):** These cases could cause errors when identifying connected components. | The pseudocode handles empty graphs, and automated tests will verify that isolated proteins are handled correctly. |
| **Validation with real data (New):** Expected results may not be known for a large PPI network. | I will compare results with NetworkX and use hand-crafted examples with known outcomes to verify correctness. |

## Generative AI Disclosure
I used Claude Opus 5.5 to assist with the following parts of this writeup:
1. **Formatting of all tables/charts:** I used Claude to format my written notes into cleanly markdown formatted tables to insert into my concept writeup to improve readability. 
2. **Time and space complexity:** I used Claude to better understand the time and space complexity of BFS and how it applies to my project. I asked it to explain the complexity of each function, which helped me write my complexity analysis and identify potential performance bottlenecks. 
3. **Automated testing plan:** I used Claude to help plan my automated unit testing plan for each function, including what to test and how to identify incorrect results.
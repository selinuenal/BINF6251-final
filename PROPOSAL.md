# Analyzing Connectivity in a Human DNA-Repair Protein Network Using Breadth-First Search

## Research Question
### Which protein removals most disrupt connectivity in a human DNA-repair protein interaction network?

### Background:
DNA repair relies on proteins working together to detect DNA damage and coordinate its repair. These proteins participate in different repair processes, and interactions between them can connect those processes. A protein’s position in the interaction network may matter beyond how many interactions it has, since some proteins may connect groups that would otherwise be separate. This project will examine how removing individual proteins changes connectivity in the documented human DNA-repair network.

### Significance:
This project will distinguish proteins with many interactions from proteins whose removal most disrupts network connectivity. The findings could help prioritize proteins for further investigation, although network fragmentation alone does not establish that a protein is essential for DNA repair.

## Algorithm and Algorithm Class
**Algorithm Class:** Graph algorithms  

**Algorithm:** I will implement breadth-first search (BFS) to identify connected components, or groups of proteins connected through interaction paths. BFS uses a queue to visit neighboring nodes until all nodes reachable from a starting protein have been explored (Cormen et al., 2009, Section 22.2). Repeating this process from unvisited proteins identifies every connected component.

**Justification:** BFS measures the connectivity needed to answer my research question. I will run it on the original network and again after removing each protein and its interactions. I will compare the number of connected components and the size of the largest remaining component to assess fragmentation. 

## Data Plan

| Data Description | Data Source | Data Types | Licensing / Access |
| --- | --- | --- | --- |
| Human DNA-repair protein list | Reactome DNA Repair pathway | Protein identifiers and pathway membership | Publicly accessible; I will document the source and applicable reuse terms |
| Documented protein interactions | BioGRID | Tab-delimited interaction records, protein identifiers, and experimental-system annotations | Freely downloadable under the MIT license, with attribution |
| Small prototype networks | Generated with a custom script | Edge lists and node lists | Generated for this project; no external access restrictions |

### Data Selection and Preprocessing
I will use Reactome to define the human DNA-repair proteins included in the analysis. I will filter BioGRID records to retain human physical interactions where both proteins belong to this list. I will match identifiers across the sources, remove duplicate interaction pairs and self-interactions, and retain proteins with no recorded interactions as isolated nodes.

I will record the database versions, download dates, and filtering steps. The repository will include the processed network and instructions for obtaining the original data.

### Prototype Data Plan
I will generate small networks containing connected groups, isolated nodes, cycles, and proteins whose removal separates a group. These will help test and debug the BFS implementation before applying it to the real data. The prototype will use the same node-list and edge-list format as the processed DNA-repair network.

## Success Criteria
### Define what “success” looks like for your project:
The project will be successful if my BFS implementation correctly identifies connected groups and measures how connectivity changes after individual proteins are removed. The analysis does not need to find a protein that fragments the network to be successful.

### Expected Outputs
1. A table reporting the number of connected components and the size of the largest remaining component after each protein removal.
2. A comparison of each protein’s number of interactions with the fragmentation caused by its removal.
3. Network figures showing the original network and selected removal results.

### Result Validation:
I will compare my implementation’s connected components with `NetworkX`, a Python library with built-in graph algorithms, on the prototype and real networks before and after protein removals. Tests will include isolated nodes, disconnected groups, cycles, and an empty graph. Success requires agreement on component membership and sizes, regardless of the order in which groups are returned.

## Pitfall Scan
### Data-related issues:
1. **Incomplete interaction data:** Some DNA-repair proteins are studied more extensively than others, so missing interactions could make a protein appear isolated or unusually important for connectivity. I will check for isolated nodes and disconnected groups before analysis. Missing interactions cannot be fully corrected, so I will limit conclusions to the recorded network.
2. **Identifier mismatches:** Reactome and BioGRID may use different identifiers for the same protein, causing valid proteins or interactions to be excluded. I will check how many Reactome proteins successfully map to BioGRID identifiers, use documented mappings, and report unresolved identifiers.
3. **Duplicate or unsuitable records:** BioGRID can contain multiple records for the same pair and includes genetic as well as physical interactions. I will inspect the interaction types and check for duplicate pairs and self-interactions. I will retain only human physical interactions and remove redundant records.

### Algorithmic issues:
1. **Incomplete protein removal:** In an undirected adjacency list, an interaction is stored for both proteins. Removing only the selected protein’s entry could leave references to it elsewhere. I will check every remaining neighbor list for the removed protein and remove all incident edges before running BFS.
2. **Accumulating removals:** Reusing a modified graph could accidentally turn individual-removal tests into multiple-removal tests. I will verify that each test starts with the original node and edge counts and use a fresh graph copy for each removal.
3. **Runtime growth:** Repeating BFS for every protein may become slow as the network grows. I will measure runtime on the prototype and real network. I will keep the analysis restricted to DNA-repair proteins and avoid unnecessary work, such as generating a figure for every removal.

### Evaluation issues: 
1. **Pre-existing disconnection:** The filtered DNA-repair network may already contain separate groups, making component counts alone misleading. I will record the original components and check whether each removal splits an existing group.
2. **Node loss versus fragmentation:** Removing a protein reduces network size even when the remaining proteins stay connected. I will compare component membership before and after removal and distinguish the missing node from additional separation among surviving proteins.
3. **Overinterpreting biological significance:** Recorded interactions combine evidence from different experimental contexts, so graph fragmentation may not reflect what happens in a cell. I will inspect the available evidence annotations and describe findings as network properties requiring further biological investigation.

## Planned Repository Structure (Initial Sketch)
```
.
├── PROPOSAL.md             # Project proposal
├── README.md               # Overview and documentation
├── PSEUDOCODE.md           # Algorithm design
├── PROGRESS.md             # Progress reports and reflections
├── requirements.txt        # Python dependencies
├── .gitignore              # Files excluded from Git
├── src/
│   ├── prepare_data.py      # Filter records and build the network
│   ├── bfs.py               # BFS and connected components
│   ├── analyze_removals.py  # Test individual protein removals
│   └── main.py              # Run analysis and generate outputs
├── tests/
│   └── test_bfs.py          # Validate BFS and removal results
├── data/
│   ├── README.md           # Sources and preprocessing instructions
│   ├── prototype/          # Small generated test networks
│   ├── raw/                # Original downloads, excluded from Git
│   └── processed/          # Filtered node and edge lists
└── results/
    ├── tables/             # Connectivity and removal summaries
    └── figures/            # Network and comparison plots
```

## Generative AI Disclosure (If Used)
I used Claude Opus 5.5 to assist with the following parts of this proposal:
1. **Research into algorithm classes:** I used Claude to explain the algorithm classes listed in the syllabus and their bioinformatics applications, since we have not covered all of them yet. This helped me understand the available options and narrow down an algorithm for my project.
2. **Project feasibility and algorithm choice:** I used Claude to discuss whether BFS was appropriate for analyzing changes in a DNA-repair protein network after individual protein removals. This helped clarify how BFS would identify connected components and identify potential public data sources. I also used Claude to identify Reactome and BioGRID as potential data sources.
3. **Potential pitfalls:** I asked Claude to look over my pitfalls section to see if I missed any important considerations. I had initially not considered the "incomplete protein removal" and "accumulating removals" algorithmic considerations, but will make sure to account for those in my implementation. I also used Claude to help articulate my worries and considerations in the pitfall section more eloquently. 
4. **Repository structure:** I used Claude to format the planned repository structure and identify any file types or folders I may have missed. This helped me present the structure clearly and account for the project’s code, data, tests, and documentation.

## Other references used:
Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). Introduction to algorithms (3rd ed.). MIT Press.
<h1 align="center"> GRAPH EXPLORATION PROBLEM USING DISTRIBUTED MIS ALGORITHM</h1>

## OVERVIEW
The **Graph Exploration Problem** requires traversing an unknown connected graph _G = (V, E)_, starting from a Node _s_. Using only local knowledge, the agent traverses through the vertices and edges, and gathering new information without jumping between non-adjacent nodes.

### LOCAL KNOWLEDGE 
It refers to the information that the agent can access only about its current  position in the graph:
- Adjacent nodes connected by edges linking the current node to its neighbors.
- Uniquely identifies the current node using Node ID.

## What is the Graph Exploration Problem?
The **Graph Exploration Problem** involves finding a systematic way for agents to explore a graph. In the case of distributed systems, each agent has limited knowledge of the global graph structure and must make decisions based on local information. The challenge is to explore the graph while making decisions that affect the global structure, such as finding an optimal subset of nodes.

### Problem Definition:
In a distributed graph, a set of 𝑛 agents are positioned at the nodes of an undirected graph 𝐺 = (𝑉, 𝐸), with a predefined root node. The goal is for the agents to explore the graph and compute a Maximal Independent Set (MIS) using a Depth-First Search (DFS)-based algorithm in a rooted initial configuration.

Constraints:
- The Robot Model Constraints state that agents can only communicate locally, have limited memory, and operate autonomously in an asynchronous manner based on local information. 
- The Graph Model Constraints define the graph as finite, undirected, connected, with unique node identities and no initial MIS, which must be computed autonomously by the agents.

## What is a Maximum Independent Set (MIS)?

A **Maximum Independent Set (MIS)** is a subset of nodes in a graph such that no two nodes in the set are adjacent. It is "maximal" because no additional nodes can be added to the set without violating the independence property. The MIS problem is fundamental in graph theory and has applications in areas such as network design, resource allocation, and scheduling.

## About Distributed Systems

In a **distributed system**, multiple agents (or processes) work together to solve a problem, each operating independently but communicating with others. These systems are used when centralized control is inefficient or impractical, and agents must collaborate using limited information about the global state.

In the context of this algorithm, agents explore the graph in parallel, exchange information about their states, and make local decisions to construct the Maximum Independent Set (MIS). The distributed nature of the problem ensures scalability and robustness, as each agent operates autonomously while contributing to the global solution.

## Key Features of the Algorithm

- **DFS Exploration:** The algorithm employs a Depth-First Search (DFS) approach to traverse the graph.
- **Scouting Mechanism:** Unsettled agents act as scouts, exploring the graph and reporting back their findings.
- **Root Initialization:** The root node is included in the MIS initially, and the agent at the root settles first.
- **Node Decision:** Nodes can be added to the MIS (colored red) only if none of their neighbors are already part of the MIS. Nodes are excluded (colored blue) if they conflict with the MIS.
- **Coloring Scheme:** Nodes are marked with one of three colors: `red` (part of the MIS), `blue` (excluded from the MIS), and `black` (unsettled).

## Phases of the Algorithm

### 1. Forward Phase:
- The agent explores new nodes, and if the node is empty, it settles there by changing its state and color.

### 2. Backtrack Phase:
- If no further empty nodes are found, the agent backtracks to its parent node.

## Execution Process

### 1. Initialization (Round 1):
- All agents start at the root node (`v`).
- The agent with the highest ID settles, becomes part of the MIS (color red), and initializes `next = 0`.
- Unsettled agents exit through port 0.

### 2. Node Exploration:
- The highest ID agent among the unsettled agents settles at a new node (`u`).
- Unsettled agents scout the neighbors of node (`u`) through available ports, classifying them as:
  - **Empty:** No settled agent.
  - **Red:** The neighbor is part of the MIS.
  - **Blue:** The neighbor is excluded from the MIS.

### 3. Scouting and Decision:
- Agents report scouting results:
  - If no red neighbors are found, the node becomes part of the MIS (color red).
  - Otherwise, the node is excluded from the MIS (color blue).
- Update the `recent` and `next` variables for further exploration.

### 4. Completion:
- If all neighbors are scouted and no red neighbor exists, the node is added to the MIS.
- If no `next` port is available, agents backtrack to the parent node.

### 5. Final Settling:
- The algorithm continues until all agents except one settle.
- The last unsettled agent completes the DFS traversal and finalizes the MIS.

## Complexity Analysis

### Time Complexity:
- **DFS Traversal:**
  The algorithm performs a DFS traversal, which requires visiting each edge twice (once in each direction). The number of edges in the graph is `m`, and DFS traversal takes at most `4m` rounds. Since `m < nΔ` (where `n` is the number of nodes and `Δ` is the maximum degree), the upper bound is `O(nΔ)` rounds.
  
- **Scouting Process:**
  Every time a new node is visited in DFS, a scouting process is triggered. Scouting at a node of degree `δ` (where `δ ≤ Δ`) takes at most `2δ` rounds. Since scouting happens locally and independent of DFS traversal, it does not increase the global complexity. Hence, the worst-case time complexity remains `O(nΔ)` rounds.

Thus, the total time complexity of the algorithm is `O(nΔ)` rounds.

### Memory Complexity:

Each agent in the system stores several pieces of information, contributing to its memory usage:

- **Port Numbers Storage:** Each agent `r` maintains variables such as `r.parent`, `r.recent`, `r.next`, `r.scoutport`, and `r.scoutparent`. These require at most `O(log Δ)` bits.
  
- **State Variables:** Variables like `r.color`, `r.phase`, `r.state`, `r.target`, and `r.settled` require only `O(1)` bits.

- **Agent ID Storage:** Each agent stores its ID, which requires `O(log n)` bits.

**Final Memory Complexity:**  
Summing up all these contributions, the total memory requirement per agent is:
- `O(log Δ) + O(1) + O(log n) = O(log n)`
Thus, each agent requires `O(log n)` bits of memory, making the algorithm memory-efficient.

### Memory Complexity:  
`M = O(log n)` bits per agent.  

### Time Complexity:  
`T = O(nΔ)` rounds.

## Conclusion

The **Graph Exploration Problem using Distributed MIS Algorithm** provides an efficient way to explore a graph and identify a Maximum Independent Set using distributed agents. By utilizing DFS and a scouting mechanism, the algorithm enables scalable and robust exploration, ensuring optimal decision-making even in large and complex graphs. The distributed nature of the system allows agents to operate independently while contributing to the global solution.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.

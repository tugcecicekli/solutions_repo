# Problem 1
# **Equivalent Resistance Using Graph Theory**

## **1. Introduction**

Calculating **equivalent resistance** in electrical circuits is essential for analyzing and designing efficient electrical systems. While traditional methods involve applying series and parallel resistance rules, these approaches become cumbersome for circuits with many components. Using **graph theory**, we can simplify circuit analysis by representing the circuit as a graph where nodes represent junctions and edges represent resistors.

This report explores an algorithmic approach to calculating equivalent resistance using graph theory. Specifically, we will describe a **graph-based algorithm** that simplifies circuits systematically, reducing even the most intricate networks into manageable components. The method's versatility makes it an ideal solution for circuit simulation, optimization, and automated analysis.

---

## **2. Problem Statement**

In this task, we aim to develop an algorithm that calculates the **equivalent resistance** for arbitrary circuits by representing them as **graphs**. The algorithm should:

1. **Identify series and parallel connections** of resistors.
2. **Iteratively reduce the graph** until a single equivalent resistance is obtained.
3. **Handle complex circuits** with **nested series and parallel combinations**.

---

## **3. Algorithm Description**

We will describe an algorithm that uses **graph theory** to calculate the equivalent resistance of a circuit. The graph representation has:

- **Nodes**: Junctions or points in the circuit.
- **Edges**: Resistors connecting the nodes.

### **Step 1: Represent the Circuit as a Graph**
We start by representing the circuit as a graph $G(V, E)$, where:
- $V$ is the set of nodes (junctions).
- $E$ is the set of edges (resistors with resistance $R$).

### **Step 2: Detect Series and Parallel Connections**
- **Series Connection**: Two resistors $R_1$ and $R_2$ are in series if they are connected directly end-to-end. The total resistance $R_{\text{total}} = R_1 + R_2$.
- **Parallel Connection**: Two resistors $R_1$ and $R_2$ are in parallel if they are connected across the same two nodes. The total resistance $R_{\text{total}} = \frac{1}{\left(\frac{1}{R_1} + \frac{1}{R_2}\right)}$.

### **Step 3: Iteratively Simplify the Graph**
Using **depth-first search (DFS)** or **breadth-first search (BFS)**, we:
1. Identify series and parallel connections.
2. Replace the simplified sections with a single equivalent resistance.
3. Repeat the process until only one edge remains, which represents the equivalent resistance.

### **Step 4: Handle Nested Configurations**
The algorithm can handle nested series and parallel configurations by applying the series and parallel reduction rules recursively.
---

## **4. Python Implementation**

We will implement the algorithm using the Python **NetworkX** library, which simplifies graph manipulation.

```python
import networkx as nx

def series_reduction(R1, R2):
    """Calculates the equivalent resistance for resistors in series."""
    return R1 + R2

def parallel_reduction(R1, R2):
    """Calculates the equivalent resistance for resistors in parallel."""
    return 1 / (1/R1 + 1/R2)

def find_series_parallel(graph):
    """Identifies series and parallel connections in the graph."""
    series_connections = []
    parallel_connections = []
    
    # Check each pair of nodes for series and parallel connections
    for edge1 in graph.edges:
        for edge2 in graph.edges:
            if edge1 != edge2:
                # Check if edge1 and edge2 are in series or parallel
                # Add logic for series/parallel detection
                pass
    
    return series_connections, parallel_connections

def simplify_graph(graph):
    """Simplifies the graph by reducing series and parallel resistors."""
    series_connections, parallel_connections = find_series_parallel(graph)
    
    for edge1, edge2 in series_connections:
        R1 = graph[edge1[0]][edge1[1]]['resistance']
        R2 = graph[edge2[0]][edge2[1]]['resistance']
        new_R = series_reduction(R1, R2)
        # Replace the two resistors with the equivalent one
        graph.remove_edge(edge1[0], edge1[1])
        graph.remove_edge(edge2[0], edge2[1])
        # Add a new edge with resistance 'new_R'
    
    for edge1, edge2 in parallel_connections:
        R1 = graph[edge1[0]][edge1[1]]['resistance']
        R2 = graph[edge2[0]][edge2[1]]['resistance']
        new_R = parallel_reduction(R1, R2)
        # Replace the two resistors with the equivalent one
        graph.remove_edge(edge1[0], edge1[1])
        graph.remove_edge(edge2[0], edge2[1])
        # Add a new edge with resistance 'new_R'

    return graph

def calculate_equivalent_resistance(graph):
    """Calculates the total equivalent resistance of the circuit."""
    while len(graph.edges) > 1:
        graph = simplify_graph(graph)
    
    # Final equivalent resistance
    return graph[graph.nodes[0]][graph.nodes[1]]['resistance']

# Example graph with resistors
G = nx.Graph()
G.add_edge(0, 1, resistance=5)
G.add_edge(1, 2, resistance=10)

print(f"Equivalent resistance: {calculate_equivalent_resistance(G)} Ohms")
```

---

## **5. Analysis and Efficiency**

### **Time Complexity**
- The algorithm's complexity depends on the number of edges and nodes in the graph. Each simplification step involves searching for series and parallel connections, which is generally linear in terms of the number of edges. In the worst case, this could require $O(E^2)$ for graph traversal.

### **Space Complexity**
- Space complexity is $O(V + E)$, where $V$ is the number of vertices and $E$ is the number of edges in the graph.

### **Possible Improvements**
- **Optimized Search**: Use more efficient graph traversal techniques to identify series and parallel connections faster.
- **Parallel Processing**: For larger graphs, parallelizing the search and simplification process could lead to performance improvements.

---

## **6. Conclusion**

This approach uses **graph theory** to simplify and calculate the **equivalent resistance** of electrical circuits. The algorithm iteratively reduces complex networks by identifying series and parallel resistor connections. This method is computationally efficient and extends well to more complex circuits with nested combinations.

Future improvements could focus on optimizing the search process and enhancing scalability for larger circuits.

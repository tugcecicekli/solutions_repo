# Problem 1
## 1. Circuit Foundations: Theoretical Basis

### Series Rule:

If node A connects to node B, which connects to node C (and B has degree 2):

$$
R_{\text{eq}} = R_{AB} + R_{BC}
$$

### Parallel Rule:

If multiple edges connect the same pair of nodes:

$$
\frac{1}{R_{\text{eq}}} = \frac{1}{R_1} + \frac{1}{R_2} + \dots
$$


---
### Kirchhoff’s Voltage Law (KVL):

$$
\sum \text{Voltage drops in a loop} = 0
$$

For an RLC loop:

$$
V(t) = i(t)R + L\frac{di(t)}{dt} + \frac{1}{C} \int i(t)\,dt
$$

### Kirchhoff’s Current Law (KCL):

$$
\sum \text{Currents into a node} = \sum \text{Currents out}
$$

---

## RLC Circuits

Using charge $q(t)$, where $i(t) = \frac{dq}{dt}$:

$$
L \frac{d^2q(t)}{dt^2} + R \frac{dq(t)}{dt} + \frac{1}{C} q(t) = V(t)
$$

---

## 2. Steady-State and Resistance

In steady-state (DC analysis):

* $\frac{dq}{dt} = 0$, $\frac{d^2q}{dt^2} = 0$
* Inductors = short circuits
* Capacitors = open circuits
* Only **resistors** matter

So we simplify using:

$$
V = IR \quad \Rightarrow \quad R_{\text{eq}} = \frac{V}{I}
$$

---

## 3. Graph-Theoretic Circuit Model

| Concept    | Description                           |
| ---------- | ------------------------------------- |
| Node       | Junction (graph vertex)               |
| Edge       | Resistor (weight = resistance in Ω)   |
| START, END | Circuit terminals                     |
| Series     | Degree-2 node (collapsible path)      |
| Parallel   | Multiple edges between same two nodes |

---

```python
# Install required libraries
!pip install networkx matplotlib --quiet

# Imports
import networkx as nx
import matplotlib.pyplot as plt

# Draw circuit graph
def draw_graph(G, title="Circuit"):
    pos = nx.spring_layout(G, seed=42)
    labels = nx.get_edge_attributes(G, 'resistance')
    nx.draw(G, pos, with_labels=True, node_color='lightblue', node_size=1000, font_size=14)
    nx.draw_networkx_edge_labels(G, pos, edge_labels=labels, font_color='red')
    plt.title(title)
    plt.axis('off')
    plt.show()

# Collapse series resistors
def collapse_series(G, start="START", end="END"):
    changed = True
    while changed:
        changed = False
        for node in list(G.nodes):
            if G.degree[node] == 2 and node not in (start, end):
                u, v = list(G.neighbors(node))
                if G.has_edge(u, node) and G.has_edge(node, v):
                    r1 = G[u][node]['resistance']
                    r2 = G[node][v]['resistance']
                    G.add_edge(u, v, resistance=r1 + r2)
                    G.remove_node(node)
                    changed = True
                    break
    return G

# Collapse parallel resistors
def collapse_parallel(G):
    merged = set()
    for u, v in list(G.edges()):
        if (u, v) in merged or (v, u) in merged:
            continue
        edges = list(G.get_edge_data(u, v).values())
        if len(edges) > 1:
            resistances = [d['resistance'] for d in edges]
            r_eq = 1 / sum(1/r for r in resistances)
            G.remove_edges_from([(u, v)] * len(edges))
            G.add_edge(u, v, resistance=r_eq)
        merged.add((u, v))
    return G

# Main function
def compute_equivalent_resistance(G, start="START", end="END", show_steps=True):
    if show_steps:
        draw_graph(G, "Original Circuit")
    changed = True
    while changed:
        prev_edges = G.number_of_edges()
        G = collapse_series(G, start, end)
        G = collapse_parallel(G)
        changed = (G.number_of_edges() != prev_edges)
        if show_steps:
            draw_graph(G, "Simplified Step")
    return G[start][end]['resistance']

# Example circuit: START - A - END in series, plus a parallel edge
G = nx.MultiGraph()
G.add_edge("START", "A", resistance=5)
G.add_edge("A", "END", resistance=10)
G.add_edge("START", "END", resistance=15)  # Parallel resistor

# Run and visualize
req = compute_equivalent_resistance(G, "START", "END", show_steps=True)
print(f" Equivalent Resistance: {req:.2f} Ω")
```
Visit:[Colab](https://colab.research.google.com/drive/1S-2XQLto7CzYvL6rtMLSLUcyY9CDzIwc#scrollTo=eCMQmkc6Av15
)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-51.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-511.png?raw=true)

---
```python
# Series Only: START — 10Ω — A — 20Ω — END
G1 = nx.MultiGraph()
G1.add_edge("START", "A", resistance=10)
G1.add_edge("A", "END", resistance=20)

req1 = compute_equivalent_resistance(G1.copy(), "START", "END", show_steps=True)
print(f"Series Example → Equivalent Resistance: {req1:.2f} Ω")
```

Visit:[Colab](https://colab.research.google.com/drive/1S-2XQLto7CzYvL6rtMLSLUcyY9CDzIwc#scrollTo=eCMQmkc6Av15
)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-52.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-522.png?raw=true)

```python
# Parallel Only: START — 10Ω — END, START — 20Ω — END
G2 = nx.MultiGraph()
G2.add_edge("START", "END", resistance=10)
G2.add_edge("START", "END", resistance=20)

req2 = compute_equivalent_resistance(G2.copy(), "START", "END", show_steps=True)
print(f"Parallel Example → Equivalent Resistance: {req2:.2f} Ω")
```

Visit:[Colab](https://colab.research.google.com/drive/1S-2XQLto7CzYvL6rtMLSLUcyY9CDzIwc#scrollTo=eCMQmkc6Av15
)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-53.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-533.png?raw=true)

```python
# Nested: two series resistors in parallel with one resistor
# START — 5Ω — A — 5Ω — END
#       \__________15Ω_________/
G3 = nx.MultiGraph()
G3.add_edge("START", "A", resistance=5)
G3.add_edge("A", "END", resistance=5)
G3.add_edge("START", "END", resistance=15)

req3 = compute_equivalent_resistance(G3.copy(), "START", "END", show_steps=True)
print(f"Nested Example → Equivalent Resistance: {req3:.2f} Ω")
```
Visit:[Colab](https://colab.research.google.com/drive/1S-2XQLto7CzYvL6rtMLSLUcyY9CDzIwc#scrollTo=eCMQmkc6Av15
)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-54.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-544.png?raw=true)

```python
# Diamond: START — A — END, START — B — END
# All resistors = 10Ω
G4 = nx.MultiGraph()
G4.add_edge("START", "A", resistance=10)
G4.add_edge("A", "END", resistance=10)
G4.add_edge("START", "B", resistance=10)
G4.add_edge("B", "END", resistance=10)

req4 = compute_equivalent_resistance(G4.copy(), "START", "END", show_steps=True)
print(f"Diamond Example → Equivalent Resistance: {req4:.2f} Ω")
```
Visit:[Colab](https://colab.research.google.com/drive/1S-2XQLto7CzYvL6rtMLSLUcyY9CDzIwc#scrollTo=eCMQmkc6Av15
)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-55.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-555.png?raw=true)

### 4. Animation 

```python
from matplotlib import animation
from IPython.display import HTML

# Global frame list
animation_frames = []

def draw_graph_for_animation(G, title=""):
    fig, ax = plt.subplots()
    pos = nx.spring_layout(G, seed=42)
    labels = nx.get_edge_attributes(G, 'resistance')
    nx.draw(G, pos, with_labels=True, node_color='lightblue', node_size=1000, ax=ax)
    nx.draw_networkx_edge_labels(G, pos, edge_labels=labels, font_color='red', ax=ax)
    ax.set_title(title)
    ax.axis('off')
    animation_frames.append(fig)

def compute_equivalent_resistance_animated(G, start="START", end="END"):
    animation_frames.clear()
    draw_graph_for_animation(G.copy(), "Original Circuit")
    changed = True
    while changed:
        prev_edges = G.number_of_edges()
        G = collapse_series(G, start, end)
        G = collapse_parallel(G)
        changed = (G.number_of_edges() != prev_edges)
        draw_graph_for_animation(G.copy(), "Simplified Step")
    return G[start][end]['resistance']
```

```python
# Define your test circuit (e.g., diamond or nested)
G_example = nx.MultiGraph()
G_example.add_edge("START", "A", resistance=5)
G_example.add_edge("A", "END", resistance=5)
G_example.add_edge("START", "END", resistance=15)

# Run the animation-enabled simplification
req = compute_equivalent_resistance_animated(G_example, "START", "END")
print(f"Animated Result: {req:.2f} Ω")
```
Visit:[Colab](https://colab.research.google.com/drive/1S-2XQLto7CzYvL6rtMLSLUcyY9CDzIwc#scrollTo=eCMQmkc6Av15
)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-5666.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/5%20Circuits/Unknown-566.png?raw=true)

```python
# Create animation
fig = animation_frames[0]
fig.set_size_inches(6, 6)

ani = animation.ArtistAnimation(fig, [f.axes[0].images + f.axes[0].texts for f in animation_frames],
                                 interval=1500, repeat_delay=1000, blit=False)

# Display the animation
HTML(ani.to_jshtml())
```
Visit:[Colab](https://colab.research.google.com/drive/1S-2XQLto7CzYvL6rtMLSLUcyY9CDzIwc#scrollTo=eCMQmkc6Av15
)

---
## 5. Example Interpretation

This circuit:

```
START — 5Ω — A — 10Ω — END
       \________________/
              15Ω
```

Reduces to:

* Series path = 15Ω
* In parallel with 15Ω → final R\_eq = 7.5Ω

---

## 6. Applications

* Circuit simulation software
* Hardware simplification
* Engineering education
* Algorithmic analysis of networks

---

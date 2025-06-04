# Problem 2
## PART 1: Estimating π Using a Circle

---

### 1. Theoretical Foundation

We inscribe a **unit circle** inside a square. The square has a side length of 2 (from $[-1, 1]$ in both $x$ and $y$), so its **area** is:

$$
A_{\text{square}} = 2 \times 2 = 4
$$

The area of the **unit circle** (radius $r = 1$) is:

$$
A_{\text{circle}} = \pi \cdot r^2 = \pi
$$

The **ratio** of the area of the circle to the square is:

$$
\frac{A_{\text{circle}}}{A_{\text{square}}} = \frac{\pi}{4}
$$

By randomly generating points in the square, the proportion that fall inside the circle should approximate $\frac{\pi}{4}$. Hence:

$$
\pi \approx 4 \cdot \frac{\text{# points inside circle}}{\text{total # of points}}
$$

---

### 2. Simulation Code

```python
import numpy as np
import matplotlib.pyplot as plt

def estimate_pi_circle(num_points):
    x = np.random.uniform(-1, 1, num_points)
    y = np.random.uniform(-1, 1, num_points)
    inside = x**2 + y**2 <= 1
    pi_estimate = 4 * np.sum(inside) / num_points

    # Visualization
    fig, ax = plt.subplots(figsize=(6, 6))
    ax.set_aspect('equal')
    ax.set_title(f"Estimation of π = {pi_estimate:.6f} with {num_points} points")
    ax.plot(x[inside], y[inside], 'b.', markersize=1, label='Inside Circle')
    ax.plot(x[~inside], y[~inside], 'r.', markersize=1, label='Outside Circle')
    ax.add_patch(plt.Circle((0, 0), 1, color='black', fill=False))
    ax.legend()
    plt.grid(True)
    plt.show()

    return pi_estimate

# Try with different sizes
for n in [100, 1000, 10000, 100000]:
    estimate_pi_circle(n)
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/6%20Statistics/Unknown-621.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/6%20Statistics/Unknown-622.png?raw=true)

Visit: [Colab](https://colab.research.google.com/drive/12BYlXtYpTRca7vZ0PzWge0qjHnQXr0b5#scrollTo=Dc_8nvQYHBdE)

---

### 3. Convergence Analysis

```python
import matplotlib.pyplot as plt

def convergence_analysis_circle(max_points=100000, step=1000):
    estimates = []
    x_vals = list(range(step, max_points + 1, step))
    for n in x_vals:
        x = np.random.uniform(-1, 1, n)
        y = np.random.uniform(-1, 1, n)
        inside = x**2 + y**2 <= 1
        pi_approx = 4 * np.sum(inside) / n
        estimates.append(pi_approx)

    plt.figure(figsize=(10, 5))
    plt.plot(x_vals, estimates, label='Estimated π')
    plt.axhline(y=np.pi, color='r', linestyle='--', label='True π')
    plt.title("Convergence of π Estimation using Circle Method")
    plt.xlabel("Number of Points")
    plt.ylabel("Estimated π")
    plt.legend()
    plt.grid(True)
    plt.show()

convergence_analysis_circle()
```
Visit: [Colab](https://colab.research.google.com/drive/12BYlXtYpTRca7vZ0PzWge0qjHnQXr0b5#scrollTo=Dc_8nvQYHBdE)

### 4. Animation

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Parameters
n_points = 5000  # Total points
step = 100       # Points per animation frame

# Generate random points
x = np.random.uniform(-1, 1, n_points)
y = np.random.uniform(-1, 1, n_points)
r = x**2 + y**2
inside = r <= 1

# Set up the figure
fig, ax = plt.subplots(figsize=(6, 6))
ax.set_aspect('equal')
ax.set_xlim(-1, 1)
ax.set_ylim(-1, 1)
ax.set_title("Estimating π via Monte Carlo")
points_in, = ax.plot([], [], 'b.', markersize=1, label='Inside Circle')
points_out, = ax.plot([], [], 'r.', markersize=1, label='Outside Circle')
circle = plt.Circle((0, 0), 1, fill=False, color='black')
ax.add_artist(circle)
ax.legend()

# Animation update function
def update(frame):
    idx = slice(0, step * (frame + 1))
    points_in.set_data(x[idx][inside[idx]], y[idx][inside[idx]])
    points_out.set_data(x[idx][~inside[idx]], y[idx][~inside[idx]])
    current_pi = 4 * np.sum(inside[idx]) / len(x[idx])
    ax.set_title(f"Estimation of π: {current_pi:.5f} (n={len(x[idx])})")
    return points_in, points_out

frames = n_points // step
ani = FuncAnimation(fig, update, frames=frames, interval=50, blit=True)
plt.close()
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/12BYlXtYpTRca7vZ0PzWge0qjHnQXr0b5#scrollTo=Dc_8nvQYHBdE)

---

## PART 2: Estimating π Using Buffon’s Needle

---

### 1. Theoretical Foundation

In Buffon's Needle experiment, we drop a needle of length $L$ onto a plane with parallel lines spaced $d$ apart.

Let:

* $L$ = length of needle
* $d$ = distance between lines
* $L \leq d$

The **probability** $P$ that the needle crosses a line is:

$$
P = \frac{2L}{\pi d}
\Rightarrow \pi \approx \frac{2L \cdot N}{d \cdot C}
$$

Where:

* $N$ = total number of drops
* $C$ = number of crosses

---

### 2. Simulation Code

```python
def simulate_buffon_needle(N=10000, L=1.0, d=2.0):
    assert L <= d, "Needle length must be less than or equal to distance between lines"
    
    # Random positions and angles
    theta = np.random.uniform(0, np.pi / 2, N)
    y = np.random.uniform(0, d / 2, N)
    
    crosses = y <= (L / 2) * np.sin(theta)
    C = np.sum(crosses)
    pi_estimate = (2 * L * N) / (C * d) if C > 0 else np.nan

    # Plot a few needles
    num_visual = 200
    x_center = np.random.uniform(0, 10, num_visual)
    y_center = np.random.uniform(0, 10, num_visual)
    theta_visual = np.random.uniform(0, np.pi, num_visual)

    plt.figure(figsize=(10, 6))
    for i in range(num_visual):
        x0 = x_center[i] - (L / 2) * np.cos(theta_visual[i])
        y0 = y_center[i] - (L / 2) * np.sin(theta_visual[i])
        x1 = x_center[i] + (L / 2) * np.cos(theta_visual[i])
        y1 = y_center[i] + (L / 2) * np.sin(theta_visual[i])
        color = 'blue' if np.floor(y0 / d) != np.floor(y1 / d) else 'gray'
        plt.plot([x0, x1], [y0, y1], color=color)

    for i in range(12):
        plt.axhline(i * d, color='black', linestyle='--', linewidth=0.5)
    plt.title(f"Buffon's Needle Estimate of π = {pi_estimate:.6f}")
    plt.xlim(0, 10)
    plt.ylim(0, 10)
    plt.grid(True)
    plt.show()

    return pi_estimate

simulate_buffon_needle()
```
Visit: [Colab](https://colab.research.google.com/drive/12BYlXtYpTRca7vZ0PzWge0qjHnQXr0b5#scrollTo=Dc_8nvQYHBdE)

---

### 3. Convergence Analysis

```python
def convergence_analysis_buffon(N_max=10000, step=500, L=1.0, d=2.0):
    estimates = []
    x_vals = []
    for n in range(step, N_max + 1, step):
        theta = np.random.uniform(0, np.pi / 2, n)
        y = np.random.uniform(0, d / 2, n)
        crosses = y <= (L / 2) * np.sin(theta)
        C = np.sum(crosses)
        if C == 0:
            estimates.append(np.nan)
        else:
            pi_est = (2 * L * n) / (C * d)
            estimates.append(pi_est)
        x_vals.append(n)

    plt.figure(figsize=(10, 5))
    plt.plot(x_vals, estimates, label="Estimated π")
    plt.axhline(np.pi, color='red', linestyle='--', label="True π")
    plt.title("Convergence of π Estimation using Buffon's Needle")
    plt.xlabel("Number of Needle Drops")
    plt.ylabel("Estimated π")
    plt.legend()
    plt.grid(True)
    plt.show()

convergence_analysis_buffon()
```
Visit: [Colab](https://colab.research.google.com/drive/12BYlXtYpTRca7vZ0PzWge0qjHnQXr0b5#scrollTo=Dc_8nvQYHBdE)

---

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Parameters
N = 500         # total needles
step = 20       # needles per frame
L = 0.8         # length of needle
d = 1.0         # line spacing

# Generate random angles and positions
theta = np.random.uniform(0, np.pi, N)
y_center = np.random.uniform(0, d/2, N)

# Compute endpoints of needles
x0 = np.zeros(N)
y0 = y_center
x1 = L/2 * np.cos(theta)
y1 = L/2 * np.sin(theta)

# Check crossing condition
crosses = y_center <= (L/2) * np.sin(theta)

# Setup plot
fig, ax = plt.subplots(figsize=(6, 6))
ax.set_xlim(-1, 1)
ax.set_ylim(0, d)
ax.set_aspect('equal')
ax.set_title("Buffon's Needle Simulation")
ax.hlines([0, d], -1, 1, colors='k', linewidth=1)

needle_lines = []

def update(frame):
    global needle_lines
    for line in needle_lines:
        line.remove()
    needle_lines = []
    end = min(N, (frame+1)*step)
    for i in range(end):
        x_start = -x1[i]
        x_end = x1[i]
        y_start = y0[i] - y1[i]
        y_end = y0[i] + y1[i]
        color = 'r' if crosses[i] else 'b'
        needle = ax.plot([x_start, x_end], [y_start, y_end], color=color, linewidth=1)
        needle_lines.extend(needle)
    if np.sum(crosses[:end]) > 0:
        pi_est = (2 * L * end) / (d * np.sum(crosses[:end]))
        ax.set_title(f"Buffon's Needle π ≈ {pi_est:.5f} (n={end})")
    else:
        ax.set_title("Waiting for first cross...")
    return needle_lines

frames = N // step
ani = FuncAnimation(fig, update, frames=frames, interval=200, blit=False)
plt.close()
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/12BYlXtYpTRca7vZ0PzWge0qjHnQXr0b5#scrollTo=Dc_8nvQYHBdE)

---

## Comparison of Both Methods

| Method          | Accuracy (fast?)   | Simplicity   | Visualization | Variance      |
| --------------- | ------------------ | ------------ | ------------- | ------------- |
| Circle Method   | Faster convergence | Very easy    | Easy to plot  | Low to Medium |
| Buffon's Needle | Slower             | More complex | Geometric     | High          |

---

## Summary

* **Circle-based method** uses 2D geometry and is fast and simple.
* **Buffon’s Needle** is elegant and connects probability to geometry but converges more slowly.
* Both methods illustrate how randomness and computation can estimate mathematical constants.

# Problem 1
## Motivation

The acceleration due to gravity $g$ is a crucial constant in physics. A simple pendulum provides an elegant way to experimentally measure $g$, emphasizing:

* Oscillation-based measurement techniques
* Precision timing and measurement
* Uncertainty quantification

---

## Theoretical Derivation

### 1. Period of a Simple Pendulum

For small angles $\theta < 15^\circ$, the motion of a simple pendulum approximates simple harmonic motion:

$$
T = 2\pi \sqrt{\frac{L}{g}}
$$

Where:

* $T$ is the period of one oscillation
* $L$ is the length of the pendulum
* $g$ is the local gravitational acceleration

### 2. Solving for $g$

$$
g = \frac{4\pi^2 L}{T^2}
$$

---

## Experimental Procedure

### Materials

* String (1–1.5 meters)
* Weight (keychain, coin bag, etc.)
* Stopwatch / Smartphone
* Ruler or tape measure

### Data Collection

1. Measure pendulum length $L$ with resolution $\Delta L$, uncertainty = $\frac{\Delta L}{2}$
2. Perform 10 measurements of time for 10 oscillations each: $t_1, t_2, ..., t_{10}$
3. Calculate:

   * Mean $\bar{t}$
   * Standard deviation $\sigma$
   * Period $T = \frac{\bar{t}}{10}$

---

## Uncertainty Analysis

### Uncertainty in Time:

$$
\sigma_T = \frac{\sigma_{\bar{t}}}{10} = \frac{\sigma}{10\sqrt{n}}
$$

### Uncertainty in $g$:

$$
\frac{\Delta g}{g} = \sqrt{ \left( \frac{\Delta L}{L} \right)^2 + \left( 2 \cdot \frac{\sigma_T}{T} \right)^2 }
$$

---

## Python Code 
```python
import numpy as np
import matplotlib.pyplot as plt

# Example time measurements (10 trials)
t_measurements = np.array([20.1, 20.3, 20.2, 20.0, 20.4, 20.1, 20.2, 20.3, 20.2, 20.1])

# Calculate stats
mean_t10 = np.mean(t_measurements)
std_t10 = np.std(t_measurements, ddof=1)

# Plot
plt.figure(figsize=(8, 5))
plt.errorbar(range(1, 11), t_measurements, yerr=0.1, fmt='o', label='Measured Times')
plt.axhline(mean_t10, color='red', linestyle='--', label=f'Mean: {mean_t10:.2f}s')
plt.fill_between(range(1, 11), mean_t10 - std_t10, mean_t10 + std_t10, color='red', alpha=0.2, label='±1 SD')

plt.title("Measured Time for 10 Oscillations (with Error Bars)")
plt.xlabel("Trial Number")
plt.ylabel("Time (s)")
plt.legend()
plt.grid(True)
plt.show()
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/7%20Measurements/Unknown-71.png?raw=true)

Visit: [Colab](https://colab.research.google.com/drive/1OTRbTTaFb3A38Wvb5JEP1-nqszRAT3YV#scrollTo=UGd9jv-Xk9O_)
---
```python
# Pendulum lengths from 0.2 m to 2.0 m
L_vals = np.linspace(0.2, 2.0, 100)
g = 9.81  # m/s²
T_vals = 2 * np.pi * np.sqrt(L_vals / g)

# Plot
plt.figure(figsize=(8, 5))
plt.plot(L_vals, T_vals, color='blue')
plt.title("Pendulum Period vs. Length")
plt.xlabel("Length (m)")
plt.ylabel("Period (s)")
plt.grid(True)
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1OTRbTTaFb3A38Wvb5JEP1-nqszRAT3YV#scrollTo=UGd9jv-Xk9O_)

---
```python
# Simulate periods with small timing errors
L = 1.0  # fixed length
T_sim = np.linspace(1.8, 2.2, 100)
g_sim = 4 * np.pi**2 * L / T_sim**2

# Plot
plt.figure(figsize=(8, 5))
plt.plot(T_sim, g_sim, label='Calculated g')
plt.axhline(9.81, color='green', linestyle='--', label='True g')
plt.title("Effect of Period Error on Calculated g")
plt.xlabel("Period (s)")
plt.ylabel("Calculated g (m/s²)")
plt.legend()
plt.grid(True)
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1OTRbTTaFb3A38Wvb5JEP1-nqszRAT3YV#scrollTo=UGd9jv-Xk9O_)

---

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Pendulum parameters
L = 1.0  # length in meters
theta0 = 0.2  # initial angle in radians (small angle)

# Time array for animation (duration = 10 seconds at 30 FPS)
t = np.linspace(0, 10, 300)
omega = np.sqrt(9.81 / L)
theta = theta0 * np.cos(omega * t)

# Convert to x, y positions
x = L * np.sin(theta)
y = -L * np.cos(theta)

# Set up the figure
fig, ax = plt.subplots(figsize=(5, 5))
ax.set_xlim(-L - 0.2, L + 0.2)
ax.set_ylim(-L - 0.2, 0.2)
ax.set_aspect('equal')
line, = ax.plot([], [], 'o-', lw=3)
pivot = [0, 0]

# Initialization
def init():
    line.set_data([], [])
    return line,

# Animation function
def update(i):
    line.set_data([pivot[0], x[i]], [pivot[1], y[i]])
    return line,

# Create animation
ani = FuncAnimation(fig, update, frames=len(t), init_func=init, blit=True)
plt.close()  # Prevent duplicate static plot

# Display in Colab
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1OTRbTTaFb3A38Wvb5JEP1-nqszRAT3YV#scrollTo=UGd9jv-Xk9O_)
---

## Markdown Table Template

```markdown
### Raw Data

| Trial | Time for 10 Oscillations (s) |
|-------|-------------------------------|
| 1     | 20.1                          |
| 2     | 20.3                          |
| 3     | 20.2                          |
| 4     | 20.0                          |
| 5     | 20.4                          |
| 6     | 20.1                          |
| 7     | 20.2                          |
| 8     | 20.3                          |
| 9     | 20.2                          |
| 10    | 20.1                          |

### Summary

- Mean time for 10 oscillations: \( \bar{t} = 20.19 \pm 0.12 \) s
- Period \( T = 2.019 \pm 0.012 \) s
- Pendulum length \( L = 1.00 \pm 0.005 \) m
- Calculated \( g = 9.66 \pm 0.11 \, \text{m/s}^2 \)
```

---

## Discussion and Analysis

### 1. Comparison to Standard Value

* Measured $g$: \~9.66 m/s²
* Accepted $g$: 9.81 m/s²
* Difference may stem from systematic timing error or measurement of $L$

### 2. Sources of Uncertainty

* **Measurement resolution**: Ruler error affects $L$
* **Human reaction time**: Affects timing measurements
* **Angle of release**: Larger angles introduce non-linear effects

### 3. Experimental Limitations

* Assumes no air resistance and massless string
* Assumes rigid pivot and no energy loss

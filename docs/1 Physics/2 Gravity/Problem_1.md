# Problem 1
# Orbital Period and Orbital Radius

---

## Motivation

The elegant relationship between the **square of the orbital period (T)** and the **cube of the orbital radius (r)** for circular orbits, known as **Kepler’s Third Law**, reveals deep insights into gravitational interactions. This project aims to derive this law starting from Newtonian mechanics and differential equations, explore its real-world implications, and validate it through simulations and animations.

---

## Theoretical Analysis via Differential Equations

### Newton’s Law of Universal Gravitation

Gravitational force between two bodies of mass $M$ (e.g., Earth) and $m$ (e.g., satellite) is:

$$F = \frac{G M m}{r^2}$$

### Circular Motion Requirement

For an object to stay in circular orbit, this gravitational force must equal the centripetal force:

$$\frac{G M m}{r^2} = m \frac{v^2}{r}
\Rightarrow v^2 = \frac{G M}{r}$$

### Derivation Using Differential Equations

The radial acceleration for a mass $m$ in circular motion is:

$$\vec{a} = \frac{d^2 \vec{r}}{dt^2} = -\frac{G M}{r^2} \hat{r}$$

If the motion is constrained to a circular orbit, the position vector is:

$$\vec{r}(t) = r (\cos(\omega t)\hat{i} + \sin(\omega t)\hat{j})$$

Differentiating twice gives:

$$\frac{d^2 \vec{r}}{dt^2} = -r \omega^2 (\cos(\omega t)\hat{i} + \sin(\omega t)\hat{j}) = -\omega^2 \vec{r}$$

From Newton’s second law:

$$m \frac{d^2 \vec{r}}{dt^2} = -\frac{G M m}{r^2} \hat{r}
\Rightarrow -m \omega^2 \vec{r} = -\frac{G M m}{r^2} \hat{r}$$

Solving:

$$\omega^2 = \frac{G M}{r^3}
\Rightarrow T^2 = \left( \frac{2\pi}{\omega} \right)^2 = \frac{4\pi^2 r^3}{G M}$$

**This is Kepler’s Third Law** for circular orbits.

---

## Real-World Examples

### Moon's Orbit Around Earth

- Radius: $r \approx 3.84 \times 10^8$ m  
- Period: $T \approx 27.3$ days

Verify Kepler’s Third Law:

$T^2 \propto r^3$

### Planets in the Solar System

| Planet | Orbital Radius (AU) | Period (years) |
|--------|---------------------|----------------|
| Mercury | 0.39 | 0.24 |
| Earth   | 1.00 | 1.00 |
| Jupiter | 5.20 | 11.86 |

$$\frac{T^2}{r^3} \approx \text{constant}$$

---

```python
import matplotlib.pyplot as plt
import numpy as np

# Sample data (replace with your actual data)
radius = np.array([0.39, 0.72, 1.00, 1.52, 5.20, 9.54])  # AU
period = np.array([0.24, 0.62, 1.00, 1.88, 11.86, 29.46]) # years

# Calculate T^2 and R^3
T_squared = period**2
R_cubed = radius**3

# Create the plot
plt.figure(figsize=(8, 6))
plt.scatter(R_cubed, T_squared, label='Data Points')  # Create the scatter plot

# Add a best fit line (linear regression)
coefficients = np.polyfit(R_cubed, T_squared, 1)
polynomial = np.poly1d(coefficients)
R_cubed_fit = np.linspace(min(R_cubed), max(R_cubed), 100)
plt.plot(R_cubed_fit, polynomial(R_cubed_fit), color='red', label='Best Fit Line')

# Add labels and title
plt.xlabel(r'$R^3$ (AU$^3$)')
plt.ylabel(r'$T^2$ (years$^2$)')
plt.title('Orbital Period vs. Radius (Kepler\'s Third Law)')
plt.grid(True) # Add grid for better readability
plt.legend()


# Calculate and print the slope
slope = coefficients[0]
print(f"Slope of the best-fit line: {slope:.2f}")

# Add text annotation showing the slope on the plot.
plt.annotate(f"Slope: {slope:.2f}", xy=(0.65 * max(R_cubed), 0.85 * max(T_squared)), fontsize=12)


# Show the plot
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1Gzs6NYgJDfira_n9CpCjV6JsMjYsQ-4I#scrollTo=pRkikqHFYLIU)
![Example Imnage](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/2%20Gravity/Unknown.png?raw=true)
---

## Verifying Kepler’s Law

Let’s plot $T^2$ vs $r^3$ to see the linear relationship:

```python
import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11  # gravitational constant
M = 5.972e24     # mass of Earth

# Ensure radii and periods have the same length
num_points = 5  # Or any other desired number of points
radii = np.linspace(1e7, 5e8, num_points) # changed the number of points to 5
periods = 2 * np.pi * np.sqrt(radii**3 / (G * M))

plt.figure()
plt.plot(radii, periods, label='T vs r')
plt.xlabel('Orbital Radius (m)')
plt.ylabel('Orbital Period (s)')
plt.title('Orbital Period vs Radius')
plt.grid(True)
plt.legend()
plt.show()

T_squared = periods**2
r_cubed = radii**3

plt.figure()
plt.plot(r_cubed, T_squared, label='T² vs r³')
plt.xlabel('r³ (m³)')
plt.ylabel('T² (s²)')
plt.title("Verification of Kepler's Third Law")
plt.grid(True)
plt.legend()
plt.show()
```
Visit: [Colab]((https://colab.research.google.com/drive/1Gzs6NYgJDfira_n9CpCjV6JsMjYsQ-4I#scrollTo=pRkikqHFYLIU))

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/2%20Gravity/Unknown-2.png?raw=true)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/2%20Gravity/Unknown-5.png?raw=true)

---

## Animation of a Circular Orbit

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Constants
G = 6.67430e-11         # Gravitational constant (m^3 kg^-1 s^-2)
M = 5.972e24            # Mass of the Earth (kg)
r = 3.84e8              # Radius of the orbit (Moon-Earth distance, m)
omega = np.sqrt(G * M / r**3)  # Angular velocity (rad/s)

# Time settings
T = 2 * np.pi / omega   # Orbital period (s)
t_vals = np.linspace(0, T, 500)  # Time points

# Orbital coordinates
x_vals = r * np.cos(omega * t_vals)
y_vals = r * np.sin(omega * t_vals)

# Plot setup
fig, ax = plt.subplots(figsize=(6, 6))
ax.set_xlim(-1.2*r, 1.2*r)
ax.set_ylim(-1.2*r, 1.2*r)
ax.set_aspect('equal')
ax.set_title('Circular Orbit')
ax.set_xlabel('x (m)')
ax.set_ylabel('y (m)')

# Static Earth
ax.plot(0, 0, 'yo', markersize=12, label='Earth')

# Satellite dot and path
satellite, = ax.plot([], [], 'ro', label='Satellite')
path, = ax.plot([], [], 'r--', alpha=0.5)

def init():
    satellite.set_data([], [])
    path.set_data([], [])
    return satellite, path

def update(frame):
    # Ensure x and y are lists or 1-dimensional arrays
    x = [x_vals[frame]]  # or x = np.array([x_vals[frame]])
    y = [y_vals[frame]]  # or y = np.array([y_vals[frame]])
    
    satellite.set_data(x, y)
    path.set_data(x_vals[:frame+1], y_vals[:frame+1])
    return satellite, path

ani = FuncAnimation(fig, update, frames=len(t_vals),
                    init_func=init, blit=True, interval=20)

# Display in Jupyter/Colab
HTML(ani.to_jshtml())
```
Visit:[Colab](https://colab.research.google.com/drive/1Gzs6NYgJDfira_n9CpCjV6JsMjYsQ-4I#scrollTo=wVy_EQuObf0j)
---

## Extensions to Elliptical Orbits

Kepler’s Third Law generalizes to elliptical orbits:

$$T^2 \propto a^3$$

Where $a$ is the **semi-major axis**. This law still holds but requires integration over elliptical motion, using:

- **Vis-viva equation:**
  $$v^2 = G M \left( \frac{2}{r} - \frac{1}{a} \right)$$

- **Orbital energy methods**
- **Numerical integration** for eccentric orbits

---

## Astronomical Implications

- Calculate planetary masses
- Estimate satellite altitudes
- Study exoplanet orbits
- Design satellite constellations (e.g., Starlink)
- Measure black hole masses via stellar motion

---

## Summary

| Aspect | Description |
|--------|-------------|
| **Law** | $T^2 \propto r^3$ for circular orbits |
| **Derived From** | Newton’s Law + differential equations |
| **Verification** | Simulations, plots of $T^2$ vs $r^3$ |
| **Used For** | Planetary systems, satellites, exoplanets |
| **Animation** | Visualized stable orbit with Python |

---

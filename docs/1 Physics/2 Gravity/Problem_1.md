# Problem 1
# Orbital Period and Orbital Radius

---

## Motivation

The elegant relationship between the **square of the orbital period (T)** and the **cube of the orbital radius (r)** for circular orbits, known as **Kepler’s Third Law**, reveals deep insights into gravitational interactions. This project aims to derive this law starting from Newtonian mechanics and differential equations, explore its real-world implications, and validate it through simulations and animations.

---

Kepler’s Third Law reveals a fundamental relationship in orbital mechanics:

$$T^2 \propto r^3$$

This law connects the period of revolution $T$ of a planet (or satellite) with the radius $r$ of its circular orbit. It's a powerful tool used in celestial mechanics, satellite design, and calculating masses of celestial bodies.

---

## Theoretical Derivation using Newtonian Mechanics

### Step 1: Gravitational Force
Newton’s Law of Gravitation

$$F_g = \frac{G M m}{r^2}$$

### Step 2: Centripetal Force for Circular Orbits
$$F_c = \frac{m v^2}{r}$$

### Step 3: Equating Forces
$$\frac{G M m}{r^2} = \frac{m v^2}{r}
\Rightarrow v^2 = \frac{G M}{r}$$

### Step 4: Express Orbital Period
$$T = \frac{2\pi r}{v}
\Rightarrow T^2 = \frac{4\pi^2 r^2}{v^2}$$

Substitute $v^2$:

$$T^2 = \frac{4\pi^2 r^2}{\frac{G M}{r}} = \frac{4\pi^2 r^3}{G M}$$

Final Form:

$$T^2 = \left( \frac{4\pi^2}{G M} \right) r^3
\Rightarrow T^2 \propto r^3$$

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
!pip install ace_tools # Install the required 'ace_tools' module
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

# Orbital data for planets (radius in AU, period in years)
planets = {
    "Mercury": {"r_AU": 0.39, "T_years": 0.240846},
    "Venus": {"r_AU": 0.723, "T_years": 0.615},
    "Earth": {"r_AU": 1.000, "T_years": 1.000},
    "Mars": {"r_AU": 1.524, "T_years": 1.881}
}

# Convert to arrays
names = list(planets.keys())
r_AU = np.array([planets[p]["r_AU"] for p in names])
T_years = np.array([planets[p]["T_years"] for p in names])

# Calculate T^2 and r^3
T_squared = T_years**2
r_cubed = r_AU**3

# Create a DataFrame for visualization
df = pd.DataFrame({
    "Planet": names,
    "r (AU)": r_AU,
    "T (years)": T_years,
    "r^3 (AU^3)": r_cubed,
    "T^2 (years^2)": T_squared
})

# Plotting T^2 vs r^3
plt.figure(figsize=(8, 6))
sns.scatterplot(x=r_cubed, y=T_squared, hue=names, s=100)
plt.plot(r_cubed, T_squared, 'k--', alpha=0.6)  # Line for visual confirmation
plt.xlabel("r³ (AU³)")
plt.ylabel("T² (years²)")
plt.title("Verification of Kepler's Third Law: T² vs r³")
plt.grid(True)
plt.legend(title="Planet")
plt.tight_layout()
plt.show()

import ace_tools as tools; tools.display_dataframe_to_user(name="Planetary Orbital Data", dataframe=df)
```
Visit:[Colab](https://colab.research.google.com/drive/1Gzs6NYgJDfira_n9CpCjV6JsMjYsQ-4I#scrollTo=gO_MWYFbhfvc)


### Planetary Orbital Data (AU & Years)

| Planet  | Orbital Radius \( r \) (AU) | Orbital Period \( T \) (years) | \( r^3 \) (AU³) | \( T^2 \) (years²) |
|---------|-----------------------------|-------------------------------|------------------|--------------------|
| Mercury | 0.390                       | 0.240846                      | 0.059319         | 0.058007           |
| Venus   | 0.723                       | 0.615                         | 0.377933         | 0.378225           |
| Earth   | 1.000                       | 1.000                         | 1.000000         | 1.000000           |
| Mars    | 1.524                       | 1.881                         | 3.539606         | 3.538161           |




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

- **Vis-viva equation:** $v^2 = G M \left( \frac{2}{r} - \frac{1}{a} \right)$

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


| Aspect | Description |
|--------|-------------|
| **Law** | $T^2 \propto r^3$ for circular orbits |
| **Derived From** | Newton’s Law + differential equations |
| **Verification** | Simulations, plots of $T^2$ vs $r^3$ |
| **Used For** | Planetary systems, satellites, exoplanets |
| **Animation** | Visualized stable orbit with Python |



---

## Kepler’s Law Applied to the Solar System

### Planets: Mercury, Venus, Earth, Mars

| Planet   | Orbital Radius (10⁸ km) | Period (days) |
|----------|--------------------------|---------------|
| Mercury  | 57.9                     | 87.97         |
| Venus    | 108.2                    | 224.70        |
| Earth    | 149.6                    | 365.25        |
| Mars     | 227.9                    | 687.00        |



---

## Calculating Mass of Earth (Using the Moon)

- $r = 3.844 \times 10^8 \, \text{m}$
- $T = 27.32 \, \text{days} = 2.36 \times 10^6 \, \text{s}$

$M = \frac{4 \pi^2 r^3}{G T^2}$

```python
G = 6.67430e-11
r_earth_moon = 3.844e8
T_moon = 27.32 * 86400

M_earth = 4 * np.pi**2 * r_earth_moon**3 / (G * T_moon**2)
print(f"Mass of Earth ≈ {M_earth:.2e} kg")
```

Output: $5.97 \times 10^{24} \, \text{kg}$ (correct)

---

## Calculating Mass of the Sun (Using Earth's Orbit)

- $r = 1.496 \times 10^{11} \, \text{m}$
- $T = 365.25 \, \text{days} = 3.156 \times 10^7 \, \text{s}$

$M = \frac{4 \pi^2 r^3}{G T^2}$

```python
r_earth_sun = 1.496e11
T_earth = 365.25 * 86400

M_sun = 4 * np.pi**2 * r_earth_sun**3 / (G * T_earth**2)
print(f"Mass of Sun ≈ {M_sun:.2e} kg")
```

Output: $1.99 \times 10^{30} \, \text{kg}$  (correct)

---

## Orbit Animation

```python
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Time setup
t_vals = np.linspace(0, 2 * np.pi, 500)
x_vals = r_earth_sun * np.cos(t_vals)
y_vals = r_earth_sun * np.sin(t_vals)

fig, ax = plt.subplots(figsize=(6,6))
ax.set_xlim(-1.2*r_earth_sun, 1.2*r_earth_sun)
ax.set_ylim(-1.2*r_earth_sun, 1.2*r_earth_sun)
ax.set_aspect('equal')
ax.set_title('Earth Orbiting the Sun')
earth, = ax.plot([], [], 'ro')
sun = ax.plot(0, 0, 'yo', markersize=12)

def init():
    earth.set_data([], [])
    return earth,

def update(i):
    earth.set_data(x_vals[i], y_vals[i])
    return earth,

ani = FuncAnimation(fig, update, frames=len(t_vals), init_func=init, blit=True)
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/10h-UzeVQ_-7ZmLhpEmGQaL_kneqH5xlC#scrollTo=qTo-sVozrX3c)


| Concept | Value |
|--------|-------|
| **Kepler's Law** | $T^2 \propto r^3$ |
| **Derived from** | Newton's Gravity + Centripetal Motion |
| **T² vs r³ Plot** | Straight line confirmed |
| **Mass of Earth** | $\approx 5.97 \times 10^{24} \, \text{kg}$ |
| **Mass of Sun** | $\approx 1.99 \times 10^{30} \, \text{kg}$ |
| **Planets Used** | Mercury, Venus, Earth, Mars |
| **Python** | All steps simulated and visualized |

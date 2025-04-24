# Problem 2
# **Escape Velocities and Cosmic Velocities**

## **1. Introduction**

In celestial mechanics and spaceflight engineering, understanding the conditions required to move between gravitational domains is essential. From launching satellites into Earth orbit to sending interstellar probes like *Voyager*, space missions must overcome gravitational boundaries defined by the **cosmic velocities**.

These velocities represent thresholds in orbital mechanics, each marking a different kind of escape:

- **First Cosmic Velocity**: Required to enter stable circular orbit.
- **Second Cosmic Velocity**: Required to completely escape a planet’s gravity.
- **Third Cosmic Velocity**: Required to escape both a planet’s and its star's gravity, reaching interstellar space.

---

## **2. Theoretical Foundation**

### **2.1 First Cosmic Velocity – Orbital Velocity**

**Concept**: The speed to stay in low circular orbit, counteracting gravity with centripetal acceleration.

**Derivation**:
$$F_{\text{gravity}} = \frac{GMm}{r^2}, \quad F_{\text{centripetal}} = \frac{mv^2}{r}$$

Equating the two:
$$\frac{GMm}{r^2} = \frac{mv_1^2}{r} \Rightarrow v_1 = \sqrt{\frac{GM}{r}}$$

- *Intuition*: Go too slow, you fall back. Go too fast, you escape.

---

### **2.2 Second Cosmic Velocity – Escape Velocity**

**Concept**: Minimum speed to reach infinity with zero kinetic energy remaining.

**Energy Approach**:
$$E = K + U = \frac{1}{2}mv^2 - \frac{GMm}{r} \geq 0 \Rightarrow v_2 = \sqrt{\frac{2GM}{r}}$$

- *Notice*: $v_2 = \sqrt{2} \cdot v_1$. It takes ~41% more speed than orbiting to escape.

---

### **2.3 Third Cosmic Velocity – Solar System Escape**

**Concept**: Speed to escape both the planet **and** the Sun from the planet’s orbit.

$$v_3 = \sqrt{v_2^2 + v_{\text{orbit,Sun}}^2} = \sqrt{2 \frac{GM_p}{r} + \frac{GM_\odot}{R_{\text{orbit}}}}$$

Where:
- $M_\odot$: mass of the Sun.
- $R_{\text{orbit}}$: orbital radius of the planet.

- *Key Point*: You’re fighting **two gravity wells**: the planet and the star.

---

## **3. Simulation & Code**

### **3.1 Python Code: Velocity Calculations & Visualization**

```python
import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11
M_sun = 1.989e30

# [Mass (kg), Radius (m), Orbital Radius from Sun (m)]
bodies = {
    "Earth": [5.972e24, 6.371e6, 1.496e11],
    "Moon": [7.342e22, 1.737e6, 1.5e11 + 3.844e8],
    "Mars": [6.417e23, 3.389e6, 2.279e11],
    "Jupiter": [1.898e27, 6.9911e7, 7.785e11]
}

def v1(M, R): return np.sqrt(G * M / R)
def v2(M, R): return np.sqrt(2 * G * M / R)
def v3(M, R, R_orbit): return np.sqrt(v2(M, R)**2 + (G * M_sun / R_orbit))

v1s, v2s, v3s, names = [], [], [], []

for name, (M, R, R_orbit) in bodies.items():
    names.append(name)
    v1s.append(v1(M, R))
    v2s.append(v2(M, R))
    v3s.append(v3(M, R, R_orbit))

# Plot
x = np.arange(len(names))
width = 0.25

plt.figure(figsize=(10, 6))
plt.bar(x - width, np.array(v1s)/1000, width, label='1st Cosmic Velocity')
plt.bar(x, np.array(v2s)/1000, width, label='2nd Cosmic Velocity')
plt.bar(x + width, np.array(v3s)/1000, width, label='3rd Cosmic Velocity')

plt.xticks(x, names)
plt.ylabel("Velocity (km/s)")
plt.title("Cosmic Velocities of Celestial Bodies")
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1ZKQKWnH0bVhEBpNgUawo7nmMMx6MW80Y#scrollTo=ob5gb7chwMNJ)
---

### **3.2 Numerical Results Table**

| Body     | $v_1$ (km/s) | $v_2$ (km/s) | $v_3$ (km/s) | Mass Ratio (to Earth) |
|----------|----------------|----------------|----------------|------------------------|
| Earth    | 7.91           | 11.19          | ~42.1          | 1.00                   |
| Moon     | 1.68           | 2.38           | ~41.4          | 0.012                  |
| Mars     | 3.56           | 5.03           | ~40.3          | 0.107                  |
| Jupiter  | 42.1           | 59.5           | ~75.4          | 317.8                  |

---
### 3.3 Escape Velocity vs. Planetary Radius and Mass

```python
import numpy as np
import matplotlib.pyplot as plt

# Gravitational constant
G = 6.67430e-11

# Ranges of radius and mass
radii = np.linspace(1e6, 1e8, 100)  # from 1,000 km to 100,000 km
masses = [1e22, 1e24, 1e26, 1e28]  # various planet masses

plt.figure(figsize=(10, 6))
for M in masses:
    v_esc = np.sqrt(2 * G * M / radii)
    plt.plot(radii / 1e6, v_esc / 1e3, label=f'M = {M:.0e} kg')

plt.xlabel('Radius (10⁶ m)')
plt.ylabel('Escape Velocity (km/s)')
plt.title('Escape Velocity vs. Radius for Different Masses')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
``` 
Visit: [Colab](https://colab.research.google.com/drive/1ZKQKWnH0bVhEBpNgUawo7nmMMx6MW80Y#scrollTo=ob5gb7chwMNJ)
---

### 3.4 Cosmic Velocity Ratios (v2/v1 and v3/v2)
```python
# Calculate ratios
v1_arr = np.array(v1s)
v2_arr = np.array(v2s)
v3_arr = np.array(v3s)

ratios_21 = v2_arr / v1_arr
ratios_32 = v3_arr / v2_arr

x = np.arange(len(names))
width = 0.35

plt.figure(figsize=(10, 5))
plt.bar(x - width/2, ratios_21, width, label='v₂ / v₁')
plt.bar(x + width/2, ratios_32, width, label='v₃ / v₂')

plt.xticks(x, names)
plt.ylabel("Velocity Ratio")
plt.title("Cosmic Velocity Ratios Across Celestial Bodies")
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1ZKQKWnH0bVhEBpNgUawo7nmMMx6MW80Y#scrollTo=ob5gb7chwMNJ)
---

### 3.5 3D-Like Plot of Velocities vs Mass and Radius

```python
from mpl_toolkits.mplot3d import Axes3D

# Meshgrid of radii and masses
r_vals = np.linspace(1e6, 1e8, 100)
m_vals = np.linspace(1e22, 1e28, 100)
R, M = np.meshgrid(r_vals, m_vals)

V_escape = np.sqrt(2 * G * M / R)

fig = plt.figure(figsize=(12, 8))
ax = fig.add_subplot(111, projection='3d')
ax.plot_surface(R/1e6, M/1e24, V_escape/1e3, cmap='viridis')

ax.set_xlabel('Radius (10⁶ m)')
ax.set_ylabel('Mass (10²⁴ kg)')
ax.set_zlabel('Escape Velocity (km/s)')
ax.set_title('Escape Velocity vs. Mass and Radius')
plt.tight_layout()
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1ZKQKWnH0bVhEBpNgUawo7nmMMx6MW80Y#scrollTo=ob5gb7chwMNJ)
---

### 3.6 Animated Escape Velocity vs. Radius for Increasing Mass



```python
# First, import animation tools
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Constants
G = 6.67430e-11
radii = np.linspace(1e6, 1e8, 300)  # Radii from 1000 km to 100,000 km

# Mass range (frame-wise)
mass_vals = np.linspace(1e22, 1e28, 100)  # From small asteroid to gas giant
```

```python
fig, ax = plt.subplots(figsize=(8, 6))
line, = ax.plot([], [], lw=2)
text = ax.text(0.05, 0.9, '', transform=ax.transAxes)

ax.set_xlim(radii[0]/1e6, radii[-1]/1e6)
ax.set_ylim(0, 100)
ax.set_xlabel('Radius (10⁶ m)')
ax.set_ylabel('Escape Velocity (km/s)')
ax.set_title('Escape Velocity vs Radius for Varying Mass')
ax.grid()

def init():
    line.set_data([], [])
    text.set_text('')
    return line, text

def update(frame):
    M = mass_vals[frame]
    v_esc = np.sqrt(2 * G * M / radii) / 1000  # Convert to km/s
    line.set_data(radii / 1e6, v_esc)
    text.set_text(f'Mass = {M:.1e} kg')
    return line, text

ani = FuncAnimation(fig, update, frames=len(mass_vals), init_func=init, blit=True)
plt.close()
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1ZKQKWnH0bVhEBpNgUawo7nmMMx6MW80Y#scrollTo=ob5gb7chwMNJ)
---

## **4. Engineering and Mission Design**

### Use Cases by Velocity

| Cosmic Velocity | Use Case |
|------------------|------------------------------|
| 1st              | Satellite launch (LEO, MEO)  |
| 2nd              | Planetary missions (Mars, Moon) |
| 3rd              | Interstellar travel (Voyager, Pioneer) |

### Design Considerations

- **Fuel Efficiency**: Most fuel is spent reaching low orbit. Timing and gravity assists help achieve 2nd/3rd velocities.
- **Gravity Assists**: Used by *Voyager*, *Cassini*, and *New Horizons* to incrementally achieve solar system escape.

---

## **5. Future Exploration and Cosmic Challenges**

- **Voyager 1** is the only human object beyond the heliopause (~17 km/s).
- **Interstellar Missions** like *Breakthrough Starshot* aim to reach 0.1c using laser propulsion.
- **Cosmic Horizons**: Interstellar escape velocities will eventually require non-chemical propulsion—solar sails, nuclear, or antimatter-based tech.

---

## **6. Conclusion**

Cosmic velocities define the very boundary between planetary, interplanetary, and interstellar travel. These are not abstract ideas but the core calculations used to plan every space mission.

From basic orbits to escaping the solar system, this knowledge:

- Guides launch protocols.
- Optimizes fuel usage.
- Enables human dreams of interstellar travel.

Space exploration begins not with a rocket launch, but with understanding these fundamental speeds.

# Problem 3
## **Trajectories of a Freely Released Payload Near Earth**

### **1. Introduction**

This project analyzes the trajectories of a freely released payload near Earth, focusing on gravitational forces and initial conditions (velocity, altitude). The goal is to simulate and visualize possible trajectories (elliptical, parabolic, hyperbolic) using Python in Google Colab.

The main challenge is to understand how different initial velocities (ranging from 5 km/s to 13 km/s) influence whether the payload stays in orbit, reenters, or escapes Earth's gravity.

---

## **2. Theoretical Background**

### **2.1 Governing Equations**

The gravitational force between Earth and the payload is given by:

$$
F = \frac{GMm}{r^2}
$$

Where:

* $G$ = Gravitational constant (6.67430 × 10⁻¹¹ m³ kg⁻¹ s⁻²)
* $M$ = Mass of the Earth (5.972 × 10²⁴ kg)
* $m$ = Mass of the payload
* $r$ = Distance from Earth's center to the payload

#### **2.2 Equations of Motion:**

The acceleration acting on the payload is:

$$
a = \frac{GM}{r^2}
$$

The second-order differential equation governing the motion is:

$$
\frac{d^2 \vec{r}}{dt^2} = -\frac{GM}{r^2} \hat{r}
$$

---
## **3. Simulation Code**

```python
# Import necessary libraries
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Gravitational constant and Earth parameters
G = 6.67430e-11  # m^3 kg^-1 s^-2
M = 5.972e24     # kg
R_earth = 6371e3 # m

# Define the equations of motion
def equations(t, y):
    r, vr, theta, vtheta = y
    ar = -G * M / r**2 + r * vtheta**2
    atheta = -2 * vr * vtheta / r
    return [vr, ar, vtheta, atheta]

# Initial conditions
altitude = 800e3  # 800 km above Earth's surface
initial_velocities = [5000, 5500, 6000, 6500, 7000, 7500, 8000, 8500, 9000, 9500, 10000, 10500, 11000, 11500, 12000, 12500, 13000]

# Time span for simulation
t_span = (0, 10000)

# Plot the Earth as a circle
plt.figure(figsize=(8, 8))
earth = plt.Circle((0, 0), R_earth, color='blue', alpha=0.3, label='Earth')
plt.gca().add_artist(earth)

# Loop over different initial velocities
for vtheta0 in initial_velocities:
    # Initial conditions for each velocity
    y0 = [R_earth + altitude, 0, 0, vtheta0]
    
    # Solve the equations
    solution = solve_ivp(equations, t_span, y0, method='RK45', max_step=1)
    
    # Convert polar to Cartesian coordinates
    x = solution.y[0] * np.cos(solution.y[2])
    y = solution.y[0] * np.sin(solution.y[2])
    
    # Plot the trajectory
    plt.plot(x, y, label=f'v = {vtheta0 / 1000:.1f} km/s')

# Final plot adjustments
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Trajectories of Payload Near Earth')
plt.legend()
plt.axis('equal')
plt.grid(True)
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1tmNx00N0d6ZO2M9a7sIeov0q_ArNJI7H#scrollTo=D2oE4rHnG28i)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/2%20Gravity/Unknown-17.png?raw=true)

---


## **4. Animation Code**

```python
import matplotlib.animation as animation
from IPython.display import HTML

fig, ax = plt.subplots(figsize=(8, 8))
ax.set_xlim(-2e7, 2e7)
ax.set_ylim(-2e7, 2e7)
earth = plt.Circle((0, 0), R_earth, color='blue', alpha=0.3)
ax.add_artist(earth)
line, = ax.plot([], [], 'r-', label='Trajectory')

def init():
    line.set_data([], [])
    return line,

def update(frame):
    vtheta0 = initial_velocities[frame % len(initial_velocities)]
    y0 = [R_earth + altitude, 0, 0, vtheta0]
    solution = solve_ivp(equations, t_span, y0, method='RK45', max_step=1)
    x = solution.y[0] * np.cos(solution.y[2])
    y = solution.y[0] * np.sin(solution.y[2])
    line.set_data(x, y)
    ax.set_title(f'Payload Trajectory (v = {vtheta0 / 1000:.1f} km/s)')
    return line,

ani = animation.FuncAnimation(fig, update, frames=len(initial_velocities), init_func=init, repeat=True, blit=True)
plt.legend()
plt.close()
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1tmNx00N0d6ZO2M9a7sIeov0q_ArNJI7H#scrollTo=D2oE4rHnG28i)

```python
# Import necessary libraries
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Gravitational constant and Earth parameters
G = 6.67430e-11  # m^3 kg^-1 s^-2
M = 5.972e24     # kg
R_earth = 6371e3 # m

# Differential equations for the motion
def equations(t, y):
    r, vr, theta, vtheta = y
    ar = -G * M / r**2 + r * vtheta**2
    atheta = -2 * vr * vtheta / r
    return [vr, ar, vtheta, atheta]

# Initial conditions
altitude = 800e3  # 800 km above Earth's surface
initial_position = R_earth + altitude
initial_velocities = np.arange(5000, 13001, 500)  # Velocities from 5 km/s to 13 km/s

# Time span for simulation
t_span = (0, 20000)  # Extended time for larger trajectories

# Plot setup
plt.figure(figsize=(8, 8))

# Plot the Earth as a filled circle
earth = plt.Circle((0, 0), R_earth, color='blue', alpha=0.5, label='Earth')
plt.gca().add_artist(earth)

# Plot the Earth's center
plt.plot(0, 0, 'ko', label='Center of Earth')

# Generate trajectories for each initial velocity
for i, vtheta0 in enumerate(initial_velocities):
    # Initial conditions for each velocity
    y0 = [initial_position, 0, np.pi/2, vtheta0]

    # Solve the differential equations
    solution = solve_ivp(equations, t_span, y0, method='RK45', max_step=10)

    # Convert polar coordinates to Cartesian for plotting
    x = solution.y[0] * np.cos(solution.y[2])
    y = solution.y[0] * np.sin(solution.y[2])

    # Plot the trajectory
    plt.plot(x, y, label=f'Trajectory {i+1} (v={vtheta0/1000:.1f} km/s)')

# Plot customization
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Trajectories in a Gravitational Field with Fixed Earth')
plt.legend(loc='upper left', fontsize=8)
plt.grid(True)
plt.axis('equal')
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1tmNx00N0d6ZO2M9a7sIeov0q_ArNJI7H#scrollTo=D2oE4rHnG28i)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/2%20Gravity/Unknown-16.png?raw=true)

---

### **5. Discussion**

1. **Orbital and Escape Dynamics:**

   * Velocities below 7.9 km/s result in elliptical orbits.
   * Velocities around 11.2 km/s result in parabolic trajectories (escape).
   * Velocities above 11.2 km/s show hyperbolic escape trajectories.

2. **Real-World Applications:**

   * Satellite launches: Achieving stable orbits with minimum energy.
   * Reentry planning: Calculating safe descent paths.
   * Space missions: Determining escape velocities for interplanetary travel.

---

### **6. Conclusion**

By simulating trajectories with varying initial velocities, we demonstrate the critical role of initial speed in determining whether the payload remains bound to Earth or escapes into space. The graphical and animated representations provide a clear visualization of these dynamics.

# Problem 1
# Electromagnetism: Lorentz Force & Trajectories of Charged Particles

## Motivation

Understanding how charged particles move in electromagnetic fields is essential for:

* Plasma physics
* Particle accelerators
* Magnetic confinement in fusion
* Mass spectrometry
* Astrophysics and space weather

We focus on the **Lorentz Force**, which governs the motion of a charged particle in electric and magnetic fields. This force alters the trajectory, causing particles to spiral, drift, or move in circles depending on initial conditions.

---

## Lorentz Force Equation

The **Lorentz Force** on a particle of charge $q$ is given by:

$$
\vec{F} = q(\vec{E} + \vec{v} \times \vec{B})
$$

Where:

* $\vec{E}$: electric field
* $\vec{B}$: magnetic field
* $\vec{v}$: velocity of the particle

For now, we will focus on **magnetic field only**:

$$
\vec{F} = q (\vec{v} \times \vec{B})
$$

Using Newton’s Second Law:

$$
\vec{a} = \frac{d\vec{v}}{dt} = \frac{\vec{F}}{m} = \frac{q}{m} (\vec{v} \times \vec{B})
$$

We solve this system numerically using time-stepping.

---

## Physical Intuition

* If $\vec{v} \perp \vec{B}$: the particle undergoes **circular motion**
* If $\vec{v}$ has a component parallel to $\vec{B}$: **spiral motion**
* If $\vec{E} \neq 0$: can result in **drift motion**

---

## Simulation Setup

**Assumptions:**

* Charge $q = 1 \, \text{C}$
* Mass $m = 1 \, \text{g} = 0.001 \, \text{kg}$
* $\vec{B} = (0, 0, 1) \, \text{T}$ (magnetic field along z-axis)

---

## Code

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Constants
q = 1     # Charge (C)
m = 0.001 # Mass (kg)
dt = 0.001
steps = 500

# Magnetic Field
B = np.array([0, 0, 1])

# Initial Conditions
def simulate_trajectory(v0, r0, damping=0.0):
    r = np.zeros((steps, 3))
    v = v0.copy()
    r[0] = r0.copy()

    for i in range(1, steps):
        F = q * np.cross(v, B)
        a = F / m
        v += a * dt
        v *= (1 - damping)  # simple damping
        r[i] = r[i-1] + v * dt

    return r
```

---

## Visualizing Trajectories

---

## Animation Code (Spiral Case)

```python
from matplotlib import animation
from IPython.display import HTML

def animate_trajectory(traj):
    fig = plt.figure(figsize=(6, 5))
    ax = fig.add_subplot(111, projection='3d')
    ax.set_xlim(np.min(traj[:,0]), np.max(traj[:,0]))
    ax.set_ylim(np.min(traj[:,1]), np.max(traj[:,1]))
    ax.set_zlim(np.min(traj[:,2]), np.max(traj[:,2]))
    line, = ax.plot([], [], [], lw=2)

    def init():
        line.set_data([], [])
        line.set_3d_properties([])
        return line,

    def update(frame):
        line.set_data(traj[:frame, 0], traj[:frame, 1])
        line.set_3d_properties(traj[:frame, 2])
        return line,

    ani = animation.FuncAnimation(fig, update, frames=len(traj), init_func=init,
                                  interval=40, blit=True)
    return ani

spiral = simulate_trajectory(np.array([1.0, 0.0, 0.5]), np.array([0.0, 0.0, 0.0]))
ani = animate_trajectory(spiral)
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1_moLRazWlQaZPgkIXS1iNou0NHHlH8U-#scrollTo=f-tm-PLyyMPQ)

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Physical constants
q = 1.0       # charge in C
m = 0.001     # mass in kg
B = np.array([0, 0, 1])     # magnetic field in z direction (T)
E = np.array([0, 0, 0])     # electric field (optional)
v0 = np.array([1., 1., 0.5])  # initial velocity in m/s - Changed to float
r0 = np.array([0., 0., 0.])    # initial position - Changed to float

# Time settings
dt = 0.001 # Reduced time step
steps = 1000 # Reduced number of steps

# Arrays to store trajectory
positions = np.zeros((steps, 3))
velocities = np.zeros((steps, 3))
positions[0] = r0
velocities[0] = v0

# Simulation (Euler method)
for i in range(1, steps):
    v = velocities[i-1]
    a = (q / m) * (E + np.cross(v, B))
    velocities[i] = v + a * dt
    positions[i] = positions[i-1] + velocities[i] * dt

# --- 3D Animation ---
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection='3d')
# Adjust limits based on trajectory data
ax.set_xlim(np.min(positions[:,0]), np.max(positions[:,0]))
ax.set_ylim(np.min(positions[:,1]), np.max(positions[:,1]))
ax.set_zlim(np.min(positions[:,2]), np.max(positions[:,2]))

ax.set_title("Lorentz Force: Particle Trajectory")
line, = ax.plot([], [], [], lw=2)
point, = ax.plot([], [], [], 'ro')

def init():
    line.set_data([], [])
    line.set_3d_properties([])
    point.set_data([], [])
    point.set_3d_properties([])
    return line, point

def update(frame):
    line.set_data(positions[:frame, 0], positions[:frame, 1])
    line.set_3d_properties(positions[:frame, 2])
    # Pass coordinates as sequences for the point
    point.set_data([positions[frame, 0]], [positions[frame, 1]])
    point.set_3d_properties([positions[frame, 2]])
    return line, point

ani = FuncAnimation(fig, update, frames=range(0, steps, 10), init_func=init,
                    blit=True, interval=30)

HTML(ani.to_jshtml())  # For Colab inline display
```
Visit: [Colab](https://colab.research.google.com/drive/1_moLRazWlQaZPgkIXS1iNou0NHHlH8U-#scrollTo=f-tm-PLyyMPQ)

---
```python
# Constants
q = 1.0       # charge (Coulombs)
m = 1.0       # mass (kg)
B = np.array([0, 0, 1.0])  # Magnetic field along z
dt = 0.01
steps = 1000

def simulate_motion(v0, r0, E=np.array([0.0, 0.0, 0.0]), damping=0.0):
    r = np.zeros((steps, 3))
    v = v0.copy()
    r[0] = r0.copy()

    for i in range(1, steps):
        F = q * (np.cross(v, B) + E)
        a = F / m
        v += a * dt
        v *= (1.0 - damping)
        r[i] = r[i - 1] + v * dt
    return r

# 1. Circular trajectory: v ⊥ B, no E field
r_circular = simulate_motion(np.array([1.0, 0.0, 0.0]), np.array([0.0, 1.0, 0.0]))

# 2. E x B drift: add electric field in x direction, v in y
r_drift = simulate_motion(np.array([0.0, 1.0, 0.0]), np.array([0.0, 0.0, 0.0]), E=np.array([1.0, 0.0, 0.0]))

# 3. Spiral motion: add z-component to velocity and weak damping
r_spiral = simulate_motion(np.array([1.0, 0.0, 0.5]), np.array([0.0, 1.0, 0.0]), damping=0.001)

# Plotting
fig = plt.figure(figsize=(12, 9))

# Plot 1: Circular
ax1 = fig.add_subplot(311, projection='3d')
ax1.plot(r_circular[:, 0], r_circular[:, 1], r_circular[:, 2], color='blue')
ax1.set_title("Circular Trajectory in Magnetic Field")
ax1.set_xlabel("X")
ax1.set_ylabel("Y")
ax1.set_zlabel("Z")

# Plot 2: E x B drift
ax2 = fig.add_subplot(312, projection='3d')
ax2.plot(r_drift[:, 0], r_drift[:, 1], r_drift[:, 2], color='blue')
ax2.set_title("E × B Drift of Charged Particle")
ax2.set_xlabel("X")
ax2.set_ylabel("Y")
ax2.set_zlabel("Z")

# Plot 3: Spiral
ax3 = fig.add_subplot(313, projection='3d')
ax3.plot(r_spiral[:, 0], r_spiral[:, 1], r_spiral[:, 2], color='blue')
ax3.set_title("Stable Spiral Trajectory in Magnetic Field")
ax3.set_xlabel("X")
ax3.set_ylabel("Y")
ax3.set_zlabel("Z")

plt.tight_layout()
plt.show()
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/4%20Electromagnetism/Unknown-39.png?raw=true)

## Insights

* The Lorentz force is always perpendicular to velocity ⇒ no work done ⇒ speed constant.
* Radius of circular motion:

$$
r = \frac{mv}{qB}
$$

* Period of rotation:

$$
T = \frac{2\pi m}{qB}
$$

* Electric fields induce drift depending on E ⊥ B direction.

---

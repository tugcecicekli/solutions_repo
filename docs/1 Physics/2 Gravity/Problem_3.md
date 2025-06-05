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
plt.figure(figsize=(8, 8))
plt.plot(trajectory[:, 0], trajectory[:, 1], label="Payload Trajectory")
earth = plt.Circle((0, 0), R_earth, color='blue', alpha=0.5, label='Earth')
plt.gca().add_patch(earth)
plt.gca().set_aspect('equal')
plt.xlabel('x (m)')
plt.ylabel('y (m)')
plt.title('Trajectory of Released Payload Near Earth')
plt.legend()
plt.grid(True)
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1tmNx00N0d6ZO2M9a7sIeov0q_ArNJI7H#scrollTo=D2oE4rHnG28i)


---


## **4. Animation Code**

```python
fig, ax = plt.subplots(figsize=(8, 8))
ax.set_xlim(-2*R_earth, 2*R_earth)
ax.set_ylim(-2*R_earth, 2*R_earth)
ax.set_aspect('equal')
ax.set_title("Payload Trajectory Animation")

earth = plt.Circle((0, 0), R_earth, color='blue', alpha=0.5)
payload, = ax.plot([], [], 'ro', markersize=4)
path, = ax.plot([], [], 'r-', linewidth=1)

ax.add_patch(earth)

def init():
    payload.set_data([], [])
    path.set_data([], [])
    return payload, path

def update(frame):
    payload.set_data([trajectory[frame, 0]], [trajectory[frame, 1]]) # Fix: Pass sequences
    path.set_data(trajectory[:frame+1, 0], trajectory[:frame+1, 1])
    return payload, path

ani = FuncAnimation(fig, update, frames=len(trajectory), init_func=init,
                    blit=True, interval=30)

HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1tmNx00N0d6ZO2M9a7sIeov0q_ArNJI7H#scrollTo=D2oE4rHnG28i)

```python
fig, ax = plt.subplots(figsize=(10, 10))

# Earth
earth = plt.Circle((0, 0), R_earth, color='blue', alpha=0.4, label='Earth')
ax.add_patch(earth)

# Trajectories
for (v_kms, traj), color in zip(trajectories, colors):
    if traj.shape[0] > 0:
        ax.plot(traj[:, 0], traj[:, 1], color=color, label=f"{v_kms:.1f} km/s")

# Initial position marker
ax.plot(r0[0], r0[1], 'ro', label="Launch Point")

# Formatting
ax.set_title("Trajectories from 800 km Altitude for Various Velocities")
ax.set_xlabel("x (m)")
ax.set_ylabel("y (m)")
ax.set_aspect('equal')
ax.set_xlim(-2*R_earth, 2*R_earth)
ax.set_ylim(-2*R_earth, 2*R_earth)
ax.grid(True)
ax.legend(loc='upper right')
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1tmNx00N0d6ZO2M9a7sIeov0q_ArNJI7H#scrollTo=D2oE4rHnG28i)


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

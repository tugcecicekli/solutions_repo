# Problem 1
# Investigating the Range as a Function of the Angle of Projection

---

## Motivation

Projectile motion is a fundamental topic in mechanics that offers a clean yet rich context for understanding the application of differential equations, Newton's laws, and kinematic principles. The central goal is to investigate how the horizontal range of a projectile depends on the angle of launch. Though the setup appears straightforward, the analysis reveals intricate dependencies on initial conditions and physical parameters.

Understanding projectile motion is essential in a variety of fields—from sports and civil engineering to space science and military applications. By starting from first principles and layering in simulation, this project provides both theoretical insight and computational practice.

---

## 1. Theoretical Foundation: Differential Equations

### Assumptions:
- The motion is in two dimensions.
- Air resistance is neglected.
- Gravity acts uniformly downward.
- Launch and landing heights are equal.

We begin with Newton's second law:
$\vec{F} = m\vec{a} \Rightarrow \frac{d^2 \vec{r}}{dt^2} = \vec{a}$

Decomposing motion into x (horizontal) and y (vertical) directions:

#### Horizontal Motion:
$\frac{d^2x}{dt^2} = 0 \Rightarrow \frac{dx}{dt} = v_0 \cos(\theta) \Rightarrow x(t) = v_0 \cos(\theta) t$

#### Vertical Motion:
$\frac{d^2y}{dt^2} = -g \Rightarrow \frac{dy}{dt} = v_0 \sin(\theta) - gt \Rightarrow y(t) = v_0 \sin(\theta) t - \frac{1}{2}gt^2$

### Time of Flight:
Set $y(t) = 0$ to find when the projectile lands:
$t = \frac{2v_0 \sin(\theta)}{g}$

### Horizontal Range:
$R = v_0 \cos(\theta) \cdot \frac{2v_0 \sin(\theta)}{g} = \frac{v_0^2 \sin(2\theta)}{g}$

This is the key result showing how range depends on initial velocity, gravity, and angle.

---

## 2. Simulation: Python Code and Visualizations

### Range vs. Angle Plot
```python
import numpy as np
import matplotlib.pyplot as plt

v0 = 30  # initial velocity in m/s
g = 9.81  # gravitational acceleration in m/s^2
angles = np.linspace(0, 90, 500)
angles_rad = np.radians(angles)

# Calculate range
R = (v0**2 * np.sin(2 * angles_rad)) / g

# Plot range vs angle
plt.figure(figsize=(10, 6))
plt.plot(angles, R, color='royalblue')
plt.axvline(45, color='red', linestyle='--', label='\u03b8 = 45\xb0 (Max Range)')
plt.title('Projectile Range vs. Launch Angle')
plt.xlabel('Angle of Projection (degrees)')
plt.ylabel('Range (meters)')
plt.grid(True)
plt.legend()
plt.show()
```

Visit: [Colab](https://colab.research.google.com/drive/1IbveeROGuSfvVgSacjFp2BdC1BfIn6ZK)

### Output:

---

### Multiple Trajectories for Different Angles
```python
# Time vector
angles_deg = [15, 30, 45, 60, 75]
colors = ['blue', 'green', 'orange', 'purple', 'brown']

plt.figure(figsize=(10, 6))
for angle, color in zip(angles_deg, colors):
    theta = np.radians(angle)
    t_flight = 2 * v0 * np.sin(theta) / g
    t_vals = np.linspace(0, t_flight, 300)
    x = v0 * np.cos(theta) * t_vals
    y = v0 * np.sin(theta) * t_vals - 0.5 * g * t_vals**2
    plt.plot(x, y, label=f"{angle}°", color=color)

plt.title("Projectile Trajectories for Different Angles")
plt.xlabel("Horizontal Distance (m)")
plt.ylabel("Vertical Height (m)")
plt.grid(True)
plt.legend()
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1IbveeROGuSfvVgSacjFp2BdC1BfIn6ZK)
### Output:
Multiple arcs, illustrating how angle affects height and range. Lower angles are flatter; higher angles reach greater heights but fall short in range.

---

### Animated Projectile Motion
```python
import matplotlib.animation as animation

fig, ax = plt.subplots(figsize=(8, 5))
angle = np.radians(45)
t_flight = 2 * v0 * np.sin(angle) / g
t_vals = np.linspace(0, t_flight, 300)
x_vals = v0 * np.cos(angle) * t_vals
y_vals = v0 * np.sin(angle) * t_vals - 0.5 * g * t_vals**2

line, = ax.plot([], [], 'bo')
ax.set_xlim(0, max(x_vals))
ax.set_ylim(0, max(y_vals)*1.1)
ax.set_xlabel('Horizontal Distance (m)')
ax.set_ylabel('Vertical Height (m)')
ax.set_title('Animated Projectile Motion')

# Initialization function
def init():
    line.set_data([], [])
    return line,

# Animation function
def animate(i):
    line.set_data(x_vals[i], y_vals[i])
    return line,

ani = animation.FuncAnimation(fig, animate, frames=len(t_vals), init_func=init, interval=20, blit=True)
plt.show()
```
Visit:[Colab](https://colab.research.google.com/drive/1IbveeROGuSfvVgSacjFp2BdC1BfIn6ZK#scrollTo=lAgwoWZfKqAD)
---

### Range vs. Angle for Different Velocities
```python
velocities = [10, 20, 30, 40]

plt.figure(figsize=(10, 6))
for v in velocities:
    R = (v**2 * np.sin(2 * angles_rad)) / g
    plt.plot(angles, R, label=f'v₀ = {v} m/s')

plt.title('Range vs. Angle for Various Initial Velocities')
plt.xlabel('Angle (degrees)')
plt.ylabel('Range (m)')
plt.legend()
plt.grid(True)
plt.show()
```
Visit[Colab](https://colab.research.google.com/drive/1IbveeROGuSfvVgSacjFp2BdC1BfIn6ZK#scrollTo=lAgwoWZfKqAD)
### Output:
Curves rising in height as initial velocity increases, but always peaking at 45°.

---

## 3. Practical Applications

- **Sports**: Optimize kicking or throwing angles.
- **Engineering**: Design water fountains, civil projectiles, or robotic arm paths.
- **Aerospace**: Launch trajectories under different gravitational conditions.
- **Defense**: Missile and artillery trajectory planning.

---

## 4. Extensions and Real-World Models

### Air Resistance
Introducing drag results in non-linear differential equations. For example:
$m \frac{d^2x}{dt^2} = -kv_x, \quad m \frac{d^2y}{dt^2} = -mg - kv_y$
where $k$ is the drag coefficient.

This requires numerical solving methods (like Runge-Kutta) for simulation.

### Python Simulation with Air Resistance (Euler Method)
```python
def simulate_drag(v0, theta_deg, dt=0.01, k=0.1):
    theta = np.radians(theta_deg)
    vx, vy = v0 * np.cos(theta), v0 * np.sin(theta)
    x, y = 0, 0
    positions_x, positions_y = [x], [y]

    while y >= 0:
        ax = -k * vx
        ay = -g - k * vy
        vx += ax * dt
        vy += ay * dt
        x += vx * dt
        y += vy * dt
        positions_x.append(x)
        positions_y.append(y)

    return positions_x, positions_y

plt.figure(figsize=(10, 6))
for angle in [30, 45, 60]:
    x_vals, y_vals = simulate_drag(30, angle)
    plt.plot(x_vals, y_vals, label=f"With drag, {angle}°")

plt.xlabel("Horizontal Distance (m)")
plt.ylabel("Vertical Height (m)")
plt.title("Projectile Motion with Air Resistance")
plt.legend()
plt.grid(True)
plt.show()
```
Visit[Colab](https://colab.research.google.com/drive/1IbveeROGuSfvVgSacjFp2BdC1BfIn6ZK#scrollTo=hpRSVGJcORIp)

### Uneven Terrain and Variable Gravity
Other extensions include:
- Launching from/landing on slopes (geometry needed)
- Varying $g$ with altitude or planetary conditions

---

## Conclusion

Starting from Newton's laws, we built a full mathematical model of projectile motion using differential equations. We derived the range formula and implemented visual simulations to understand how the angle of projection affects range. The model, while idealized, provides deep insight and serves as a powerful foundation for more complex analyses.

With Python, we've created tools to simulate, visualize, and animate this system dynamically. Extensions include modeling drag and simulating on non-flat terrains—perfect for bridging theory and real-world applications.

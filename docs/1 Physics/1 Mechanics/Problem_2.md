# Problem 2
# Investigating the Dynamics of a Forced Damped Pendulum

---

## Motivation

The pendulum is a powerful model in physics. From simple harmonic motion to chaos, it demonstrates how small changes — damping and forcing — result in complex behavior. This project investigates:

- **Simple Pendulum**
- **Damped Pendulum**
- **Forced Damped Pendulum**

---

## 1. Differential Equations

### Simple Pendulum

$$\frac{d^2\theta}{dt^2} + \omega_0^2 \sin(\theta) = 0$$

For small angles:

$$\frac{d^2\theta}{dt^2} + \omega_0^2 \theta = 0, \quad \omega_0 = \sqrt{\frac{g}{L}}$$

---

### Damped Pendulum

$$\frac{d^2\theta}{dt^2} + b \frac{d\theta}{dt} + \omega_0^2 \theta = 0$$

---

### Forced Damped Pendulum

$$\frac{d^2\theta}{dt^2} + b \frac{d\theta}{dt} + \omega_0^2 \sin(\theta) = A \cos(\omega t)$$

---

## 2. Visualizing the Simple Pendulum (Small Angle Approximation)

```python
import numpy as np
import matplotlib.pyplot as plt

# Parameters
g = 9.81
L = 1.0
omega0 = np.sqrt(g / L)
theta0 = 0.2
t = np.linspace(0, 10, 1000)
theta = theta0 * np.cos(omega0 * t)

# Plot
plt.figure(figsize=(10, 4))
plt.plot(t, theta)
plt.title("Simple Pendulum (Small Angle Approximation)")
plt.xlabel("Time (s)")
plt.ylabel("Angle (rad)")
plt.grid(True)
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1mZ4tKUUQdA2M2ejXxRenjbDZ4mv9cX8s#scrollTo=AMg6N81d6J_D)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/1%20Mechanics/Unknown5.png?raw=true)
---

## 3. Visualizing the Damped Pendulum 

```python
# Damped system using RK4
import numpy as np
import matplotlib.pyplot as plt
dt = 0.01
t = np.arange(0, 20, dt)
b = 0.3  # damping
theta = np.zeros_like(t)
omega = np.zeros_like(t)
theta[0] = 0.5
# Define omega0 here 
omega0 = 1.0  # You can change this value to the desired natural frequency

def rk4_damped(theta, omega, t, dt, b, omega0):
    def f(t, y):
        theta, omega = y
        return np.array([omega, -b * omega - omega0**2 * theta])
    
    y = np.array([theta, omega])
    k1 = f(t, y)
    k2 = f(t + dt/2, y + dt*k1/2)
    k3 = f(t + dt/2, y + dt*k2/2)
    k4 = f(t + dt, y + dt*k3)
    return y + dt * (k1 + 2*k2 + 2*k3 + k4)/6

for i in range(1, len(t)):
    theta[i], omega[i] = rk4_damped(theta[i-1], omega[i-1], t[i-1], dt, b, omega0)


plt.figure(figsize=(10, 4))
plt.plot(t, theta)
plt.title("Damped Pendulum Motion")
plt.xlabel("Time (s)")
plt.ylabel("Angle (rad)")
plt.grid(True)
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1mNZ4qqAkfLGtsvfvqSBXUXPuzzm0EqMt#scrollTo=BRT2yI8kOk18)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/1%20Mechanics/Unknown-6.png?raw=true)
---

## 4. Visualizing the Forced Damped Pendulum

```python
# Forced Damped Pendulum Simulation
import numpy as np
import matplotlib.pyplot as plt

# Parameters
omega0 = 2
dt = 0.01
t = np.arange(0, 10, dt)
A = 1.2
omega_d = 2/3
b = 0.5
theta = np.zeros_like(t)
omega = np.zeros_like(t)
theta[0] = 0.5

def rk4_forced(theta, omega, t, dt, b, A, omega_d, omega0):
    def f(t, y):
        theta, omega = y
        return np.array([omega, -b * omega - omega0**2 * np.sin(theta) + A * np.cos(omega_d * t)])
    
    y = np.array([theta, omega])
    k1 = f(t, y)
    k2 = f(t + dt/2, y + dt*k1/2)
    k3 = f(t + dt/2, y + dt*k2/2)
    k4 = f(t + dt, y + dt*k3)
    return y + dt * (k1 + 2*k2 + 2*k3 + k4)/6

for i in range(1, len(t)):
    theta[i], omega[i] = rk4_forced(theta[i-1], omega[i-1], t[i-1], dt, b, A, omega_d, omega0)

# Plot
plt.figure(figsize=(10, 4))
plt.plot(t, theta)
plt.title("Forced Damped Pendulum Motion")
plt.xlabel("Time (s)")
plt.ylabel("Angle (rad)")
plt.grid(True)
plt.show()
```
Visit: [Colab](https://colab.research.google.com/drive/1J1BiWvt02427kZD9_FCtcQln2kks8YP0)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/1%20Mechanics/Unknown-7.png?raw=true)
---

## 5. Animation of the Forced Damped Pendulum

```python
import matplotlib.animation as animation
from IPython.display import HTML
import numpy as np # This was missing

# Assuming L represents the length of the pendulum, set it to a reasonable value
L = 1  # You can adjust this value as needed

x_vals = L * np.sin(theta)
y_vals = -L * np.cos(theta)

fig, ax = plt.subplots(figsize=(6, 6))
ax.set_xlim(-1.2, 1.2)
ax.set_ylim(-1.2, 1.2)
ax.set_aspect('equal')
ax.grid()
line, = ax.plot([], [], 'o-', lw=2)
trail, = ax.plot([], [], '-', lw=0.5)
xdata, ydata = [], []

def init():
    line.set_data([], [])
    trail.set_data([], [])
    return line, trail

def animate(i):
    x, y = x_vals[i], y_vals[i]
    xdata.append(x)
    ydata.append(y)
    if len(xdata) > 100:
        xdata.pop(0)
        ydata.pop(0)
    line.set_data([0, x], [0, y])
    trail.set_data(xdata, ydata)
    return line, trail

ani = animation.FuncAnimation(fig, animate, frames=range(0, len(x_vals), 5),
                              init_func=init, blit=True, interval=20)
plt.close()
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1J1BiWvt02427kZD9_FCtcQln2kks8YP0#scrollTo=vd6dcAXcQ5U9)
---
```
import numpy as np
import matplotlib.pyplot as plt

# Constants
g = 9.81
L = 1.0
omega0 = np.sqrt(g / L)
theta0 = 0.2
omega_init = 0.0
dt = 0.01
t_max = 20
t = np.arange(0, t_max, dt)

# Unified RK4 method
def rk4_step(theta, omega, t, dt, b=0, A=0, omega_d=0, use_sin=True):
    def f(t, y):
        theta, omega = y
        dtheta = omega
        if use_sin:
            domega = -b * omega - omega0**2 * np.sin(theta) + A * np.cos(omega_d * t)
        else:
            domega = -b * omega - omega0**2 * theta + A * np.cos(omega_d * t)
        return np.array([dtheta, domega])
    y = np.array([theta, omega])
    k1 = f(t, y)
    k2 = f(t + dt/2, y + dt*k1/2)
    k3 = f(t + dt/2, y + dt*k2/2)
    k4 = f(t + dt, y + dt*k3)
    return y + dt * (k1 + 2*k2 + 2*k3 + k4)/6

def simulate(b=0, A=0, omega_d=0, use_sin=True):
    theta = np.zeros_like(t)
    omega = np.zeros_like(t)
    theta[0], omega[0] = theta0, omega_init
    for i in range(1, len(t)):
        theta[i], omega[i] = rk4_step(theta[i-1], omega[i-1], t[i-1], dt, b, A, omega_d, use_sin)
    return theta, omega

# Simulate all three
theta1, omega1 = simulate(b=0, A=0, use_sin=False)                   # Simple
theta2, omega2 = simulate(b=0.2, A=0, use_sin=False)                 # Damped
theta3, omega3 = simulate(b=0.5, A=1.2, omega_d=2/3, use_sin=True)   # Forced

# Plot layout
fig, axs = plt.subplots(3, 2, figsize=(12, 10))
titles = ["1) Simple Pendulum", "2) Damped Pendulum", "3) Forced Pendulum"]
colors = ['red', 'blue', 'teal']

# Time Series
axs[0, 0].plot(t, theta1, color=colors[0])
axs[1, 0].plot(t, theta2, color=colors[1])
axs[2, 0].plot(t, theta3, color=colors[2])
for i in range(3):
    axs[i, 0].set_title("Time Series")
    axs[i, 0].set_xlabel("Time (s)")
    axs[i, 0].set_ylabel("θ (rad)")
    axs[i, 0].grid(True)
    axs[i, 0].annotate(titles[i], xy=(0.95, 0.85), xycoords='axes fraction',
                      ha='right', fontsize=11, color=colors[i], weight='bold')

# Phase Portraits
axs[0, 1].plot(theta1, omega1, color=colors[0])
axs[1, 1].plot(theta2, omega2, color=colors[1])
axs[2, 1].plot(theta3, omega3, color=colors[2])
for i in range(3):
    axs[i, 1].set_title("Phase Portrait")
    axs[i, 1].set_xlabel("θ (rad)")
    axs[i, 1].set_ylabel("ω (rad/s)")
    axs[i, 1].grid(True)

plt.tight_layout()
plt.show()
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/1%20Mechanics/Unknown-6.png?raw=true)

## 6. Extensions and Advanced Explorations

- **Phase Portraits & Poincaré Sections**: Reveal geometry of motion and transitions to chaos.
- **Bifurcation Diagrams**: Vary forcing amplitude $A$ or frequency $\omega$ and plot long-term values.
- **Energy Analysis**: Study how energy is gained/lost under forcing and damping.
- **Lyapunov Exponents**: Quantify sensitivity to initial conditions (chaos).
- **Double Pendulum**: A 2-link pendulum introduces deeper chaos with no external force.
- **Planetary Gravity**: Modify $g$ to simulate behavior on Mars, Moon, etc.

---

## 7. Conclusion

This project comprehensively examined pendulum dynamics in three stages:

| Variant               | Damping | Forcing | Behavior                             |
|----------------------|---------|---------|--------------------------------------|
| Simple Pendulum      | ✘       | ✘       | Periodic                             |
| Damped Pendulum      | ✔       | ✘       | Decaying oscillations                |
| Forced Damped        | ✔       | ✔       | Periodic / Quasiperiodic / Chaotic   |

We:
- Derived differential equations
- Simulated each case using Runge-Kutta
- Visualized the systems with graphs and animations
- Proposed extensions into bifurcations, chaos, and energy studies

The forced damped pendulum, though conceptually simple, becomes a gateway into the study of nonlinear systems and chaos — illustrating how deterministic rules can yield unpredictable motion.

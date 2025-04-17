# Problem 2
# Investigating the Dynamics of a Forced Damped Pendulum

---

## Motivation

The pendulum is more than a classic classroom example — it's a portal into the world of nonlinear physics. From the elegance of simple harmonic motion to the unpredictability of chaos, the pendulum reveals how small changes in conditions lead to vastly different outcomes. In this project, we:

- Explore **three types** of pendulums:
  1. Simple (undamped, unforced)
  2. Damped (no forcing)
  3. Forced Damped (full system)
- Analyze them **theoretically and computationally**
- Visualize them through **graphs and animations**
- Extend our study into **chaotic behavior and bifurcations**

---

## 1. Differential Equations

### A. Simple Pendulum

$$\frac{d^2\theta}{dt^2} + \omega_0^2 \sin(\theta) = 0, \quad \omega_0 = \sqrt{\frac{g}{L}}$$

**Small-angle approximation**:

$$\frac{d^2\theta}{dt^2} + \omega_0^2 \theta = 0$$

---

### B. Damped Pendulum

$$\frac{d^2\theta}{dt^2} + b \frac{d\theta}{dt} + \omega_0^2 \sin(\theta) = 0$$

---

### C. Forced Damped Pendulum

$$\frac{d^2\theta}{dt^2} + b \frac{d\theta}{dt} + \omega_0^2 \sin(\theta) = A \cos(\omega t)$$

Where:
- $\theta$: angular displacement  
- $b$: damping coefficient  
- $A$: amplitude of driving force  
- $\omega$: driving frequency  

---

## 2. Python Simulation and Visualization

### Imports and Parameters

```python
import numpy as np
import matplotlib.pyplot as plt
```

```python
# Constants
g = 9.81
L = 1.0
omega0 = np.sqrt(g / L)
theta0 = 0.2
omega_init = 0.0
dt = 0.01
t_max = 20
t = np.arange(0, t_max, dt)
```

---

### Runge-Kutta Solver 

```python
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
```

---

### Simulate and Plot All Three Systems

```python
theta_simple, _ = simulate(b=0, A=0, use_sin=False)
theta_damped, _ = simulate(b=0.2, A=0, use_sin=False)
theta_forced, _ = simulate(b=0.5, A=1.2, omega_d=2/3)

plt.figure(figsize=(12, 6))
plt.plot(t, theta_simple, label='Simple Pendulum')
plt.plot(t, theta_damped, label='Damped Pendulum')
plt.plot(t, theta_forced, label='Forced Damped Pendulum')
plt.title("Pendulum Motion Comparison")
plt.xlabel("Time (s)")
plt.ylabel("Angle (rad)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 3. Pendulum Animation (Colab-Ready)

### Animation Code for Forced Damped Pendulum

```python
import matplotlib.animation as animation
from IPython.display import HTML

theta_anim, _ = simulate(b=0.5, A=1.2, omega_d=2/3)
x_vals = L * np.sin(theta_anim)
y_vals = -L * np.cos(theta_anim)

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

### To Animate Simple or Damped Pendulum

Just replace:

```python
theta_anim, _ = simulate(b=0.5, A=1.2, omega_d=2/3)
```

With:
- Simple: `simulate(b=0, A=0, use_sin=False)`
- Damped: `simulate(b=0.2, A=0, use_sin=False)`

---

## 4. Extensions and Advanced Explorations

- **Phase Portraits & Poincaré Sections**: Reveal geometry of motion and transitions to chaos.
- **Bifurcation Diagrams**: Vary forcing amplitude $A$ or frequency $\omega$ and plot long-term values.
- **Energy Analysis**: Study how energy is gained/lost under forcing and damping.
- **Lyapunov Exponents**: Quantify sensitivity to initial conditions (chaos).
- **Double Pendulum**: A 2-link pendulum introduces deeper chaos with no external force.
- **Planetary Gravity**: Modify $g$ to simulate behavior on Mars, Moon, etc.

---

## 5. Conclusion

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

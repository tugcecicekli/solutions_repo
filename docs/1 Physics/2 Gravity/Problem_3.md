# Problem 3
# **Trajectories of a Freely Released Payload Near Earth**

## **1. Introduction**  
When a payload is released from a moving rocket near Earth, its motion depends on **initial velocity, altitude, and gravitational forces**. Understanding these trajectories is essential for **orbital insertion, reentry planning, and satellite deployment**.  

The possible paths include:  
- **Elliptical orbits** (if velocity is below escape velocity).  
- **Parabolic trajectories** (if velocity equals escape velocity).  
- **Hyperbolic escape paths** (if velocity exceeds escape velocity).  
- **Suborbital trajectories** (if reentry occurs).  

---

## **2. Theoretical Background**  

### **2.1 Governing Equations**  
Using Newton’s Second Law and Universal Gravitation, the equation of motion for a payload influenced only by Earth’s gravity is:

$$\mathbf{F} = m \mathbf{a} = -\frac{G M m}{r^2} \hat{r}$$

which leads to the **gravitational acceleration**:

$$\mathbf{a} = -\frac{G M}{r^2} \hat{r}$$

where:  
- $G = 6.674 \times 10^{-11}$ m³/kg/s² (gravitational constant).  
- $M = 5.972 \times 10^{24}$ kg (mass of Earth).  
- $r$ is the payload’s radial distance from Earth’s center.  

For **different initial velocities** $( v_0)$ at release altitude $r_0$:  
- If $v_0 < v_1$ (orbital velocity): **Suborbital reentry**.  
- If $v_0 = v_1$: **Circular orbit**.  
- If $v_1 < v_0 < v_2$ (escape velocity): **Elliptical orbit**.  
- If $v_0 = v_2$: **Parabolic escape**.  
- If $v_0 > v_2$: **Hyperbolic escape**.  

---

## **3. Numerical Simulation**  

We simulate a payload's trajectory using **Runge-Kutta methods** to solve the equation of motion in two dimensions.

### **Python Implementation**

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp
from scipy.constants import G

# Define Earth parameters
M_earth = 5.972e24  # kg
R_earth = 6.371e6  # meters

# Define the equations of motion
def equations(t, state):
    x, y, vx, vy = state
    r = np.sqrt(x**2 + y**2)
    ax = -G * M_earth * x / r**3
    ay = -G * M_earth * y / r**3
    return [vx, vy, ax, ay]

# Initial conditions: Release altitude and velocity
altitude = 400e3  # 400 km above Earth
r0 = R_earth + altitude
v0_values = [6.8e3, 7.9e3, 11.2e3]  # Below, at, and above orbital velocity (m/s)

# Time span
t_span = (0, 5000)
t_eval = np.linspace(*t_span, 1000)

# Simulate and plot different cases
plt.figure(figsize=(7, 7))

for v0 in v0_values:
    initial_state = [r0, 0, 0, v0]  # (x0, y0, vx0, vy0)
    sol = solve_ivp(equations, t_span, initial_state, t_eval=t_eval, method='RK45')
    
    plt.plot(sol.y[0], sol.y[1], label=f'v0 = {v0 / 1e3:.1f} km/s')

# Plot Earth
theta = np.linspace(0, 2*np.pi, 100)
plt.plot(R_earth * np.cos(theta), R_earth * np.sin(theta), 'k', label='Earth')

plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.title("Payload Trajectories Near Earth")
plt.legend()
plt.grid()
plt.axis('equal')
plt.show()
```
https://colab.research.google.com/drive/1tmNx00N0d6ZO2M9a7sIeov0q_ArNJI7H

---

## **4. Results and Discussion**  

- **Suborbital (e.g., \( v_0 = 6.8 \) km/s)**: The payload follows a curved trajectory but falls back to Earth.  
- **Orbital Insertion (e.g., \( v_0 = 7.9 \) km/s)**: The payload enters a stable circular orbit.  
- **Escape Trajectory (e.g., \( v_0 = 11.2 \) km/s)**: The payload follows a hyperbolic path, escaping Earth's gravity.  

### **Real-World Applications**  
- **Satellite Deployment**: Space agencies use precise velocity control to achieve stable orbits.  
- **Reentry Planning**: Spacecraft must control velocity for safe reentry (e.g., ISS, capsules).  
- **Interplanetary Travel**: Higher velocity payloads can escape Earth and travel to Mars or beyond.  

---

## **5. Conclusion**  

This report analyzed the **trajectories of a freely released payload** using theoretical equations and **numerical simulations**. The results confirm how initial velocity dictates whether a payload **reenters, orbits, or escapes**. This understanding is fundamental in **space mission planning, satellite deployment, and interplanetary travel**.

Future extensions could include **atmospheric drag effects, three-body interactions, or maneuvering thruster effects**.

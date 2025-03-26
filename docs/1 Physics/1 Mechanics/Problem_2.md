# Problem 2
### **Investigating the Dynamics of a Forced Damped Pendulum** (Summary)  

#### **1. Introduction**  
The forced damped pendulum is a nonlinear system exhibiting behaviors like **resonance, periodic motion, and chaos** due to damping and external forcing. Understanding its dynamics is crucial for applications in **engineering, energy harvesting, and structural stability**.  

#### **2. Theoretical Background**  
The equation of motion:  
$\ddot{\theta} + b\dot{\theta} + \omega_0^2 \sin\theta = A \cos(\omega t)$
- **Damping $(b)$** controls energy loss.  
- **Forcing $(A, \omega)$** introduces external periodic energy, leading to resonance or chaotic motion.  
- For **small angles $(\sin\theta \approx \theta)$**, the system behaves like a linear **driven harmonic oscillator**.  

#### **3. Numerical Simulation**  
Since the equation is nonlinear, we use the **Runge-Kutta method** to solve it numerically.  

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Define system parameters
b = 0.2   # Damping coefficient
A = 1.5   # Driving force amplitude
omega = 2.0  # Driving frequency
g = 9.81  # Gravity
L = 1.0   # Pendulum length
omega_0 = np.sqrt(g / L)

# Differential equation for the forced damped pendulum
def pendulum_eq(t, y):
    theta, omega_dot = y
    d_theta = omega_dot
    d_omega = -b * omega_dot - omega_0**2 * np.sin(theta) + A * np.cos(omega * t)
    return [d_theta, d_omega]

# Time range and initial conditions
t_span = (0, 50)
t_eval = np.linspace(*t_span, 1000)
y0 = [0.5, 0]  # Initial angle and angular velocity

# Solve the equation
solution = solve_ivp(pendulum_eq, t_span, y0, t_eval=t_eval, method='RK45')

# Plot results
plt.figure(figsize=(10, 5))
plt.plot(solution.t, solution.y[0], label=r'$\theta(t)$')
plt.xlabel('Time (s)')
plt.ylabel('Angle (rad)')
plt.title('Forced Damped Pendulum Motion')
plt.legend()
plt.grid()
plt.show()
```
#### **Source**
[Colab] (https://colab.research.google.com/drive/1mZ4tKUUQdA2M2ejXxRenjbDZ4mv9cX8s)

#### **4. Results and Discussion**  
- **Periodic motion** occurs for weak forcing.  
- **Resonance** amplifies oscillations when $\omega \approx \omega_0$.  
- **Chaos** emerges for specific parameter combinations, observable in **Poincaré sections** and **phase portraits**.  

#### **5. Practical Applications**  
- **Energy Harvesting**: Similar principles are used in vibration-based power generation.  
- **Mechanical Systems**: Used in **suspension bridges, oscillating circuits, and robotics**.  
- **Electrical Analogies**: Equivalent to driven **RLC circuits** in electronics.  

#### **6. Conclusion**  
The forced damped pendulum exhibits **diverse behaviors** based on damping and forcing conditions. **Numerical simulations** reveal transitions from periodic to chaotic motion, with practical implications in **engineering and physics**.  

# Problem 2
# **Escape Velocities and Cosmic Velocities**

## **1. Introduction**  
Escape velocity is the minimum speed an object needs to break free from a celestial body's gravitational pull without further propulsion. This concept extends to **cosmic velocities**, which define thresholds for orbiting, escaping, and leaving a star system. These principles are fundamental in **space exploration**, affecting satellite launches, interplanetary travel, and potential interstellar missions.

### **Cosmic Velocities:**
1. **First Cosmic Velocity** $(v_1)$: Orbital velocity for a stable circular orbit.
2. **Second Cosmic Velocity** $(v_2)$: Escape velocity to leave a planet’s gravity.
3. **Third Cosmic Velocity** $(v_3)$: Velocity needed to escape the Sun’s gravity from a planet.

---

## **2. Theoretical Background**  

### **2.1 First Cosmic Velocity (Orbital Velocity)**
For an object to stay in a circular orbit around a planet of mass $M$ and radius $R$, its centripetal force must equal gravitational force:

$$\frac{G M m}{R^2} = m \frac{v_1^2}{R}$$

Solving for $v_1$:

$$v_1 = \sqrt{\frac{G M}{R}}$$

### **2.2 Second Cosmic Velocity (Escape Velocity)**
The escape velocity is found by equating kinetic energy to gravitational potential energy:

$$\frac{1}{2} m v_2^2 = \frac{G M m}{R}$$

Solving for $v_2$:

$$v_2 = \sqrt{\frac{2 G M}{R}}$$

### **2.3 Third Cosmic Velocity (Interstellar Escape)**
To leave the solar system, an object must overcome both Earth's and the Sun's gravity. The required velocity is:

$$v_3 = \sqrt{v_2^2 + v_{\text{escape,Sun}}^2}$$

where $v_{\text{escape,Sun}}$ is the escape velocity from the Sun at Earth’s orbit.

---

## **3. Computational Simulation**

We compute these velocities for **Earth, Mars, and Jupiter** using Python.

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.constants import G

# Define celestial bodies (Mass in kg, Radius in meters)
bodies = {
    "Earth": (5.972e24, 6.371e6),
    "Mars": (6.417e23, 3.389e6),
    "Jupiter": (1.898e27, 6.9911e7)
}

# Compute first, second, and third cosmic velocities
def compute_velocities(M, R):
    v1 = np.sqrt(G * M / R)  # First cosmic velocity (Orbital)
    v2 = np.sqrt(2 * G * M / R)  # Second cosmic velocity (Escape)
    return v1, v2

# Store results
results = {body: compute_velocities(M, R) for body, (M, R) in bodies.items()}

# Plot results
labels = ["First Cosmic Velocity (km/s)", "Second Cosmic Velocity (km/s)"]
colors = ['blue', 'red']
x = np.arange(len(bodies))

plt.figure(figsize=(8, 5))
for i in range(2):
    plt.bar(x + i * 0.3, [results[body][i] / 1e3 for body in bodies], width=0.3, label=labels[i], color=colors[i])

plt.xticks(x + 0.15, bodies.keys())
plt.ylabel("Velocity (km/s)")
plt.title("Cosmic Velocities for Different Planets")
plt.legend()
plt.grid()
plt.show()
```
[Colab](https://colab.research.google.com/drive/1ZKQKWnH0bVhEBpNgUawo7nmMMx6MW80Y)

---

## **4. Results and Discussion**

- **Earth:**  
  - $v_1 \approx 7.91$ km/s (orbital)  
  - $v_2 \approx 11.19$ km/s (escape)  
- **Mars:**  
  - Lower due to weaker gravity ($v_1 \approx 3.55$ km/s, $v_2 \approx 5.03 $ km/s)  
- **Jupiter:**  
  - Higher due to stronger gravity ($v_1 \approx 42.1$ km/s, $v_2 \approx 59.5$ km/s)  

### **Importance in Space Exploration**
- **Satellites & Space Stations** require first cosmic velocity to remain in orbit.  
- **Missions to the Moon & Mars** need escape velocity to leave Earth.  
- **Interstellar Missions (e.g., Voyager probes)** need third cosmic velocity to leave the solar system.  

---

## **5. Conclusion**

This report derived and computed **cosmic velocities** for Earth, Mars, and Jupiter, demonstrating their importance in space missions. The Python simulation confirmed the theoretical values, showing how gravitational differences impact space travel.

Future work can explore **atmospheric drag, multi-body interactions, and advanced propulsion methods** for interstellar exploration.

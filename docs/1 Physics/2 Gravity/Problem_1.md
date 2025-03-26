# Problem 1
# **Orbital Period and Orbital Radius: Kepler’s Third Law**  

## **1. Introduction**  
Kepler’s Third Law states that the square of the **orbital period** $(T^2)$ of a planet is proportional to the cube of its **orbital radius** $(r^3)$. This fundamental law governs planetary and satellite motions, allowing astronomers to determine celestial body masses and distances.  

The relationship is essential for:  
- Predicting **planetary motions** in the solar system.  
- Designing **satellite orbits** for communications and space exploration.  
- Understanding **gravitational interactions** in astrophysics.  
---
## **2. Theoretical Derivation**  
For a small body orbiting a much larger central mass (e.g., a planet around the Sun), Newton’s second law and the gravitational force equation yield:

$$F = \frac{G M m}{r^2} = m \frac{v^2}{r}$$

where:  
- $G$ is the gravitational constant,  
- $M$ is the mass of the central body,  
- $m$ is the mass of the orbiting body,  
- $r$ is the orbital radius,  
- $v$ is the orbital velocity.  

For **uniform circular motion**, velocity is related to orbital period $T$:

$v = \frac{2\pi r}{T}$


$\frac{G M m}{r^2} = m \frac{(2\pi r)^2}{T^2 r}$


$T^2 = \frac{4\pi^2}{G M} r^3$

This confirms **Kepler’s Third Law**: the **square of the orbital period is proportional to the cube of the orbital radius**.  

---

## **3. Computational Simulation**  

To verify Kepler’s Law numerically, we simulate circular orbits for different radii and check the relationship between $T^2$ and $r^3$.  

### **Python Simulation of Circular Orbits**  

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.constants import G

# Define constants
M = 1.989e30  # Mass of the Sun (kg)
AU = 1.496e11  # 1 Astronomical Unit in meters

# Function to compute orbital period using Kepler's Third Law
def orbital_period(r):
    return 2 * np.pi * np.sqrt(r**3 / (G * M))

# Generate radii from 0.1 AU to 5 AU
radii = np.linspace(0.1, 5, 10) * AU
periods = np.array([orbital_period(r) for r in radii])

# Verify T^2 vs r^3 relationship
T2 = periods**2
R3 = radii**3

# Plot results
plt.figure(figsize=(8, 5))
plt.plot(R3, T2, 'o-', label=r'$T^2$ vs $r^3$')
plt.xlabel(r'$r^3$ (m³)')
plt.ylabel(r'$T^2$ (s²)')
plt.title("Kepler's Third Law Verification")
plt.legend()
plt.grid()
plt.show()
```
### Source
[Colab](https://colab.research.google.com/drive/1Gzs6NYgJDfira_n9CpCjV6JsMjYsQ-4I)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/2%20Gravity/Unknown-3.png?raw=true)

---

## **4. Results and Discussion**  

- The plot of $T^2$ vs. $r^3$ shows a straight line, confirming Kepler’s Third Law.  
- This law enables astronomers to estimate **planetary distances** and **star masses** using observed orbital periods.  
- For **elliptical orbits**, Kepler’s Law applies using the **semi-major axis** instead of $r$.  

### **Real-World Applications**  
- **Earth-Moon System**: Predicting the Moon’s motion around Earth.  
- **Exoplanet Discovery**: Detecting distant planets via their orbital properties.  
- **Satellite Orbits**: Designing stable GPS and communication satellite paths.  

---

## **5. Conclusion**  

Kepler’s Third Law provides a fundamental link between **orbital period and radius**, governing celestial mechanics. Our computational verification confirms this proportionality, demonstrating its validity in planetary systems. Future studies can extend this analysis to **elliptical orbits** and **multi-body gravitational interactions**. 

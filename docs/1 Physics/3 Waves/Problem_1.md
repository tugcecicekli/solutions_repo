# Problem 1
# **Interference Patterns on a Water Surface**

## **1. Introduction**  
Wave interference occurs when multiple waves overlap, leading to **constructive** (amplification) and **destructive** (cancellation) interference. On a **water surface**, circular waves from different sources create **complex interference patterns**, which are influenced by the **number, position, and phase of sources**.

This study investigates **interference patterns from multiple point sources** arranged in **regular polygons**. Understanding such patterns has applications in **acoustics, optics, and fluid dynamics**.

---

## **2. Theoretical Background**  

### **2.1 Wave Equation for a Point Source**
A **circular wave** from a point source at position $\mathbf{r_i}$ is given by:

$$\psi_i(\mathbf{r}, t) = A \cos(k r_i - \omega t + \phi_0)$$

where:
- $\psi_i(\mathbf{r}, t)$ is the **displacement** at point $\mathbf{r}$ and time $t$.
- $A$ is the **amplitude**.
- $k = \frac{2\pi}{\lambda}$ is the **wave number**.
- $\omega = 2\pi f$ is the **angular frequency**.
- $r_i = |\mathbf{r} - \mathbf{r_i}|$ is the **distance** from the source.
- $\phi_0$ is the **initial phase**.

### **2.2 Superposition of Multiple Waves**
If there are $N$ wave sources at positions $\mathbf{r_i}$, the total displacement is:

$$\Psi(\mathbf{r}, t) = \sum_{i=1}^{N} A \cos(k r_i - \omega t + \phi_0)$$

The **interference pattern** results from this sum.

### **2.3 Regular Polygon Source Arrangement**
We place sources at the **vertices of a regular polygon** (e.g., **triangle, square, pentagon**) with center at the origin. Each vertex has coordinates:

$$\mathbf{r_i} = R (\cos \theta_i, \sin \theta_i)$$

where $\theta_i = \frac{2\pi i}{N}$, for $i = 0, 1, ..., N-1$.

---

## **3. Computational Simulation**

### **Python Implementation**
We simulate **wave interference** for sources placed at the vertices of a **regular polygon** using **NumPy and Matplotlib**.

```python
import numpy as np
import matplotlib.pyplot as plt

# Define wave parameters
A = 1  # Amplitude
lambda_wave = 1  # Wavelength
k = 2 * np.pi / lambda_wave  # Wave number
omega = 2 * np.pi  # Angular frequency
N = 5  # Number of sources (Regular pentagon)
R = 5  # Distance of sources from origin
grid_size = 200  # Resolution

# Define spatial grid
x = np.linspace(-10, 10, grid_size)
y = np.linspace(-10, 10, grid_size)
X, Y = np.meshgrid(x, y)

# Define source positions (vertices of polygon)
angles = np.linspace(0, 2*np.pi, N, endpoint=False)
sources = [(R * np.cos(angle), R * np.sin(angle)) for angle in angles]

# Compute wave superposition
wave_sum = np.zeros_like(X)

for (x0, y0) in sources:
    r = np.sqrt((X - x0)**2 + (Y - y0)**2)
    wave_sum += A * np.cos(k * r)

# Plot the interference pattern
plt.figure(figsize=(7, 7))
plt.imshow(wave_sum, extent=[-10, 10, -10, 10], cmap="RdBu", origin="lower")
plt.colorbar(label="Wave Displacement")
plt.scatter(*zip(*sources), color="black", marker="o", label="Wave Sources")
plt.title("Interference Pattern for a Regular Pentagon")
plt.xlabel("x")
plt.ylabel("y")
plt.legend()
plt.show()
```
https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-

---

## **4. Results and Discussion**  

- **Constructive Interference** occurs where waves reinforce each other, creating bright regions.
- **Destructive Interference** occurs where waves cancel, forming dark regions.
- The **polygon shape affects the pattern**, leading to **symmetric and repeating interference zones**.

### **Different Polygon Cases**
| Polygon  | Interference Behavior |
|----------|----------------------|
| **Triangle (N=3)**  | Large interference zones, simple symmetry. |
| **Square (N=4)**  | More complex interference fringes. |
| **Pentagon (N=5)** | Higher symmetry, intricate wave interactions. |

### **Real-World Applications**
- **Optics:** Interference of light waves in **holography** and **diffraction gratings**.
- **Acoustics:** Sound wave interference in **concert halls and speaker systems**.
- **Fluid Dynamics:** Wave interactions in **oceans and engineering**.

---

## **5. Conclusion**  

This report investigated **wave interference on a water surface** for **multiple sources** arranged in **regular polygons**. The **numerical simulation** confirmed that the **source arrangement** significantly influences **interference patterns**.

Future work could explore **nonlinear effects, wave damping, and 3D wave interactions**. 

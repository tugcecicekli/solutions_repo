# Problem 1

# Interference Patterns on a Water Surface

## **Motivation**

Wave interference explains how multiple wave sources interact. Water waves from different points (like stones dropped into a pond) create **constructive** (amplifying) and **destructive** (canceling) interference patterns. By simulating these with point sources at the vertices of polygons, we visually understand the superposition principle in 2D wave systems.

---

## **Wave Theory**

### Wave Equation for a Single Source:

A circular wave from a point $(x_0, y_0)$ is modeled as:

$$
z(x, y, t) = A \cdot \sin(kr - \omega t + \phi)
$$

Where:

* $A$: Amplitude
* $r = \sqrt{(x - x_0)^2 + (y - y_0)^2}$: Distance from source
* $k = \frac{2\pi}{\lambda}$: Wavenumber
* $\omega = 2\pi f$: Angular frequency
* $\phi$: Initial phase

### Superposition for $N$ Sources:

$$
Z(x, y, t) = \sum_{i=1}^{N} A \cdot \sin(k r_i - \omega t + \phi_i)
$$

---

## 1. Single Source Simulation 

```python
# Parameters
A = 1.0         # amplitude
λ = 2.0         # wavelength
f = 1.0         # frequency
ω = 2 * np.pi * f
k = 2 * np.pi / λ
phi = 0

# Grid
x = np.linspace(-10, 10, 500)
y = np.linspace(-10, 10, 500)
X, Y = np.meshgrid(x, y)

# Source
source = (0, 0)

# Distance from source
R = np.sqrt((X - source[0])**2 + (Y - source[1])**2)

# Wave function
def wave_frame(t):
    return A * np.sin(k * R - ω * t + phi)

# Static heatmap
plt.figure(figsize=(6, 6))
plt.imshow(wave_frame(0), extent=[-10, 10, -10, 10], cmap='RdBu', origin='lower')
plt.title('Single Source - Heatmap')
plt.colorbar(label='Amplitude')
plt.show()
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/3%20Waves/Unknown-31.png?raw=true)

Visit: [Colab](https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-#scrollTo=RE23Izc_flOo)


### Animation:

```python
fig, ax = plt.subplots(figsize=(6, 6))
im = ax.imshow(wave_frame(0), extent=[-10, 10, -10, 10], cmap='RdBu', origin='lower')
ax.set_title('Single Source Wave Animation')

def animate(t):
    im.set_array(wave_frame(t))
    return [im]

ani = animation.FuncAnimation(fig, animate, frames=np.linspace(0, 2, 60), interval=100, blit=True)
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-#scrollTo=RE23Izc_flOo)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/3%20Waves/Unknown-2.gif?raw=true)
---

## 2. Two Sources Interference

```python
sources = [(-3, 0), (3, 0)]

def interference_frame(t):
    total = np.zeros_like(X)
    for sx, sy in sources:
        R = np.sqrt((X - sx)**2 + (Y - sy)**2)
        total += A * np.sin(k * R - ω * t)
    return total

# Static Interference Heatmap
plt.figure(figsize=(6, 6))
plt.imshow(interference_frame(0), extent=[-10, 10, -10, 10], cmap='RdBu', origin='lower')
plt.title('Two Sources - Heatmap')
plt.colorbar(label='Amplitude')
plt.show()
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/3%20Waves/Unknown-32.png?raw=true)

Visit: [Colab](https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-#scrollTo=RE23Izc_flOo)

### Animation:

```python
fig, ax = plt.subplots(figsize=(6, 6))
im = ax.imshow(interference_frame(0), extent=[-10, 10, -10, 10], cmap='RdBu', origin='lower')
ax.set_title('Two Source Interference Animation')

def animate(t):
    im.set_array(interference_frame(t))
    return [im]

ani = animation.FuncAnimation(fig, animate, frames=np.linspace(0, 2, 60), interval=100, blit=True)
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-#scrollTo=RE23Izc_flOo)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/3%20Waves/Unknown-3.gif?raw=true)

---

## 3. Polygon Sources (Triangle, Square, Pentagon)

```python
def generate_polygon_sources(n, radius=5):
    return [(radius * np.cos(2 * np.pi * i / n),
             radius * np.sin(2 * np.pi * i / n)) for i in range(n)]

# Choose n = 3, 4, 5 for triangle, square, pentagon
sources = generate_polygon_sources(n=5)

def polygon_wave(t):
    total = np.zeros_like(X)
    for sx, sy in sources:
        R = np.sqrt((X - sx)**2 + (Y - sy)**2)
        total += A * np.sin(k * R - ω * t)
    return total

# Static
plt.figure(figsize=(6, 6))
plt.imshow(polygon_wave(0), extent=[-10, 10, -10, 10], cmap='RdBu', origin='lower')
plt.title('Interference from Pentagon Vertices')
plt.colorbar(label='Amplitude')
plt.show()
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/3%20Waves/Unknown-33.png?raw=true) 

Visit: [Colab](https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-#scrollTo=RE23Izc_flOo)

### Polygon Animation:

```python
fig, ax = plt.subplots(figsize=(6, 6))
im = ax.imshow(polygon_wave(0), extent=[-10, 10, -10, 10], cmap='RdBu', origin='lower')
ax.set_title('Polygon Source Interference Animation')

def animate(t):
    im.set_array(polygon_wave(t))
    return [im]

ani = animation.FuncAnimation(fig, animate, frames=np.linspace(0, 2, 60), interval=100, blit=True)
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-#scrollTo=RE23Izc_flOo)

---

## 3D Visualization

```python
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure(figsize=(10, 6))
ax = fig.add_subplot(111, projection='3d')
Z = polygon_wave(0)
ax.plot_surface(X, Y, Z, cmap='viridis', edgecolor='none')
ax.set_title("3D Interference from Polygon")
ax.set_zlim(-2, 2)
plt.show()
```
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/3%20Waves/Unknown-18.png?raw=true)

Visit: [Colab](https://colab.research.google.com/drive/1oZVOuy4Nx-Na9ILn-ozYxNjUFb3O9TK-#scrollTo=RE23Izc_flOo)

---

## Summary

| Case | Sources                  | Description                          |
| ---- | ------------------------ | ------------------------------------ |
| 1    | Single                   | Basic circular wave                  |
| 2    | Two                      | Shows constructive/destructive lines |
| 3    | Triangle/Square/Pentagon | Complex interference lattices        |


# Problem 1
## Investigating the Range as a Function of the Angle of Projection
# 1. Theoretical Foundation

## Derivation of the Equations of Motion
Projectile motion can be broken down into horizontal and vertical components. The two key equations governing the motion are:

1. Horizontal Motion (constant velocity):

   $x(t) = v_0 \cos(\theta) \cdot t$

2. Vertical Motion (under gravitational acceleration):
   
   $y(t) = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2$

Where:
- $v_0$ is the initial velocity,
- $\theta$ is the angle of projection,
- $g$ is the gravitational acceleration,
- $t$ is the time.

By solving the vertical motion equation for the time when the projectile returns to the ground, we can find the time of flight:

$t_{\text{flight}} = \frac{2 v_0 \sin(\theta)}{g}$

Using this in the horizontal motion equation, we find the range:

$$R = \frac{v_0^2 \sin(2\theta)}{g}$$

This equation represents the range of the projectile as a function of the initial velocity $v_0$, gravitational acceleration $g$, and launch angle $\theta$.


#### 1.2 Finding the Time of Flight
The range $R$ is the horizontal distance traveled by the projectile before it hits the ground. To find the time of flight, we solve the vertical motion equation for the time $t$ when $y(t) = 0$ (when the projectile returns to the ground).

$0 = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2$
This gives:
$t \left( v_0 \sin(\theta) - \frac{1}{2} g t \right) = 0$

The solutions are $t = 0$ (initial launch time) and:

$t_{\text{flight}} = \frac{2 v_0 \sin(\theta)}{g}$

#### 1.3 Finding the Range
The range $R$ is the horizontal distance the projectile travels when it reaches the ground. Using the time of flight $t_{\text{flight}}$ in the horizontal motion equation:

$R = v_0 \cos(\theta) \cdot t_{\text{flight}} = v_0 \cos(\theta) \cdot \frac{2 v_0 \sin(\theta)}{g}$

Simplifying:
$R = \frac{v_0^2 \sin(2\theta)}{g}$

Thus, the range is a function of the initial velocity $v_0$, the gravitational acceleration $g$, and the launch angle $\theta$.

# 2. Analysis of the Range

## Range vs Angle of Projection

The horizontal range $R$ is a function of the launch angle $\theta$. The range is maximized when $\theta = 45^\circ$, as this angle maximizes the value of $\sin(2\theta)$.

## Effect of Initial Velocity

The range is proportional to the square of the initial velocity $v_0$. Doubling the initial velocity will increase the range by a factor of four.

## Effect of Gravitational Acceleration

The range is inversely proportional to gravitational acceleration $g$. A higher value of $g$ results in a shorter range. For example, the range on the Moon would be much greater than on Earth due to the lower gravitational acceleration.


# 3. Practical Applications

## Uneven Terrain

In real-world situations, such as launching a projectile on uneven terrain, the initial and final heights may differ. To account for this, the vertical motion equation needs to be adjusted to incorporate the height difference between launch and landing.

## Air Resistance

The idealized model assumes no air resistance. However, in reality, air resistance will slow the projectile down, affecting its trajectory and range. This requires more complex equations and numerical methods to model.

## Sports and Engineering

Understanding projectile motion is crucial in sports like soccer, football, and basketball. Engineers also use projectile motion principles in launching projectiles and rockets.

# 4. Implementation 
## Source
[Colab] (https://colab.research.google.com/drive/1ituJ1v7ZvE1DFsCsE_xxTT5jaNQ_-kp1)

# 5. Limitations of the Idealized Model

The idealized model assumes:
1. No air resistance.
2. A flat surface for launch and landing.
3. Constant gravitational acceleration.

Real-world conditions, such as air resistance, uneven terrain, and varying gravity, are not accounted for in this simple model. To improve the accuracy, air resistance and other factors must be included.

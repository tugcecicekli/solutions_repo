# Problem 1
## 1. Theoretical Background

### What is the Central Limit Theorem?

> The **Central Limit Theorem (CLT)** is a fundamental result in statistics that explains why the normal distribution arises so frequently in practice. It states:
>
> > *Given a population with any shape distribution (with finite mean μ and variance σ²), the distribution of the sample means (𝑋̄) of sufficiently large random samples will approach a normal distribution as the sample size (n) increases.*

This happens **regardless of the shape of the original distribution** — it could be skewed, uniform, or binomial — and yet the **distribution of the sample means becomes approximately normal** for large enough sample sizes.

---

## 2. Key Formulas

Let $X_1, X_2, \dots, X_n$ be independent and identically distributed (i.i.d.) random variables from some population with:

* Mean $\mu = E[X_i]$
* Variance $\sigma^2 = Var(X_i)$

Then, the **sample mean** is:

$$
\bar{X} = \frac{1}{n} \sum_{i=1}^{n} X_i
$$

The **expected value and variance** of the sample mean are:

* $E[\bar{X}] = \mu$
* $Var(\bar{X}) = \frac{\sigma^2}{n}$

According to the CLT:

$$
\frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \xrightarrow{d} \mathcal{N}(0, 1) \quad \text{as } n \to \infty
$$

---

## 3. Understanding Through Simulations

We will simulate the CLT using three different population distributions:

1. **Uniform Distribution**
2. **Exponential Distribution**
3. **Binomial Distribution**

For each distribution:

* We generate a large population.
* Draw multiple random samples.
* Calculate the sample mean for different sample sizes.
* Plot the **sampling distribution** of the sample mean.

---

## 4. Python Implementation

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

sns.set(style="whitegrid")

# Settings
population_sample_size = 100000
num_samples = 1000
sample_sizes = [5, 10, 30, 50]
```

---

### Population Generators

```python
# Define distributions
population_distributions = {
    'Uniform(0,1)': lambda size: np.random.uniform(0, 1, size),
    'Exponential(λ=1)': lambda size: np.random.exponential(scale=1.0, size=size),
    'Binomial(n=10, p=0.5)': lambda size: np.random.binomial(n=10, p=0.5, size=size)
}
```

---

### Function to Plot Sampling Distribution

```python
def plot_sampling_distribution(dist_name, generator):
    population = generator(population_sample_size)
    
    fig, axes = plt.subplots(1, len(sample_sizes), figsize=(20, 4))
    fig.suptitle(f'Sampling Distribution of Sample Mean ({dist_name})', fontsize=16)

    for idx, n in enumerate(sample_sizes):
        sample_means = [np.mean(generator(n)) for _ in range(num_samples)]
        sns.histplot(sample_means, kde=True, bins=30, ax=axes[idx], color="skyblue")
        axes[idx].set_title(f'Sample size n = {n}')
        axes[idx].set_xlabel('Sample Mean')
        axes[idx].set_ylabel('Frequency')

    plt.tight_layout()
    plt.show()
```

---

### Generate Sampling Distributions

```python
for name, gen in population_distributions.items():
    plot_sampling_distribution(name, gen)
```
Visit: [Colab](https://colab.research.google.com/drive/1o0TQMxKll0IjxXCilOuSTzD5PICTyxrn#scrollTo=_Oklt7YVDRqV)

![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/6%20Statistics/Unknown-611.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/6%20Statistics/Unknown-612.png?raw=true)
![Example Image](https://github.com/tugcecicekli/solutions_repo/blob/main/docs/1%20Physics/6%20Statistics/Unknown-613.png?raw=true)

---

## 5. Animation: How the Sample Mean Converges

We animate the build-up of sample means from the **Exponential distribution**.

```python
# Animation for Exponential Distribution
sample_means = []

fig, ax = plt.subplots(figsize=(6, 4))

def animate(i):
    ax.clear()
    sample = np.random.exponential(scale=1.0, size=30)
    sample_means.append(np.mean(sample))
    sns.histplot(sample_means, kde=True, bins=30, ax=ax, color="orange")
    ax.set_xlim(0, 3)
    ax.set_ylim(0, 150)
    ax.set_title(f'Step {i+1}: CLT in Action')

ani = FuncAnimation(fig, animate, frames=100, repeat=False)
plt.close()
HTML(ani.to_jshtml())
```
Visit: [Colab](https://colab.research.google.com/drive/1o0TQMxKll0IjxXCilOuSTzD5PICTyxrn#scrollTo=_Oklt7YVDRqV)


---

## 6. In-Depth Discussion

### Influence of Distribution Shape

| Distribution     | Shape        | CLT Behavior                         |
| ---------------- | ------------ | ------------------------------------ |
| Uniform(0,1)     | Flat         | Fast convergence                     |
| Exponential(λ=1) | Skewed right | Slower convergence                   |
| Binomial(10,0.5) | Discrete     | Moderate convergence, quickly normal |

* The **more skewed** or **discrete** the population, the **larger the sample size** needed for normality.
* All converge eventually, confirming the **universality of the CLT**.

### Influence of Variance

The spread (standard deviation) of the sample means depends on:

$$
\text{Standard deviation of the mean} = \frac{\sigma}{\sqrt{n}}
$$

→ **Larger sample sizes yield tighter, less variable distributions**.

---

## 7. Real-World Applications of the CLT

### Manufacturing

* **Quality control**: CLT allows estimating the defect rate from a sample.
* Example: Testing 50 widgets out of 1000 to assess quality.

### Finance

* Predicting **expected return** on portfolios over time.
* Modeling **risk** by sampling past performance.

### Medicine & Biology

* Evaluating average effects of drugs on patient groups.
* Using **sample averages** in clinical trials to infer **population behavior**.

### Polling & Surveys

* Estimating **population opinion** from a representative sample.
* Used in **election forecasting** and public opinion research.

---

## 8. Summary

| Concept                  | Explanation                                                           |
| ------------------------ | --------------------------------------------------------------------- |
| Central Limit Theorem    | Sample means approximate normal distribution as sample size increases |
| Sample Mean $\bar{X}$    | $\bar{X} = \frac{1}{n} \sum X_i$                                      |
| Variance of Sample Mean  | $\frac{\sigma^2}{n}$                                                  |
| Simulation Distributions | Uniform, Exponential, Binomial                                        |
| Visualization            | Histograms, KDEs, Animation                                           |
| Key Insight              | Distribution shape and sample size influence convergence to normality |

---


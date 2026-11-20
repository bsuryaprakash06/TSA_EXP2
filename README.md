# Linear and Polynomial Trend Estimation using Least Squares

A first-principles mathematical implementation of deterministic secular trend estimation using closed-form Ordinary Least Squares (OLS) normal equations, comparing linear and polynomial curve-fitting formulations on aggregated annual temperature data.

---

## Objective

Isolate the underlying long-term secular trend from high-frequency annual fluctuations. The program resamples continuous daily observations to yearly aggregates and applies orthogonalized Least Squares normal equations to fit both linear (1st degree) and quadratic (2nd degree) trend functions.

---

## Dataset Utilized

- **Dataset Name**: Daily Minimum Temperatures in Melbourne
- **Filename**: `daily-min-temperatures.csv`
- **Domain**: Climatological Time Series
- **Aggregation**: Daily measurements resampled to annual calendar means (`Temp.resample('YE').mean()`), yielding yearly average minimum temperatures across 10 years (1981–1990).
- **Target Variable**: Annual Average Minimum Temperature (°C).

---

## Mathematical Formulation

To prevent numerical instability and collinearity during power calculations, time points are centered relative to the midpoint year $t_{\text{mid}}$:

$$X_i = t_i - t_{\text{mid}} \quad \implies \quad \sum_{i=1}^n X_i = 0$$

### 1. Linear Trend Model ($y = a + bx$)

Minimizing the sum of squared residuals $\sum (y_i - (a + b X_i))^2$ yields the closed-form normal equations:

$$a = \frac{\sum y_i}{n} = \bar{y}$$

$$b = \frac{n \sum X_i y_i - \sum X_i \sum y_i}{n \sum X_i^2 - (\sum X_i)^2} = \frac{\sum X_i y_i}{\sum X_i^2}$$

### 2. Quadratic Polynomial Model ($y = a + bx + cX^2$)

Minimizing the sum of squared residuals for the quadratic model yields a system of three normal equations:

$$\sum y = n a + b \sum X + c \sum X^2$$

$$\sum X y = a \sum X + b \sum X^2 + c \sum X^3$$

$$\sum X^2 y = a \sum X^2 + b \sum X^3 + c \sum X^4$$

In matrix form $\mathbf{A} \mathbf{\beta} = \mathbf{Y}$:

```math
\begin{bmatrix}
n & \sum X & \sum X^2 \\
\sum X & \sum X^2 & \sum X^3 \\
\sum X^2 & \sum X^3 & \sum X^4
\end{bmatrix}
\begin{bmatrix}
a \\
b \\
c
\end{bmatrix}
=
\begin{bmatrix}
\sum y \\
\sum X y \\
\sum X^2 y
\end{bmatrix}
```

Coefficients $[a, b, c]^T$ are computed via direct linear algebra matrix decomposition (`np.linalg.solve`).

---

## Results & Visualizations

### Linear Trend Estimation
<img width="855" height="470" alt="Linear Trend of Average Daily Minimum Temperatures" src="https://github.com/user-attachments/assets/315b1e10-f769-4cb1-83cb-674d962466a4" />

### Polynomial Trend Estimation
<img width="855" height="470" alt="Polynomial Trend of Average Daily Minimum Temperatures" src="https://github.com/user-attachments/assets/42cd116a-76de-475c-85ca-482fa5e09e40" />

---

## License

This project is licensed under the GNU General Public License v3.0. See the [LICENSE](LICENSE) file for details.

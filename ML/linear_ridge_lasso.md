
# Linear Regression, Ridge Regression, and Lasso Regression

## 1. Linear Regression

### 1.1 Definition
Linear regression models the relationship between an independent variable (features) \( X \) and a dependent variable (target) \( y \) by fitting a **linear equation**.

\[
y \approx \hat{y} = X\beta + \epsilon
\]

Where:
- \( X \): \(n \times p\) matrix of input features
- \( \beta \): \(p \times 1\) vector of coefficients
- \( \epsilon \): error term

---

### 1.2 Objective Function (Ordinary Least Squares - OLS)

We minimize the **sum of squared residuals**:

\[
J(\beta) = \sum_{i=1}^{n} (y_i - x_i^T\beta)^2
\]

In matrix form:

\[
J(\beta) = (y - X\beta)^T (y - X\beta)
\]

---

### 1.3 Derivation of OLS Solution

1. **Expand the loss function**:
\[
J(\beta) = y^T y - 2\beta^T X^T y + \beta^T X^T X \beta
\]

2. **Differentiate w.r.t. \(\beta\)**:
\[
\frac{\partial J}{\partial \beta} = -2 X^T y + 2 X^T X \beta
\]

3. **Set derivative to zero**:
\[
X^T X \beta = X^T y
\]

4. **Solve for \(\beta\)**:
\[
\hat{\beta} = (X^T X)^{-1} X^T y
\]

> **Condition**: \(X^T X\) must be invertible (full rank). If multicollinearity exists, \(X^T X\) is singular → no unique solution.

---

### 1.4 Limitations of OLS
- **Overfitting** when features are many (\(p \approx n\))
- **Multicollinearity** → unstable coefficients
- **No regularization** → large coefficients possible

---

## 2. Ridge Regression (L2 Regularization)

### 2.1 Idea
Add a **penalty on the squared magnitude of coefficients** to prevent large weights.

---

### 2.2 Objective Function

\[
J(\beta) = \underbrace{\sum_{i=1}^n (y_i - x_i^T \beta)^2}_{\text{OLS loss}} + \lambda \sum_{j=1}^p \beta_j^2
\]

Matrix form:

\[
J(\beta) = (y - X\beta)^T(y - X\beta) + \lambda \beta^T \beta
\]

Here:
- \(\lambda \ge 0\) is the **regularization strength**.
- Larger \(\lambda\) → more shrinkage.

---

### 2.3 Derivation of Ridge Solution

1. Differentiate:
\[
\frac{\partial J}{\partial \beta} = -2X^T y + 2 X^T X \beta + 2\lambda \beta
\]

2. Set to zero:
\[
(X^T X + \lambda I) \beta = X^T y
\]

3. Solve:
\[
\hat{\beta}_{ridge} = (X^T X + \lambda I)^{-1} X^T y
\]

> **Key**: \(X^T X + \lambda I\) is **always invertible** if \(\lambda > 0\) → solves multicollinearity.

---

### 2.4 Effects of Ridge
- Shrinks coefficients **towards zero** but never exactly zero.
- Reduces variance → improves generalization.
- Good when many correlated predictors exist.

---

## 3. Lasso Regression (L1 Regularization)

### 3.1 Idea
Add a penalty on the **absolute value of coefficients** to force some coefficients exactly **to zero** → **feature selection**.

---

### 3.2 Objective Function

\[
J(\beta) = \sum_{i=1}^n (y_i - x_i^T \beta)^2 + \lambda \sum_{j=1}^p |\beta_j|
\]

---

### 3.3 Why Lasso Sets Coefficients to Zero (Geometric Intuition)

- L1 penalty constraint forms a **diamond-shaped** region.
- OLS loss contours are ellipses.
- The optimal point often hits **a corner** of the diamond → some coefficients exactly zero.
- This makes Lasso a **sparse model**.

---

### 3.4 Mathematical Derivation (Not closed form)

Unlike Ridge, Lasso has no closed-form solution because \(|\beta|\) is **non-differentiable** at 0.

Instead, it’s solved by:
- **Coordinate Descent**
- **LARS (Least Angle Regression)**

For one parameter in **Coordinate Descent**, the update is:

\[
\beta_j \leftarrow S\left( \frac{\sum_{i=1}^n x_{ij}(y_i - \hat{y}_{-j})}{n}, \frac{\lambda}{n} \right)
\]
where \(S(z, \gamma) = \text{sign}(z) \cdot \max(|z| - \gamma, 0)\) is the **soft-thresholding function**.

---

### 3.5 Effects of Lasso
- Performs **automatic feature selection**.
- Good when only a subset of features are relevant.
- Can be unstable if predictors are highly correlated (Ridge handles this better).

---

## 4. Comparison Table

| Feature | Linear Regression | Ridge Regression | Lasso Regression |
|---------|-------------------|------------------|------------------|
| Regularization | None | L2 (squared magnitude) | L1 (absolute value) |
| Coefficients | No shrinkage | Shrunk, not zero | Shrunk, some exactly zero |
| Feature Selection | No | No | Yes |
| Multicollinearity handling | Poor | Good | Moderate |
| Closed-form solution | Yes | Yes | No |
| Main use | Interpretability, baseline | Multicollinearity, generalization | Sparse model, feature selection |

---

## 5. Geometric Intuition

- **OLS** → intersection of loss ellipse with entire coefficient space.
- **Ridge** → loss ellipse intersecting L2 ball (circle) → smooth shrinkage.
- **Lasso** → loss ellipse intersecting L1 diamond → sparsity.

---

## 6. When to Use
- **Linear Regression**: When features are few, no multicollinearity, and interpretability is key.
- **Ridge**: When all features are relevant but may be correlated.
- **Lasso**: When many features are irrelevant → want automatic selection.

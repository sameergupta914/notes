#Linear Regression

- Imagine you have some data points and you want to draw a straight line that best fits them.
That line lets you predict a number (target) from some input(s).
Example: predicting salary from years of experience, or house price from size.

Simple case: one input and one output.

- The model is: y=mx+c

- How it learns the line
We pick the line so that the predictions are as close as possible to the real answers.
The “closeness” is measured by squared differences (errors).
We adjust the slope(s) and intercept to make the total error as small as possible.

Two common ways to find those:

Formula (closed-form): directly compute the best coefficients using matrix math.

Gradient descent: start with a guess and slowly improve it by stepping in the direction that reduces error.

- Example: Easy way with scikit-learn

    from sklearn.linear_model import LinearRegression
    import numpy as np

    np.random.seed(0)
    X = np.random.rand(100, 1)
    y = 3 + 2 * X[:, 0] + 0.1 * np.random.randn(100)

    model = LinearRegression()
    model.fit(X, y)
    print("Intercept:", model.intercept_)
    print("Coefficient:", model.coef_[0])




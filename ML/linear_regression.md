Linear Regression (OLS)
What it is:
A straight-line (or hyperplane) fit from features to a numeric target. It tries to make predictions close to actual values on average.

Use cases (pick when):

You want a strong baseline with clear interpretability.

Features aren’t too many and aren’t heavily correlated.

You care about unbiased estimates and simple inference.

Cost function (idea, no formulas):

Measure how far predictions are from the true values (square the errors so big misses hurt more).

Add them up. That’s the thing you minimize.

Gradient descent (how to fit it):

Initialize all weights (coefficients) to small values (often zeros).

Loop:

Predict for all training examples.

Compute residuals (prediction − truth).

Nudge each weight in the direction that reduces the total squared error (the “negative gradient” direction).

Stop when changes are tiny or after a set number of passes.

Practical: standardize features for stability; tune learning rate; don’t forget an intercept term.

Ridge Regression (L2)
What it is:
Linear regression with shrinkage. It discourages large weights by adding a penalty on their squared size. This stabilizes the model when features are correlated or you have many of them.

Use cases (pick when):

Many correlated features (multicollinearity).

You suspect all features contain some signal, but you want to control variance.

You have more features than rows (p > n) and need a unique, stable solution.

Cost function (idea):

Same squared-error piece as linear regression plus

A penalty that grows with the sum of squares of the weights (bigger weights → bigger penalty).

A knob λ controls how hard you penalize: higher λ ⇒ stronger shrinkage.

Gradient descent (how to fit it):

Do the usual OLS gradient step and apply “weight decay”: every step slightly pulls each weight toward zero, proportional to its size and the penalty strength.

Practically this looks like: update from error, then shrink weights a bit.

Tips: standardize features; do not penalize the intercept; choose λ via cross-validation.

Lasso (L1)
What it is:
Linear regression with a penalty on the absolute size of weights. This tends to push some weights exactly to zero → feature selection built in.

Use cases (pick when):

You think only a subset of features truly matter (sparsity).

You want an interpretable model that automatically drops irrelevant features.

You have lots of features and need a compact subset.

Cost function (idea):

Same squared-error piece plus

A penalty that grows with the sum of absolute values of the weights.

The shape of this penalty encourages exact zeros, unlike Ridge.

Gradient descent (how to fit it):

Plain gradient descent is tricky because the absolute-value penalty has a sharp corner at zero.

Two practical routes:

Subgradient/Proximal Gradient (soft-thresholding):

Take a normal gradient step on the squared error.

Then apply a “shrink and clip” operation to each weight: pull it toward zero by a fixed amount; if it crosses zero, set it to exactly zero. (That’s why Lasso creates sparsity.)

Coordinate Descent (often fastest):

Update one weight at a time while holding others fixed, repeatedly cycling through features; each update is the same “shrink and clip” idea.

Tips: standardize features; don’t penalize the intercept; choose λ via cross-validation. With highly correlated features, Lasso may pick one and drop the rest (can be unstable)—consider Elastic Net if that’s a problem.

Quick “When to Use What”
Linear Regression: small/clean feature sets, you want a simple, interpretable baseline.

Ridge: many or correlated features; you want stability and better generalization without dropping features.

Lasso: lots of features; you want feature selection and a sparse, interpretable model.

Gradient Descent Cheatsheet (all three)
Initialize weights; scale features (very important for Ridge/Lasso).

Pick a learning rate (start small; consider decay or use an optimizer like Adam for convenience).

Update rule each step:

Move opposite the gradient of the squared-error part.

Ridge: also “shrink” weights toward zero (weight decay).

Lasso: after the error step, apply soft-thresholding (shrink by a fixed amount; clip to zero if you cross it).

Stop when the validation score stops improving or parameter changes get tiny.

Tune λ (penalty strength) via cross-validation.
# econ3916-lab16-regularization

# Regularization: Buying Bias to Sell Variance

## Objective

I tested how LASSO and Ridge penalties trade a little bias for a large drop in variance when there are more predictors than training rows.

## Methodology

- Built a simulated design with 200 predictors, only 5 of them real, and 105 training rows.
- Fitted OLS, LASSO and Ridge, and compared their test MSE.
- Traced the LASSO path to see the order in which variables enter.
- Chose the penalty by 5-fold cross-validation, with the scaler fitted inside every fold.
- Scored LASSO's selection against the known truth.
- Gave LASSO and Ridge two near-duplicate predictors to see how each splits the credit.
- Wrote my own prompt asking an AI for the cross-validation loop, then checked its lambda and CV MSE against my `GridSearchCV` result.

## Key Findings

| Model | Test MSE |
|-------|----------|
| OLS   | 12.9587  |
| Ridge | 10.1128  |
| LASSO | 1.4219   |

OLS fits the training rows exactly but generalizes badly. LASSO beats Ridge by a wide margin because the true model is sparse: LASSO's penalty zeroes out coefficients, while Ridge only shrinks them.

- **LASSO path:** all five true predictors were in by step 6 of 60. The first noise variable entered at step 17.
- **Cross-validation:** lambda = 0.1995, CV MSE = 1.5502.
- **Selection against the truth:** LASSO found 5 of 5 true predictors, with 11 false positives.
- **Near-duplicate predictors:**

  | Model | Var 0 | Var 5 |
  |-------|-------|-------|
  | LASSO | 4.473 | 0.000 |
  | Ridge | 1.638 | 1.629 |

- **AI-written CV loop:** it took 2 rounds. The first failed on an undefined seed name. The second matched my results exactly:

  | Penalties | Lambda | CV MSE |
  |-----------|--------|--------|
  | All 21 | 0.1995 | 1.5502 |
  | Every second one | 0.1585 | 1.5810 |

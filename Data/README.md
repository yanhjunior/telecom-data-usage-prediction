# Telecom data usage prediction

Can you guess how much mobile data a customer uses just by looking at the price of their plan? That's the question behind this project. I used a simple linear regression to predict monthly data usage (in GB) from the plan price, then looked at where the model works and where it falls short.

## Data

The dataset (`clients.csv`) contains telecom customers with their seniority, plan price, data usage, support calls, late payments and whether they churned. For this project I only used two columns:

- `forfait`: monthly plan price (€), the input
- `conso_data`: monthly data usage (GB), the target

## What the notebook does

1. Loads the data and plots plan price against data usage
2. Sets a baseline: predicting the average usage for everyone
3. Fits a linear regression and interprets the coefficients
4. Evaluates the model on a 70/30 train/test split
5. Discusses the model's limits and how to improve it

## Results

The fitted line is roughly:

```
data_usage ≈ 1.23 × plan_price − 20.24
```

So each extra euro on the plan goes with about 1.23 GB more usage.

| Model | RMSE (GB) | R² |
|---|---|---|
| Baseline (mean) | 27.02 | 0 |
| Linear regression (train) | 15.73 | 0.62 |
| Linear regression (test) | 17.78 | 0.65 |

The regression cuts the error by about 40% compared to the baseline, and train and test scores are close, so there's no overfitting. For example, a customer on a €45 plan is predicted to use around 35 GB.

## Limitations

The model is better than guessing, but an error of 16 to 18 GB is still a lot for individual predictions. A few reasons:

- The intercept is negative, so the model predicts negative usage for plans under about €16.40, which makes no sense.
- The spread of the data grows with the plan price. Customers on expensive plans have very different usage, while linear regression assumes the error stays roughly constant.
- One variable isn't enough to explain how much data someone uses.

## Next steps

- Plot the residuals to confirm the growing spread
- Predict `log(1 + conso_data)` instead of the raw value
- Move to a multiple regression by adding `anciennete`, `appels_support` and `retards`

## Run it

```bash
pip install pandas matplotlib scikit-learn jupyter
jupyter notebook prediction_conso_data.ipynb
```

Make sure `clients.csv` is in the same folder as the notebook.
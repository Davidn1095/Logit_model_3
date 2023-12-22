# Logit_model_3

This code performs a comprehensive logistic regression analysis, starting from data preparation, fitting multiple models, selecting the best model based on stepwise procedures, evaluating model fit, and conducting ROC curve analysis. It represents a typical workflow in statistical modeling for binary outcomes.

### Reading and Preparing the Data:

- `Chapman <- read.csv('CHAPMAN.DAT', header=F, sep='')`: This command reads a CSV file named 'CHAPMAN.DAT' into a DataFrame called Chapman. `header=F` indicates that the file does not have a header row, and `sep=''` suggests that the CSV file might have a unique separator or is space-delimited.
- `names(Chapman) <- c("Id","Edad","Sistolica","Diastolica", "Colesterol","Altura","Peso","Coronarios")`: This sets the column names of the Chapman DataFrame to the specified values.
- `head(Chapman)`: Displays the first few rows of the DataFrame for a quick overview of the data.

### Full Logistic Regression Model:

- `Ajuste.all <- glm(Coronarios ~ Edad + Sistolica + Diastolica + Colesterol + Altura + Peso, family=binomial, data=Chapman)`: This fits a logistic regression model (glm) with Coronarios as the dependent variable and all other variables as predictors. The `family=binomial` argument indicates it's a binary logistic regression.
- `summary(Ajuste.all)`: Summarizes the full model, showing coefficients, statistical significance, etc.

### Null Model for Comparison:

- `Ajuste.0 <- glm(Coronarios ~ 1, family=binomial, data=Chapman)`: Fits a null logistic regression model with only the intercept (no predictors).
- `summary(Ajuste.0)`: Summarizes the null model.

### Stepwise Model Selection:

- `Ajuste.step <- step(Ajuste.0, scope=list(lower=Coronarios~1, upper=Coronarios~Edad+Sistolica+Diastolica+Colesterol+Altura+Peso), direction="both")`: Performs a stepwise model selection starting from the null model (Ajuste.0). It considers adding or removing variables between the lower (only intercept) and upper (all variables) scopes. `direction="both"` allows both forward and backward steps.
- `summary(Ajuste.step)`: Summarizes the selected model from the stepwise procedure.

### Confidence Intervals:

- `exp(confint.default(Ajuste.step))`: Calculates the confidence intervals for the model parameters and exponentiates them, which is common for logistic regression to obtain odds ratios.

### Goodness-of-Fit Test:

- `Bondad.Hosmer.Step <- hoslem.test(Chapman$Coronarios, fitted.values(Ajuste.step))`: Performs the Hosmer-Lemeshow test to assess the goodness-of-fit of the stepwise model.
- `Bondad.Hosmer.Step`: Displays the test results.

### Residuals and Influence Measures:

- Various functions (`rstandard(Ajuste.step, type="pearson")`, `rstandard(Ajuste.step, type="deviance")`, `cooks.distance(Ajuste.step)`) are used to compute residuals and influence measures, important for diagnosing model fit and identifying influential data points.

### Binary Classification and Confusion Matrix:

- `Categoria.Pred <- ifelse(fitted.values(Ajuste.step) >= 0.5, 1, 0)`: Creates a binary classification based on the predicted probabilities from the stepwise model.
- `table(Chapman$Coronarios, Categoria.Pred)`: Generates a confusion matrix comparing actual and predicted classifications.

### ROC Curve Analysis:

- `CurvaROC <- roc(Chapman$Coronarios, fitted.values(Ajuste.step))`: Calculates the Receiver Operating Characteristic (ROC) curve using the roc function.
- `plot(CurvaROC)`: Plots the ROC curve, which is a graphical representation of the diagnostic ability of a binary classifier system.

### Stepwise Model with Interaction Terms:

- `Ajuste.Step.Inter <- step(Ajuste.step, scope=list(lower=Coronarios~Edad+Peso+Colesterol, upper=Coronarios~Edad*Peso*Colesterol), direction="both")`: Performs another stepwise model selection, this time considering interaction terms between Edad, Peso, and Colesterol.
- `summary(Ajuste.Step.Inter)`: Summarizes the resulting model with interaction terms.


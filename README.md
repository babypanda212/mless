# README — MLESS HW2

Ozone Forecasting with Time Series Models
Name: Ayesha Khan

## Task 1 — Univariate Forecasting

In this task I used the forecasting models shown in class for inference using the provided air quality dataset:

* **SARIMA (Notebook 3)**
* **MLP (Notebook 4)**
* **LSTM (Notebook 5)**
* **PatchTST (Notebook 6)**

The goal was to forecast temperature values.

### What I Did

For SARIMA, I followed the suggestions from Sindhu and reduced the time range to be able to load test data for inference. The SARIMAX model was trained on part of the dataset to predict temperature, but I had problems later when comparing results to the ML models because they used a different dataset slice.

For the ML models, I used checkpointed versions to save training time and only did inference to generate forecasts. However, because the training data is saved in pickle files and I don't know how to get it into a dataframe, I couldn't properly align the results with SARIMA for consistent plots.

### Observations

**SARIMA:** Shows correct warming trend over 96 hours, but underestimates peaks and lags behind the true data.

**LSTM:** Captures temperature oscillations and predicts peak events better than SARIMA.

**MLP:** Had trouble tracking peaks, sometimes underestimating or overestimating them.

I also noticed the `create_sequences` function in Notebook 3 says "past 24 hours" and "next 6 hours" in the comments, but the actual code uses 336 and 96 steps (which is 2 weeks and 4 days??) — not sure if that’s intentional or I misunderstood.

### Other Struggles

* Ran into memory problems on Colab, especially when trying to load the full dataset for both context and future windows.
* Lost work several times because I opened new notebooks in the same Colab tab to reference code.
* Fell behind because I started late (my fault), and by the time I got to PatchTST, I lost Colab compute entirely.

## Task 2 — Multivariate Forecasting

The goal was to forecast ozone based on both temperature and past ozone values using the MLP model from Task 1.

### What I Did

Tried using the default `create_sequences` function but kept getting empty arrays (zero windows). Eventually realized the dataset had duplicate timestamps, and it broke the continuity check.

A classmate suggested dropping duplicates per hour — I did that and managed to generate sequences, but I am not confident this is the "correct" solution. I am thinking it might shift the data distribution wrongly.

Once sequences were generated, I flattened the multivariate input and modified the MLP to handle two input variables. No compute to train and evaluate the model, will run later.

Next time I would definitely start earlier, I underestimated the struggle of Colab.

## Task 3 - Incorporating Future Forecasts


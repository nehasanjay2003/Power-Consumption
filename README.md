# Electric Power Consumption Forecasting

This project predicts electricity usage in three zones based on historical power consumption and environmental features such as temperature, humidity, wind, and sunlight.

## Dataset
- **Rows:** 52,416 (10-minute intervals)  
- **Columns:**
  - `Datetime` → timestamp of measurement  
  - `Temperature`, `Humidity`, `WindSpeed`, `GeneralDiffuseFlows`, `DiffuseFlows` → input features  
  - `PowerConsumption_Zone1`, `Zone2`, `Zone3` → targets to forecast  

## Objective
Forecast power consumption to help plan electricity usage, reduce wastage, and improve energy management.

## Approach
1. **Data Preprocessing**
   - Convert `Datetime` to datetime type and set as index
   - Extract time features: hour, day, weekday, month
   - Check for missing values

2. **Feature Engineering**
   - Original features: Temperature, Humidity, WindSpeed, GeneralDiffuseFlows, DiffuseFlows  
   - **Lag features:** previous 1, 2, 3 time steps of power consumption  
   - **Rolling averages:** 3, 6, 12-step moving averages of past consumption  

3. **Train/Test Split**
   - Split 80% training / 20% testing based on time  

4. **Model Training**
   - Trained **XGBoost models** separately for each zone  
   - Hyperparameters optimized using **RandomizedSearchCV**  

5. **Evaluation**
   - Metrics used: MSE, RMSE, MAE, R²  
   - Plotted actual vs predicted power consumption  

6. **Multi-Step Forecasting**
   - Recursive prediction for next `N` steps  
   - Lag and rolling features updated using predicted values  
   - Time features updated automatically  

## Results

| Zone | MSE       | RMSE    | MAE     | R²   |
|------|-----------|---------|---------|------|
| Zone 1 | 500,532.27 | 707.48  | 529.77  | 0.99 |
| Zone 2 | 200,836.13 | 448.15  | 308.75  | 0.99 |
| Zone 3 | 289,516.21 | 538.07  | 369.28  | 0.97 |

**Interpretation:**  
- All zones show **high accuracy** with R² values 0.97–0.99.  
- RMSE and MAE are reasonably low, meaning predictions are close to actual values.  
- Zone 2 has the most precise predictions, Zone 1 slightly higher errors, and Zone 3 slightly lower R².  
- Lag and rolling features significantly improved model performance.



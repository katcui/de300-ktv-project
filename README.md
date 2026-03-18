# DE300 Project: Early Warning Water Quality Risk Detection System and Forecasting

## Project Roadmap:

1. **Goal**: Building a system that can _detect early warning signals of water quality degradation_ by forecasting chemical levels 3 months into the future.
   - Identifies locations where trends indicate potential future risk.
   - **Output**: A risk score per location predicting future degradation.
2. **Location Selection and Feasibility Analysis**: Test multiple locations (large cities, cities with known degradation over time, cities with known poor water quality) --> determine feasibility for model
   - Evaluate based on time span of data, sampling frequency, data density/sparsity, presence of long-term trends, and seasonal patterns
   - Identify 10 locations that provide sufficient data to support our forecasting model

3. **Data Ingestion Pipeline**: From the _Water Quality Portal (WQP) webservice API_, collect relevant data.
   - **Pipeline**:
     - query WQP API
     - retrieve measurements for selected locations
     - store raw data (S3 or locally)

4. **Data Cleaning and Standardization**
   - complete unit normalization for unique measurement types
   - aggregate measurements into monthly averages
   - identify gaps in handling
     - forward fill short gaps

5. **Exploratory Data Analysis (EDA)**L understand how water quality evolves over time
   - **Key Analyses**: time series visualization, chemical levels per location over time, trend detection

6. **Feature Engineering**: convert time series into model-ready features
   - **Features**:
     - previous chemical levels (_lag features_)
     - rolling averages (3 months)
     - month of year (seasonality)

7. **Forecasting Model (Trend Prediction)**
   - train a global model across multiple locations
   - all locations contribute training examples
   - each _training example_:
     - input last X months of data, output predicted measurement at t+3 months
   - **Lookback Window**: 12-24 months of historical data
   - **Prediction Target**: chemical level in 3 months
   - **Model**: Random Forest
     - Based on data analysis, Random Forest was chosen due to the size of the final modeling dataset with tabular features.
     - Very robust for noisy environmental data, handles nonlinear relationships, noisy predictors, and missin structure
     - simpler models preferred for early prototypes

8. **Early Warning Risk Detection**: Convert predictions into _risk signals_
   - Determine _risk thresholds_ based on regulations
   - Calculate risk score and categorize into low, medium, and high

9. **Pipeline Automation** (if time allows): Use _Airflow DAG_ to orchestrate the workflow.
   - Ensure the system can _continously update forecasts_

10. **Output Storage**: Store predictions and risk results (locally, S3, NoSQL database)

11. **Final Deliverables**:
    - Forecasts of water quality 3 months ahead
    - Interpretable risk scores per location

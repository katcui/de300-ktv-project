# DE300 Project: Early Warning Water Quality Risk Detection System and Forecasting

## Project Roadmap:
1. **Goal**: Building a system that can *detect early warning signals of water quality degradation* by forecasting chemical levels 3 months into the future.
    - Identifies locations where trends indicate potential future risk.
    - **Output**: A risk score per location predicting future degradation.
    
2. **Location Selection and Feasibility Analysis**: Test multiple locations (large cities, cities with known degradation over time, cities with known poor water quality) --> determine feasibility for model
    - Evaluate based on time span of data, sampling frequency, data density/sparsity, presence of long-term trends, and seasonal patterns
    - Identify 10 locations that provide sufficient data to support our forecasting model

3. **Data Ingestion Pipeline**: From the *Water Quality Portal (WQP) webservice API*, collect relevant data.
    - **Pipeline**: 
        - query WQP API
        - retrieve measurements for selected locations
        - store raw data (currently locally, in the future S3)

4. **Data Cleaning and Standardization**
    - complete unit normalization for unique measurement types
    - aggregate measurements into monthly averages

5. **Feature Engineering**: convert time series into model-ready features
    - **Features**: 
        - previous month chemical levels (*lag features*)
        - month of year (seasonality)

6. **Forecasting Model (Trend Prediction)**
    - train a global model across multiple locations
    - all locations contribute training examples
    - each *training example*:
        - input last month of data, output predicted measurement at t+3 months
    - **Prediction Target**: chemical level in 3 months
    - **Model**: Random Forest Regressor
        - Based on data analysis, Random Forest was chosen due to the size of the final modeling dataset with tabular features.
        - Very robust for noisy environmental data, handles nonlinear relationships, noisy predictors, and missin structure
        - simpler models preferred for early prototypes

7. **Early Warning Risk Detection**: Convert predictions into *risk signals*
    - Determine *risk thresholds* based on regulations
    - Calculate risk score and categorize into low, medium, and high


8. **Pipeline Automation** (if time allows): Use *Airflow DAG* to orchestrate the workflow.
    - Ensure the system can *continously update forecasts*

9. **Output Storage**: Store predictions and risk results (currently locally, in the future S3 and NoSQL database)

10. **Final Deliverables**:
    - Forecasts of water quality 3 months ahead
    - Interpretable risk scores per location

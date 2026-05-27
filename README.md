# Revenue-Forecasting-of-Retail-store-data
To forecast quarterly revenue of a retail store data using Prophet in Python

## Table of Contents
- [Project Overview](#Project-Overview)
- [Data Engineering & Integrity](#Data-Engineering-&-Integrity)
- [Tools](#Tools) 
- [Key Insights](#Key-Insights)
- [Challenges and Solutions](#Challenges-and-Solutions)
  
### Project Overview 
- This project delivers a high-fidelity revenue forecast for a UK based e-commerce retailer using Prophet. Analysed 12000+ raw, dirty retail transactions to forecast quarterly revenue
- By decomposing complex retail signals into organic trends,seasonal waves and holiday multipliers,the model achieves a Mean Absolute Percentage Error(MAPE) of <10%,providing decision-ready guidance for fiscal planning and inventory management.

### Data Engineering & Integrity
1. Data Collection 
- Retail sales data([link](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning))
2. Data Cleaning 
- Noise Reduction: Identified and purged ~4.8% of records(609 records) lacking critical financial variables (Quantity and Spend) to prevent imputation bias.
- Deterministic Imputation: Mathematically reconstructed missing unit prices using the logic (Total Spent/Qty)
- Mapping: Developed a multi dimensional look up table using Category and Unit Price to recover missing Item Names, ensuring 100% sectoral coverage for the revenue model. 
- Outliers: The histogram confirms right skewed distribution of consumer spend. the continuous decay in frequency suggests these are valid high-value consumer transactions rather than data entry errors or outliers. I chose to retain 100% of these records to capture the full revenue for the model. 
- Datatype Conversion: Converted object strings to datetime for transaction_date column and extracted "Year","Month", and "Quarter" for growth analysis
<img width="698" height="307" alt="Capture2" src="https://github.com/user-attachments/assets/111f6697-0284-4b5d-b2fa-85f694ec1c9a" />

3. Analysis and Visualization
- Revenue Forecast(2025)
  - The blue line tracks historical actuals with high precision, projecting a significant surge into early 2025.
<img width="2975" height="1849" alt="forecast_plot1" src="https://github.com/user-attachments/assets/542b98c5-6587-48f3-bb5d-0b22df9d2634" />

- Component Decomposition
  - Trend: shows the underlying health of the business after removing seasonal noise.
  - Seasonality: Identifies the "Summer Surge" and "Autumn Dip" patterns
  - Holidays: Quantifies the massive impact of UK-specific events on total volume.
<img width="2658" height="2666" alt="Trend_plot" src="https://github.com/user-attachments/assets/7837807f-7072-4239-8717-f8cac51a3304" />

### Tools
- Python 3.10
- Prophet - Time Series Modelling
- Pandas/Numpy (Data Engineering)
- Matplotlib (Visualization)
- Conda - Forge (Environment Management)

### Key Insights
- YoY Analysis: in 2023 contraction was led by high spend customer pullback followed by 2024 recovery across all spend tiers. High spend transactions accelerated sharply in Q4 2024(+18% to +27%), suggesting premium segment recovery - an indicator that can give positive earnings to retail operators.
<img width="591" height="511" alt="YoY Analysis" src="https://github.com/user-attachments/assets/83dbd696-cab5-4731-a22b-b03e9db70c3c" />


- Market consensus of 118,000 was approximated based on 2023 quartely averages.This should be replaced with actual consensus estimates.The +14.64% divergence suggests the transaction would have indicated an earnings beat.
<img width="580" height="228" alt="divergence" src="https://github.com/user-attachments/assets/10e0656e-54b9-427f-a904-c42ef6ac4114" />


- January 2025 Peak: Projected revenue of ~52k, marking the strongest performing month in the 3-year historical dataset. Q1 2025 suggest a February holiday dip and rise from March. 
- July Dip: Identified a recurring 18-20% revenue dip in July, providing a clear window for cost-optimization or aggressive pivoting
<img width="624" height="173" alt="Capture" src="https://github.com/user-attachments/assets/d8fd04d9-3b84-4ae1-a017-f0e1280e16b4" />
<img width="691" height="134" alt="Capture1" src="https://github.com/user-attachments/assets/ef4a88d9-beec-4139-9dac-959764bc5fcf" />
<img width="874" height="490" alt="dashboard" src="https://github.com/user-attachments/assets/95d5356f-61e6-46f9-a9cd-40e0b2f57635" />

### Challenges and Solutions
- Mititgating Data Latency Bias: Identified that january 2025 reporting period was incomplete (containing only 18 days of transactions). To avoid a false negative in the growth analysis and prevent skewing the time series model, I truncated the dataset at EOY 2024. This ensured the final 14.64% positive divergence insight was derived from full, comparable reports
- Standard linear models failed to capture holiday spikes and the subsequent February "Post-holiday dip" which is common in UK retail. So deployed Prophet with yearly_seasonality=True, by model successfully isolated the underlying growth trend from predictable calendar based noise, resulting in a tighter confidence interval for the Q1 forecast.

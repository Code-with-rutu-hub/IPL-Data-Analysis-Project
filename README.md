# IPL Match & Player Performance Analytics Dashboard
Power BI +SQL Project | 5 Tesla Models | State-wise Sales Insights  


---

### Project Overview  
This project uses two IPL datasets:

matches.csv
deliveries.csv

The goal is to extract insights into:
- Team performance across IPL seasons 
- Batting performance and top run scorers 
- Match-winning patterns and toss impact
- Overall IPL trends using interactive visual analytics

For the five most common Tesla models:
- Model 3  
- Model Y  
- Model S  
- Model X  
- Cybertruck

---

### Tools & Technologies  
- Microsoft Power BI (for dashboard creation)  
- Excel (data formatting & pre-processing)  
- Power Query (ETL in Power BI)  
- DAX (measures and KPIs)  
- Adobe Illustrator / Figma (UI enhancement, optional)

---

### Excel for Data Formatting & Pre-Processing  

- Cleaned missing values and duplicates  
- Formatted numeric fields (e.g., price, mileage, EMI)  
- Standardized column names and categories  
- Created calculated fields like EMI (if missing)  
- Prepared structured layout for Power BI import  

> Excel was used for quick and efficient pre-processing before loading data into Power BI.

![yoo mee](https://github.com/user-attachments/assets/17746245-9132-4077-8da6-64941244dafc)



### Dataset Breakdown  
The dataset used for this analysis includes:
- Tesla model name  
- Average price (USD)  
- Median odometer (miles)  
- EMI (Estimated Monthly Installment)  
- State of sale  
- Accident history (% with or without)  
- Driver Assist System type:
  - Full Self-Driving (FSD)  
  - Enhanced Autopilot  
  - Standard Autopilot  

Each vehicle record was filtered and grouped to compute KPIs across states.

---

### Data Transformation  
Using **Power Query** and **DAX**, the data was transformed to calculate:  
- Average price per model/state
```
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    AVERAGE('Tesla Stock Dataset'[price]),
    CALCULATE(
        AVERAGE('Tesla Stock Dataset'[price]),
        'Tesla Stock Dataset'[model] = selected
    )
)

```
- Median mileage (odometer)
```
MedianOdometer = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    MEDIAN('Tesla Stock Dataset'[odometer]),
    CALCULATE(
        MEDIAN('Tesla Stock Dataset'[odometer]),
        'Tesla Stock Dataset'[model] = selected
    )
)

```
- EMI estimates
```
AverageEMI = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    AVERAGE('Tesla Stock Dataset'[EMI]),
    CALCULATE(
        AVERAGE('Tesla Stock Dataset'[EMI]),
        'Tesla Stock Dataset'[model] = selected
    )
)
```
- Accident percentage
```
PercentWithAccident = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    DIVIDE(
        CALCULATE(COUNTROWS('Tesla Stock Dataset'), 'Tesla Stock Dataset'[accident_history] <> "No Accidents"),
        COUNTROWS('Tesla Stock Dataset'),
        0
    ),
    DIVIDE(
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected,
            'Tesla Stock Dataset'[accident_history] <> "No Accidents"
        ),
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected
        ),
        0
    )
)

```
- Percent With No Accident
```
PercentNoAccident = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    DIVIDE(
        CALCULATE(COUNTROWS('Tesla Stock Dataset'), 'Tesla Stock Dataset'[accident_history] = "No Accidents"),
        COUNTROWS('Tesla Stock Dataset'),
        0
    ),
    DIVIDE(
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected,
            'Tesla Stock Dataset'[accident_history] = "No Accidents"
        ),
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected
        ),
        0
    )
)
``` 

Visuals were color-coded by driver assist systems and filtered by model.

---

### Power BI Dashboard  
Each Tesla model has a dedicated dashboard featuring the following key visuals:
- Average Price  
- Median Odometer  
- EMI  
- % With/Without Accident  
- Price by State  
- Mileage by State  
- Average Price by Driver Assist System (DAS)  
- 3D Vehicle View (UI/UX enhancement)

![cyber truck dashboard](https://github.com/user-attachments/assets/56e1b3af-ebdb-4aeb-9ea8-b0e65f69ae5c)

![modep s](https://github.com/user-attachments/assets/a7a29b17-7f0d-4763-ad5b-c4731fad2908)

![model 3 dashboard](https://github.com/user-attachments/assets/cbe5a9f2-7601-4adb-b063-d5eeb02bf45a)

![model x dashboard](https://github.com/user-attachments/assets/f3409ee2-8347-4a86-92da-971011239a3c)

![model y](https://github.com/user-attachments/assets/c7567f61-9a0a-4c38-b835-32f0261c7d48)


---

### Insights & Findings  
1. **Model Y** is the most affordable option at ~$33.9K with the lowest EMI ($529.37).  
2. **Model X** has the highest average price and EMI, making it the most premium used Tesla option.  
3. **Model 3** is the most balanced in terms of price vs. mileage.  
4. **Cybertruck** data suggests it's new to the market (lower mileage) but priced mid-range.  
5. **States like NY, OH, NC, VA** appear across the top for mileage, suggesting high usage or resales.  
6. **Accident-free rate** is high across all models, with **Model 3 and Y** being the safest (>90%).

---

### Recommendations  

#### 1. 🎯 Target High-Volume States  
Focus marketing and resale efforts in states like **NY, OH, NC, VA**, which show high vehicle turnover and mileage demand.

#### 2. 💼 Price Based on DAS  
Use **Driver Assist System tiers** as a pricing lever.  
> Full Self-Driving variants command significantly higher average prices (up to **$63.2K**).

#### 3. 📉 Promote Lower EMI Models  
**Model Y and Model 3** with lower EMIs may be ideal for entry-level EV buyers or fleet purchases.

#### 4. 🔍 Expand Dataset for Model Y & Cybertruck  
As newer models, their datasets will grow. Monitoring pricing trends can help forecast **depreciation** and **resale value**.

---

### Future Work
* Integrate live market data (Tesla API, vehicle resale sites)
* Include model year and battery degradation metrics
* Predict resale value using regression models (Python + ML)
* Build a web-based dashboard using Power BI Embedded or Streamlit
* Analyze charging patterns and locations (if location data becomes available)

# IPL Match & Player Performance Analytics Dashboard
Power BI +SQL Project | 5 Tesla Models | State-wise Sales Insights  


---

### Project Overview  
This project uses two IPL datasets:

- matches.csv
- deliveries.csv

The goal is to extract insights into:
- Team performance across IPL seasons 
- Batting performance and top run scorers 
- Match-winning patterns and toss impact
- Overall IPL trends using interactive visual analytics


---

### SQL Database

- Create Database
```
CREATE DATABASE IPL_Project;
USE IPL_Project;
```
- Create Matches Table
```
CREATE TABLE matches (
    id INT PRIMARY KEY,
    season VARCHAR(20),
    city VARCHAR(100),
    date DATE,
    team1 VARCHAR(100),
    team2 VARCHAR(100),
    toss_winner VARCHAR(100),
    toss_decision VARCHAR(20),
    result VARCHAR(20),
    dl_applied INT,
    winner VARCHAR(100),
    win_by_runs INT,
    win_by_wickets INT,
    player_of_match VARCHAR(100),
    venue VARCHAR(255),
    umpire1 VARCHAR(100),
    umpire2 VARCHAR(100),
    umpire3 VARCHAR(100)
);
```
- Create Deliveries Table
```
CREATE TABLE deliveries (
    match_id INT,
    inning INT,
    batting_team VARCHAR(100),
    bowling_team VARCHAR(100),
    over_no INT,
    ball INT,
    batsman VARCHAR(100),
    non_striker VARCHAR(100),
    bowler VARCHAR(100),
    is_super_over INT,
    wide_runs INT,
    bye_runs INT,
    legbye_runs INT,
    noball_runs INT,
    penalty_runs INT,
    batsman_runs INT,
    extra_runs INT,
    total_runs INT,
    player_dismissed VARCHAR(100),
    dismissal_kind VARCHAR(100),
    fielder VARCHAR(100)
);
```
### Data Cleaning Queries
- Check Null Values
```
SELECT *
FROM matches
WHERE winner IS NULL;
```
- Remove Duplicate Matches
```
SELECT id, COUNT(*)
FROM matches
GROUP BY id
HAVING COUNT(*) > 1;
```

- Standardize Team Names
```
UPDATE matches
SET team1 = 'Delhi Capitals'
WHERE team1 = 'Delhi Daredevils';
```

### SQL Analysis Queries
- Total Matches Played
```
SELECT COUNT(*) AS total_matches
FROM matches;
```

- Matches Won by Each Team
```
SELECT winner, COUNT(*) AS wins
FROM matches
GROUP BY winner
ORDER BY wins DESC;

``` 
- Top 10 Run Scorers
```
SELECT batsman,
       SUM(batsman_runs) AS total_runs
FROM deliveries
GROUP BY batsman
ORDER BY total_runs DESC
LIMIT 10;

```
- Top Wicket Takers
```
SELECT bowler,
       COUNT(player_dismissed) AS wickets
FROM deliveries
WHERE dismissal_kind NOT IN ('run out', 'retired hurt')
GROUP BY bowler
ORDER BY wickets DESC
LIMIT 10;

```
- Highest Team Score
```
SELECT match_id,
       batting_team,
       SUM(total_runs) AS total_score
FROM deliveries
GROUP BY match_id, batting_team
ORDER BY total_score DESC
LIMIT 10;
```
- Toss Impact Analysis
```
SELECT toss_decision,
       COUNT(*) AS matches,
       SUM(CASE WHEN toss_winner = winner THEN 1 ELSE 0 END) AS toss_and_match_won
FROM matches
GROUP BY toss_decision;

```
- Most Player of the Match Awards
```
SELECT player_of_match,
       COUNT(*) AS awards
FROM matches
GROUP BY player_of_match
ORDER BY awards DESC
LIMIT 10;

```

- Best Strike Rate Players
```
SELECT batsman,
       SUM(batsman_runs) AS runs,
       COUNT(ball) AS balls,
       ROUND((SUM(batsman_runs) * 100.0 / COUNT(ball)),2) AS strike_rate
FROM deliveries
GROUP BY batsman
HAVING runs > 1000
ORDER BY strike_rate DESC;

```
- Economy Rate of Bowlers
```
SELECT bowler,
       ROUND(SUM(total_runs) * 6.0 / COUNT(ball),2) AS economy
FROM deliveries
GROUP BY bowler
HAVING COUNT(ball) > 300
ORDER BY economy ASC;

```
- Venue-wise Match Count
```
SELECT venue,
       COUNT(*) AS total_matches
FROM matches
GROUP BY venue
ORDER BY total_matches DESC;

``` 
### Data Transformation  
Using **Power Query** and **DAX**, the data was transformed to calculate:  
- Total Runs
```
Total Runs = SUM(deliveries[total_runs])

```
- Total Matches
```
Total Matches = DISTINCTCOUNT(matches[id])

```
- Total Sixes
```
Total Sixes =
CALCULATE(
    COUNTROWS(deliveries),
    deliveries[batsman_runs] = 6
)

```
- Total Fours
```
Total Fours =
CALCULATE(
    COUNTROWS(deliveries),
    deliveries[batsman_runs] = 4
)

```
- Total Wickets
```
Total Wickets =
COUNT(deliveries[player_dismissed])

```

- Win Percentage
```
Win % =
DIVIDE(
    COUNT(matches[winner]),
    DISTINCTCOUNT(matches[id])
) * 100

```
---

### Power BI Dashboard  


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

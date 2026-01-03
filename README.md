# ✈️ U.S. Flight Delay & Cancellation Analysis - Excel

## 📌 Introduction

This project analyzes U.S. flight delay and cancellation data from 2019 to 2023 to evaluate airline and airport performance and uncover patterns in travel disruptions. The analysis focuses on identifying trends in delays over time, the primary causes of delays and cancellations, and differences in on-time performance across carriers and airports.

Using key performance metrics, such as on-time arrival rate, average delay times, and cancellation rates, the project assesses which carriers and airports experience the most operational challenges. The results are presented through an interactive Excel dashboard that enables users to explore trends, compare performance, and identify actionable insights to support data-driven operational improvements.

## 🎯 Objectives

1. Analyze flight delay trends over time
2. Identify major causes of delays and cancellations
3. Determine which carriers have the lowest on-time performance
4. Evaluate airport performance to identify which airport experiences the most delays
5. Build an interactive dashboard to explore the data
6. Highlight actionable insights for operational improvements based on patterns

## 📂 Dataset

**Source:** [Flight Delay and Cancellation Dataset (2019-2023)](https://www.kaggle.com/datasets/patrickzel/flight-delay-and-cancellation-dataset-2019-2023)

This dataset is a random sample of 3,000,000 from 29,380,334 flights between January 2019 - August 2023. It includes information on:

- Flight date
- Airline
- Origin city and airport
- Destination city and airport
- Scheduled departure time
- Actual departure time
- Delay causes and duration

The original data was sourced from the [US Department of Transportation - Bureau of Transportation Statistics](https://www.transtats.bts.gov/).

## 🛠 Skills Demonstrated

- **ETL (Extract, transform, load)**
- **Power Query**
- **Power Pivot**
- **Data Modeling**
- **Pivot Tables**
- **Pivot Charts**
- **Formulas and Functions**
- **DAX (Data Analysis Expressions)**

### 🔄 Power Query (ETL)

I used Power Query to extract the source dataset (`flights_sample_3m.csv`) and started cleaning the data. The time columns were in the wrong format, so I created several custom columns to make the necessary adjustments. Additionally, I sorted the rows in ascending order by flight date and added an index column to serve as the primary key. This initial query will act as the main table.

> flights_sample_3m

![power_query_steps.PNG](/assets/power_query_steps.PNG)

To reduce redundancy, I created a separate table for airline data, which I used to identify the corresponding airline for each flight. I also eliminated all unnecessary columns from the main table.

> airlines

![airlines.PNG](/assets/airlines.PNG)

As the cancellation codes were not clearly defined in the dataset, I created a new table using the advanced editor to link each cancellation code with its corresponding category. The descriptive category will improve insights into analyzing cancellations.

```m
= #table(
        {"CODE", "CATEGORY"}, 
        {
            {"A", "Carrier"}, 
            {"B", "Weather"}, 
            {"C", "National Aviation System"}, 
            {"D", "Security"}}   
        )
```

> cancel_codes

![cancel_codes.PNG](/assets/cancel_codes.PNG)

The columns for delay reasons in the original dataset were already in a pivot format. To better aggregate the data, I created a new table that unpivots these columns. I also included the index column in the new table to preserve the connection to the main table.

> delays

![delays.PNG](/assets/delays.PNG)

### 🧩 Power Pivot (Data Model)

To link multiple tables together, I used Power Pivot to establish connections between them. I also created a date table within the model. The diagram view made it easy to visualize all the tables and their cardinalities.

![data_model.PNG](/assets/data_model.PNG)

I used DAX to calculate measures such as the on-time arrival rate.

```
On-Time Arrival Rate:=CALCULATE(COUNT([ARR_DELAY]), FILTER(flights_sample_3m,[ARR_DELAY] <= 0)) / COUNT([ARR_DELAY])
```

## 📊 Dashboard

https://github.com/user-attachments/assets/ff167f51-eb34-4d45-b3df-7a9c0fa1fc9e

>  👇 Click below to download the dashboard

[![Static Badge](https://img.shields.io/badge/dashboard-gray?style=for-the-badge&label=excel&labelColor=217346)](https://drive.google.com/drive/folders/1r1j_F6_IbZPvQdBhMYoxIo0Ztw7SDCET?usp=sharing)

## 🔍 Insights

### 📈 Flight Delay Trends Over Time

![avg_delay_trends.png](/assets/avg_delay_trends.png)

- **Seasonality** is a significant driver of delays, with summer travel consistently associated with higher average delays.
- **COVID-19** in 2020 led to temporary improvements in on-time performance, with flights arriving ahead of schedule.
- **Delays** increased in 2021 and remained high through 2023, indicating ongoing operational challenges and a reduced ability to recover schedules.

### ⚙️ Primary Causes of Delays

![delay_causes.png](/assets/delay_causes.png)

- **Carrier-related** issues are the leading cause of delays, contributing the highest overall volume.
- **Late aircraft** and the **National Airspace System** (NAS)  are similarly significant, indicating strong operational dependencies.
- **Weather-related** delays occur far less frequently than operational causes, even though they are more noticeable.
- **Security** delays are minimal and have a negligible impact on the overall delay totals.

### 🚫 Cancellations Breakdown

![cancel_causes.png](/assets/cancel_causes.png)

- **Weather** is the primary reason for cancellations, accounting for the largest proportion of cases.
- **Security** issues are the second most significant contributor, indicating disruption beyond routine operations.
- **Carrier** causes represent a notable portion but are less dominant than weather and security.
- **NAS** contributes the smallest portion, indicating that system-wide constraints are not a frequent cause of cancellations.

### ⏱️ Lowest On-Time Performing Airlines

![lowest_airlines.png](/assets/lowest_airlines.png)

- **Allegiant Air** has the highest average arrival delay, making it the lowest on-time airline. 

### 🛫 Origin Airports with the Longest Delays

![lowest_origin_airports.png](/assets/lowest_origin_airports.png)

- **Pago Pago International Airport** has the highest average departure delays among all analyzed origin airports.
- The remaining airports from top to bottom are **Santa Maria Airport**, **Portsmouth International Airport**, **Pitt–Greenville Airport**, and **Hagerstown Regional Airport**.

## 🏁 Conclusion

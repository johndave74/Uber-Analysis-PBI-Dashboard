# 🚗 Uber Analysis Power BI Dashboard

A complete end-to-end data analytics project built using **Power BI**, delivering insights into customer behavior, booking performance, cancellations, ride patterns, and operational efficiency.
This project includes full DAX modeling, data transformations, KPI calculations, and an interactive multi-page dashboard.

---

<img width="1059" height="592" alt="Screenshot 2025-11-19 112256" src="https://github.com/user-attachments/assets/9c67cb72-6815-446d-8b23-d2aa8d505d1b" />


## 📌 Project Overview

This project analyzes Uber booking and trip data to identify key business metrics such as:

* Total Trips
* Completed vs Cancelled Trips

<img width="1056" height="594" alt="Screenshot 2025-11-19 112308" src="https://github.com/user-attachments/assets/14b1ef32-7ab7-489c-81f8-93494ff7d910" />


* Customer Behavior Patterns
* Cancellation Insights (Customer + Driver)

<img width="1058" height="594" alt="Screenshot 2025-11-19 112356" src="https://github.com/user-attachments/assets/2353453a-7967-4f9e-9dc5-639e2b0e9af3" />


* Time-of-Day Booking Trends
* Ride Distance & Value Metrics
* Estimated Driver Activity
* Ratings Analysis

<img width="1057" height="596" alt="Screenshot 2025-11-19 112415" src="https://github.com/user-attachments/assets/b955316b-aa93-41b2-8cf4-cb37564f5faa" />


The final dashboard enables the business to understand performance, detect issues, and optimize ride operations.

---

## 🗂️ Dataset Description

The dataset includes the following key columns:

* **Date**
* **Time**
* **Booking ID**
* **Booking Status** (Completed, Cancelled by Customer, Cancelled by Driver)
* **Customer ID**
* **Vehicle Type**
* **Pickup Location / Drop Location**
* **Cancelled Rides by Customer / Driver**
* **Cancellation Reasons**
* **Incomplete Rides & Reasons**
* **Booking Value**
* **Ride Distance**
* **Driver Rating & Customer Rating**
* **Payment Method**

Some versions of the dataset may not contain Driver IDs; estimates were developed where necessary.

---

## 🛠️ Tools Used

* **Power BI Desktop**
* **Power Query** for data cleaning
* **DAX (Data Analysis Expressions)** for calculations
* **GitHub** for version control
* **Excel / CSV Source Files**

---

## 🧹 Data Cleaning & Preparation

The following steps were performed in Power Query:

1. Converted Date and Time to proper types
2. Created a **DateKey** column
3. Extracted **Hour**, **Day**, **Month**, **Year**, **Weekday Name**
4. Trimmed/cleaned text values
5. Replaced nulls with meaningful values
6. Created standardized booking status values
7. Created numerical flags for cancellations and incomplete rides
8. Validated Booking Value and Ride Distance fields
9. Removed duplicate Booking IDs

---

## 📊 Data Model

The final model contains:

### **Fact Table**

* `Raw_data` (Bookings & Trips)

### **Dimension Tables**

* `DimDate`
* `DimTime` (optional, if using hour granularity)
* `DimCustomer` (from Customer ID)
* `DimLocation` (pickup/drop mapping)

### Relationships

* `Raw_data[DateKey]` → `DimDate[DateKey]`
* `Raw_data[Customer ID]` → `DimCustomer[Customer ID]`

(Driver dimension omitted due to missing Driver ID in dataset)

---

## 📐 Key DAX Measures

A few examples used in the report:

```dax
Total Customers = DISTINCTCOUNT(Raw_data[Customer ID])

Total Trips = COUNTROWS(Raw_data)

Completed Trips =
CALCULATE(COUNTROWS(Raw_data), Raw_data[Booking Status] = "Completed")

Cancelled Trips =
CALCULATE(
    COUNTROWS(Raw_data),
    Raw_data[Booking Status] = "Cancelled by Customer"
        || Raw_data[Booking Status] = "Cancelled by Driver"
)

Cancellation Rate =
DIVIDE([Cancelled Trips], [Total Trips], 0)

Avg Booking Value = AVERAGE(Raw_data[Booking Value])

Avg Distance = AVERAGE(Raw_data[Ride Distance])
```

### **Time Segment Column**

```dax
Time Segment =
VAR Hr = Raw_data[Hour]
RETURN
    SWITCH(
        TRUE(),
        Hr >= 5  && Hr < 12, "Morning",
        Hr >= 12 && Hr < 17, "Afternoon",
        Hr >= 17 && Hr < 21, "Evening",
        "Night"
    )
```

### **Estimated Drivers (due to missing Driver ID)**

```dax
Estimated Drivers =
CALCULATE(
    DISTINCTCOUNT(Raw_data[Booking ID]),
    Raw_data[Cancelled Rides by Driver] = 1
)
```

Full list of measures is included inside the `.pbix` file.

---

## 📈 Dashboard Pages

The report contains the following pages:

### **1. Home / Landing Page**

* Hero image
* Navigation buttons
* Project title: **Uber Analysis**

### **2. Overview Dashboard**

* Total Customers
* Estimated Drivers
* Total Trips
* Completion Rate
* Avg Distance
* Avg Ratings
* Booking trend (daily/monthly)

### **3. Cancellation Analysis**

* Customer vs Driver Cancellations
* Cancellation reasons
* Heatmap by hour & weekday
* Top locations with high cancellation rates

### **4. Time & Distance Insights**

* Time-of-day segments
* Distance patterns
* Ride value by time
* Peak demand periods

### **5. Customer Insights**

* Distribution of customer ratings
* Repeat customers
* Payment method usage

### **6. Detailed Trip Explorer**

* Table view for all fields
* Slicers for filtering
* Drillthrough enabled

---

## 📝 Key Insights Summary

* Majority of cancellations are driven by customers
* Peak booking times fall between **Morning (8AM–11AM)** and **Evening (5PM–8PM)**
* Most rides are short-range (auto rides dominate dataset)
* "Change of plans" is the most common customer cancellation reason
* Very few incomplete rides
* Payment preference varies by region

---

## 📦 Repository Structure

```
📁 uber-analysis-powerbi
│
├── 📄 README.md
├── 📁 data
│   └── uber_raw.xlsx
│
├── 📁 powerbi
│   └── Uber_Analysis.pbix
│
├── 📁 docs
│   └── dashboard_screenshots/
│
└── 📁 dax
    └── measures.txt
```

---

## 🚀 How to Use This Project

1. Clone the repo
2. Open the `.pbix` file in Power BI Desktop
3. Refresh the dataset
4. Explore different dashboards
5. Modify or extend the analysis as needed

---

## 📧 Contact

Created by **CloudTech Analytics**

*John David* | cloudtechanalytics.consultant@gmail.com

For collaborations or improvements, feel free to open an issue or submit a pull request.

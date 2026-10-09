# ZoomRide Project (SQL)

## Project Overview
This project analyzes ZoomRide trip data using SQL to answer key business questions related to revenue, customer behavior, data quality, and operational performance.

The objective was to clean the dataset, identify data quality issues, perform exploratory analysis, and provide actionable insights to management.

---

## Business Questions
The Manager requested answers to the following questions:

- Which city earns the most money?
- Which month do customers ride the most?
- Which vehicle type generates the highest revenue?

Additional tasks included:

- Identifying duplicate records
- Detecting missing values
- Cleaning inconsistent city names
- Finding top customers by spending
- Identifying customers who have never booked a trip

---

## Tools Used
Onecompiler (MySQL)
https://onecompiler.com/mysql/455g6zy54
---

## Dataset Tables
[View the zoomride dataset](Data_set_Tables)
- Trips Table
  
Contains trip information including:
Trip ID, Customer ID, Driver ID, City, Trip Date, Distance (KM), Fare, Status

- Drivers Table
  
Contains driver information including:
Driver ID, Driver Name, Vehicle Type

- Customers Table
  
Contains customer information including:
Customer ID, Customer Name

---

## Data Cleaning Process
![SQL Query Result](Data_cleaning)
- Checked Total Records
  
QUERY:

SELECT COUNT(*) FROM trips;
The query is executed to determine total number of rows before cleaning.

- Identified and Remove Duplicate Records
  
QUERY 1:

SELECT customer_id,driver_id,trip_date,fare,
MIN(trip_id),
MAX(trip_id) FROM trips
GROUP BY customer_id,driver_id,trip_date,fare
HAVING COUNT(*) > 1;

I noticed that there were trip having same Customer, Driver, Trip Date, Fare but different Trip IDs.

QUERY 2:

DELETE FROM trips
WHERE trip_id IN (25, 87);

- Checked Missing Fares
  
QUERY:

SELECT COUNT(*) FROM trips
WHERE status='Completed'
AND fare IS NULL;

- Standardized City Names
  
It was discovered that some cities had no consistent name across all records, for instance Lagos, lagos, LAGOS,  Lagos

QUERIES:

UPDATE trips
SET city = TRIM(city);

UPDATE trips
SET city = 'Lagos'
WHERE city IN ('lagos', 'LAGOS');

UPDATE trips
SET city = 'Abuja'
WHERE city IN ('abuja', 'ABUJA');

UPDATE trips
SET city = 'Kampala'
WHERE city IN ('kampla');

UPDATE trips
SET city = 'Nairobi'
WHERE city IN ('Nairobbi');

UPDATE trips
SET city = 'Port Harcourt'
WHERE city IN ('port harcourt', 'PORT HARCOURT', 'PH', 'Port-Harcourt'); 

---

## Analysis Performed
![Explore the analysis performed](Analysis_performed)
- Q1: Total Number of Trips
  
QUERY:

SELECT COUNT(*) FROM trips;

- Q2: Longest Completed Trips
  
QUERY:

SELECT trip_id, city, distance_km, fare
FROM trips
WHERE status='Completed'
ORDER BY distance_km DESC
LIMIT 5;

- Q3: Trips by City
  
QUERY:

SELECT city,
COUNT(*) AS total_trips
FROM trips
GROUP BY city;

- Q4: Revenue by City
  
QUERY:

SELECT city,
COUNT(*) AS trips,
SUM(fare) AS revenue,
ROUND(AVG(fare),2) AS average_fare
FROM trips
WHERE status='Completed'
GROUP BY city
ORDER BY revenue DESC;

- Q5: Revenue by Month
  
QUERY:

SELECT DATE_FORMAT(trip_date,'%Y-%m') AS month,
COUNT(*) AS trips, SUM(fare) AS revenue
FROM trips
WHERE status='Completed'
GROUP BY month
ORDER BY month;

- Q6: Revenue by Vehicle Type
  
QUERY:

SELECT d.vehicle_type,
COUNT(*) AS trips,
SUM(t.fare) AS revenue
FROM trips t
INNER JOIN drivers d
ON t.driver_id=d.driver_id
WHERE t.status='Completed'
GROUP BY d.vehicle_type
ORDER BY revenue DESC;

---

## Key Findings

![Explore the findings](Analysis_performed)
Answers to The Manager's questions:

- Which city earns the most money?
  The city with the highest revenue is Lagos, generating ₦205,280 from completed trips.
- Which month do customers ride the most?
  The month with the highest ride volume is 12th,2025 recording 31 completed trips.
- Which vehicle type generates the highest revenue
  The highest-earning vehicle category is Economy, generating ₦262,550 in total revenue.

Others:
- Data quality issues found
Duplicate trip records.
Inconsistent city naming conventions.
Missing fares in completed trips
- Data Quality Improvements or corrections made
Standardized city names.
Removed duplicate records.
Preserved missing fare records for transparency.

---

## Recommendations
- There should be increase marketing efforts in the highest-performing city (Lagaos).

- There should be employment of additional drivers during peak-demand months.

- There should be expansion of the most profitable vehicle category.

- The implementation of validation rules to prevent duplicate trip entries should be employed.

- Enforcement of standardized city naming during data entry should be mandated.

---

## Conclusion
The project highligted the importance of accurate and consistent data in generating meaningful insights. It also demonstrated how SQL queries can support evidence-based decision-making in areas such as revenue monitoring, operational planning, and customer engagement.

Overall, this project strengthened my practical SQL skills and my ability to approach business problems analytically, from data preparation / cleaning to insight generation and actionable recommendations.

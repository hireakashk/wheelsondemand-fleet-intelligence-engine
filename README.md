# WheelsOnDemand — Enterprise Vehicle Rental Management & Fleet Intelligence Engine

A relational database and SQL analytics project built to manage a vehicle rental business — tracking fleet availability, customer bookings, payments, and revenue performance across cars and bikes.

---

## Project Overview

**WheelsOnDemand** simulates the core database and reporting needs of a vehicle rental company that rents out cars and bikes on a short-term basis. The project includes a normalized relational schema with primary/foreign-key relationships, along with **20 SQL queries** that answer real business questions a rental company's management team would ask — from vehicle availability to top-spending customers.

- **Author:** Akash Kumar
- **Role Focus:** Data Analyst / SQL Developer Portfolio Project
- **Database Engine:** PostgreSQL (pgAdmin 4)
- **Total Queries:** 20

---

## Problem Statement

A vehicle rental company provides cars and bikes to customers on a short-term rental basis. The company needs a database system to track available vehicles, customer information, bookings, rental duration, payment status, rental bills, and vehicle usage and revenue.

This project answers key business questions such as:
- Which vehicles are currently available?
- Which customers have made bookings?
- Which vehicle types are most popular?
- How much revenue has been generated?
- Which customers have spent the most?
- Which vehicles have been rented most frequently?
- Which bookings are unpaid?
- What is the average rental bill?
- Which vehicles generate the highest revenue?

---

## Database Design

The system is built on **3 relational tables**: `VEHICLE`, `CUSTOMER`, and `BOOKING`.

### Relationship Structure

```
CUSTOMER  1 ──────< BOOKING >────── 1  VEHICLE
```

- One customer can make multiple bookings (`CUSTOMER.CID` → `BOOKING.CID`)
- One vehicle can be booked multiple times over time (`VEHICLE.VID` → `BOOKING.VID`)
- `BOOKING` acts as the central transactional table linking customers to the vehicles they rent

### VEHICLE Table

| Column | Description |
|---|---|
| `VID` | Vehicle ID (Primary Key) |
| `Vehicle_Name` | Vehicle name/model |
| `Vehicle_Type` | Car / Bike |
| `Brand` | Vehicle brand |
| `Model_Year` | Manufacturing year |
| `Status` | Available / Rented / Maintenance |
| `Fuel_Type` | Petrol / Diesel / Electric |
| `Rent_Per_Day` | Daily rental cost |

### CUSTOMER Table

| Column | Description |
|---|---|
| `CID` | Customer ID (Primary Key) |
| `Customer_Name` | Customer name |
| `Phone` | Contact number |
| `Age` | Customer age |
| `Licence_Number` | Driving licence number |
| `Address` | Customer address |

### BOOKING Table

| Column | Description |
|---|---|
| `BID` | Booking ID (Primary Key) |
| `CID` | Customer ID (Foreign Key → CUSTOMER) |
| `VID` | Vehicle ID (Foreign Key → VEHICLE) |
| `Booking_Date` | Date booking was created |
| `Start_Date` | Rental start date |
| `End_Date` | Rental end date |
| `Payment_Status` | Paid / Pending / Partial |
| `Bill_Amount` | Total rental bill |

---

## Query Sections

The 20 queries are organized into 4 progressive sections:

| Section | Range | What It Covers |
|---|---|---|
| **A — Basic SQL** | Q1 – Q5 | Filtering vehicles by availability/type/price, customers by age, bookings by payment status |
| **B — JOIN** | Q6 – Q10 | Combining customer, vehicle, and booking data; finding customers/vehicles with at least one booking |
| **C — Aggregate Functions** | Q11 – Q15 | Total revenue, average bill, highest bill, booking counts, revenue split by vehicle type |
| **D — GROUP BY / HAVING** | Q16 – Q20 | Per-customer and per-vehicle booking counts, high-frequency vehicle types, top-3 revenue-generating customers |

---

## Highlighted Queries

**Multi-Table JOIN — Full Booking Details with Customer Info**
```sql
SELECT
    b.BID,
    b.Booking_Date,
    b.Bill_Amount,
    v.Vehicle_Name,
    v.Brand,
    c.Customer_Name,
    c.Phone
FROM
    BOOKING b
JOIN
    VEHICLE v ON b.VID = v.VID
JOIN
    CUSTOMER c ON b.CID = c.CID
WHERE
    v.Vehicle_Type = 'Car';
```

**Revenue Split by Vehicle Type**
```sql
SELECT
    v.Vehicle_Type,
    SUM(b.Bill_Amount) AS total_segment_revenue
FROM
    BOOKING b
JOIN
    VEHICLE v ON b.VID = v.VID
GROUP BY
    v.Vehicle_Type;
```

**Vehicle Types with More Than 3 Bookings (`HAVING`)**
```sql
SELECT
    v.Vehicle_Type,
    COUNT(b.BID) AS total_bookings
FROM
    VEHICLE v
JOIN
    BOOKING b ON v.VID = b.VID
GROUP BY
    v.Vehicle_Type
HAVING
    COUNT(b.BID) > 3;
```

**Business Question — Top 3 Customers by Revenue**
```sql
SELECT
    c.Customer_Name,
    COUNT(b.BID) AS Number_of_Bookings,
    SUM(b.Bill_Amount) AS Total_Spending
FROM
    CUSTOMER c
JOIN
    BOOKING b ON c.CID = b.CID
GROUP BY
    c.Customer_Name
ORDER BY
    Total_Spending DESC
LIMIT 3;
```

---

## Skills Demonstrated

- Relational database design with primary and foreign-key relationships
- Core SQL: `SELECT`, `WHERE`, `BETWEEN`, filtering by multiple conditions
- Multi-table `JOIN` operations across 3 related tables
- Aggregate functions: `SUM`, `AVG`, `MAX`, `COUNT`, `ROUND`
- Grouped analysis: `GROUP BY`, `HAVING`
- Business KPI reporting: total revenue, average bill, top-spending customers, vehicle utilization
- Sorting and ranking with `ORDER BY` and `LIMIT`

---

## How to Use

1. Set up a PostgreSQL database and create the `VEHICLE`, `CUSTOMER`, and `BOOKING` tables using the schema described above.
2. Load sample or real vehicle rental data into the tables.
3. Run the queries sequentially from `vehicle_rental_analytics.sql`, or execute individual queries by section based on the category you want to explore.

---

## Author

**Akash Kumar**
<br>
Aspiring Data Analyst / SQL Analyst

- 📧 Email: [hire.akashk@gmail.com](mailto:hire.akashk@gmail.com)
- 🔗 LinkedIn: [linkedin.com/in/akashkumar-56398241a](https://www.linkedin.com/in/akashkumar-56398241a)


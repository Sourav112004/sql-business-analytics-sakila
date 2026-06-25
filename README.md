# SQL Business Analytics — Sakila DVD Rental Database

## Overview
A collection of real-world business analytics queries written in MySQL 
on the Sakila sample database — a DVD rental store dataset. Each query 
solves a specific business problem using advanced SQL techniques including 
multi-table JOINs, CTEs, aggregations, and CASE-based segmentation.

---

## Tools & Technologies
| Tool  | Purpose                                      |
|-------|----------------------------------------------|
| MySQL | Querying, aggregation, business reporting    |
| JOINs | Multi-table data retrieval                   |
| CTEs  | Readable subquery logic                      |
| CASE  | Customer and revenue segmentation            |

---

## Database
- Sakila Sample Database (MySQL official sample)
- Tables used: film, film_category, category, rental, customer, 
  inventory, payment, store

---

## Business Queries

### Query 1 — Genre-Based Film Catalogue
Retrieves all films belonging to the Sci-Fi category by joining the 
film, film_category and category tables.

```sql
SELECT f.title, fc.film_id, fc.category_id, c.name 
FROM film_category fc
JOIN category c ON c.category_id = fc.category_id
JOIN film f ON f.film_id = fc.film_id
WHERE c.name = 'Sci-Fi';
```

Business Use: Content filtering for genre-specific promotions or 
catalogue management.

---

### Query 2 — Customer Rental Traffic Segmentation
Counts total rentals per customer and segments them into traffic tiers 
using a CASE statement — High, Moderate, Low, and Little to No Traffic.

```sql
SELECT r.customer_id, c.first_name, c.last_name,
  COUNT(*) AS "No of Rentals",
  CASE
    WHEN COUNT(*) > 40  THEN 'High Traffic'
    WHEN COUNT(*) >= 30 THEN 'Moderate Traffic'
    WHEN COUNT(*) >= 20 THEN 'Low Traffic'
    ELSE 'Little to No Traffic'
  END AS "Traffic Segmentation"
FROM rental r
JOIN customer c ON c.customer_id = r.customer_id
GROUP BY r.customer_id, c.first_name, c.last_name
ORDER BY COUNT(*) DESC;
```

Business Use: Identify VIP customers for loyalty programmes and 
targeted marketing campaigns.

---

### Query 3 — Low Performing Inventory Detection
Uses a CTE to identify inventory items rented only once or never, 
then joins back to retrieve the film details.

```sql
WITH low_rentals AS (
  SELECT inventory_id, COUNT(*) 
  FROM rental r
  GROUP BY inventory_id
  HAVING COUNT(*) <= 1
)
SELECT * FROM low_rentals
JOIN inventory i ON i.inventory_id = low_rentals.inventory_id
JOIN film f ON f.film_id = i.film_id;
```

Business Use: Supports inventory decisions — identify films to 
discontinue, discount, or replace.

---

### Query 4 — Unreturned Rental Tracking
Identifies all rentals where the return date is NULL, along with 
customer contact details and the film title — useful for follow-up 
communications.

```sql
SELECT f.title, c.first_name, c.last_name, c.email,
  r.customer_id, r.rental_date
FROM rental r
JOIN customer c ON c.customer_id = r.customer_id
JOIN inventory i ON i.inventory_id = r.inventory_id
JOIN film f ON f.film_id = i.inventory_id
WHERE return_date IS NULL;
```

Business Use: Operations team can use this to chase overdue returns 
and reduce inventory loss.

---

### Query 5 — Store Revenue & Customer Profit Segmentation
Calculates total spend per customer for Store 1 and segments them 
into profit tiers using a CASE statement.

```sql
SELECT s.store_id, c.customer_id, c.email,
  SUM(p.amount) AS total_spent,
  CASE
    WHEN SUM(p.amount) >= 65 THEN 'High Profit'
    WHEN SUM(p.amount) >= 55 THEN 'Medium Profit'
    WHEN SUM(p.amount) >= 40 THEN 'Low Profit'
    WHEN SUM(p.amount) <  40 THEN 'Dont Waste Resources'
  END AS profit_type
FROM store s
JOIN customer c ON c.store_id = s.store_id
JOIN payment p ON p.customer_id = c.customer_id
WHERE s.store_id = 1
GROUP BY s.store_id, c.customer_id, c.email
ORDER BY total_spent DESC;
```

Business Use: Revenue intelligence for store managers to prioritise 
high-value customers and reduce spend on low-return segments.

---

## Key SQL Techniques Demonstrated
- Multi-table JOINs (3 and 4 table joins)
- CTE (Common Table Expression) for modular query logic
- CASE statements for business segmentation
- GROUP BY with aggregate functions (COUNT, SUM)
- NULL filtering for operational reporting
- ORDER BY for ranked output

---

## Project Structure
sql-business-analytics-sakila/

│

├── sql/

│   ├── 01_genre_film_catalogue.sql

│   ├── 02_customer_traffic_segmentation.sql

│   ├── 03_low_inventory_detection.sql

│   ├── 04_unreturned_rentals.sql

│   └── 05_store_revenue_segmentation.sql

│

└── README.md

---

## Outcome
Demonstrated ability to write clean, business-focused SQL queries that 
answer real operational and strategic questions — from customer 
segmentation to inventory management and revenue reporting.

---

## About
Sourav Prakash — Data Analyst | SQL | Power BI | Python | Excel
Location: Kerala, India
LinkedIn: https://www.linkedin.com/in/sourav-prakash-analyst/
GitHub: https://github.com/Sourav112004

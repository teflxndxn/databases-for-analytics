# Exercise 02: World Database – Joins, Grouping, and Data Quality

- Name: Blessing Aganaga
- Course: Database for Analytics
- Module: 2
- Database Used: World Database (PostgreSQL)

---

## Instructions

- Answer each question below using SQL executed against the **World database**.
- All SQL commands **must be run by you**.
- For each SQL-based question:
  - Include the SQL command in a fenced code block
  - Include a **screenshot** showing the command and its results
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

When importing records from `worldPGSQL.sql`, **how many cities were imported**?

### Answer
4079

### Screenshot


```sql
-- Your SQL here
```

![Q1 Screenshot](screenshots/q1_city_count.png)

---

## Question 2

Using the World database, write the SQL command to **display each country name along with the name of each language spoken in that country**.

### SQL

```sql
-- SELECT c.name AS country_name,
       cl.language AS language_spoken
FROM public.country c
JOIN public.countrylanguage cl
  ON c.code = cl.countrycode
ORDER BY c.name, cl.language;

```

### Screenshot

![Q2 Screenshot](screenshots/q2_country_languages.png)

---

## Question 3

Using the World database, write the SQL command to **display each country name along with the name of each official language spoken in that country**.

### SQL

```sql
-- SELECT c.name AS country_name,
       cl.language AS official_language
FROM public.country c
JOIN public.countrylanguage cl
  ON c.code = cl.countrycode
WHERE cl.isofficial = 'T'
ORDER BY c.name, cl.language;

```

### Screenshot

![Q3 Screenshot](screenshots/q3_official_languages.png)

---

## Question 4

Consider the following two SQL statements:

```sql
SELECT *
FROM country, countrylanguage
WHERE country.code = countrylanguage.countrycode;
```

```sql
SELECT *
FROM country
LEFT OUTER JOIN countrylanguage
ON country.code = countrylanguage.countrycode;
```

**In your own words**, describe what data the **second query returns that the first query does not**.

### Answer
_The second query returns every country from the country table, even if there is no matching row in countrylanguage. The first query only returns countries that have at least one matching language record, but the LEFT OUTER JOIN also includes countries with no language and shows NULL values for the language columns.

---

## Question 5

Using the World database, write the SQL command to **list all different forms of government** found in the data.
Do **not** repeat any form of government more than once.

### SQL

```sql
-- SELECT DISTINCT governmentform
FROM country
ORDER BY governmentform;

```

### Screenshot

![Q5 Screenshot](screenshots/q5_government_forms.png)

---

## Question 6

Using the World database, write the SQL command to **list all names of cities and countries in one column**.
Label the column **"City or Country Name"**.

### SQL

```sql
-- SELECT name AS "City or Country Name"
FROM public.city

UNION

SELECT name AS "City or Country Name"
FROM public.country;

```

### Screenshot

![Q6 Screenshot](screenshots/q6_union_city_country.png)

---

## Question 7

Using the World database, write the SQL command to **list all countries by name**, along with the **number of languages spoken in each country**.
Be sure to **sort by country name**.

### SQL

```sql
-- SELECT c.name AS country_name,
       COUNT(cl.language) AS number_of_languages
FROM public.country c
JOIN public.countrylanguage cl
  ON c.code = cl.countrycode
GROUP BY c.name
ORDER BY c.name;

```

### Screenshot

![Q7 Screenshot](screenshots/q7_language_count_by_country.png)

---

## Question 8

Using the World database, write the SQL command to **list all languages**, along with the **number of countries where each language is spoken**.
Be sure to **sort by language name**.

### SQL

```sql
-- SELECT language,
       COUNT(countrycode) AS number_of_countries
FROM public.countrylanguage
GROUP BY language
ORDER BY language;

```

### Screenshot

![Q8 Screenshot](screenshots/q8_language_country_count.png)

---

## Question 9

Using the World database, write the SQL command to **list countries that have more than two official languages**, along with the **number of official languages spoken**.

*Hint: There are 8 such countries in this dataset.*

### SQL

```sql
-- SELECT c.name AS country_name,
       COUNT(cl.language) AS official_language_count
FROM public.country c
JOIN public.countrylanguage cl
  ON c.code = cl.countrycode
WHERE cl.isofficial = 'T'
GROUP BY c.name
HAVING COUNT(cl.language) > 2
ORDER BY c.name;

```

### Screenshot

![Q9 Screenshot](screenshots/q9_multiple_official_languages.png)

---

## Question 10

Using the World database, write the SQL command to **find cities where the district value is missing**.

*Hint: Use `LIKE` and the dash (`-`) since some rows use that instead of actual data.*

### SQL

```sql
-- SELECT *
FROM public.city
WHERE district IS NULL
   OR district = ''
   OR district LIKE '-';

```

### Screenshot

![Q10 Screenshot](screenshots/q10_missing_districts.png)

---

## Question 11

Using the World database, write the SQL command to **calculate the percentage of cities with missing district values**.

*Hint: The result should be approximately 0.4%.*

### SQL

```sql
-- 0.10% of cities have missing district values.
```

### Screenshot

![Q11 Screenshot](screenshots/q11_missing_district_percentage.png)

# SQL CRUD
## Restaurant finder
### Table structure
I decided to create two tables, one for restaurants and one for reviews following the intruction of the assignment. And the code is shown below:
```sql
CREATE TABLE restaurants (
    restaurant_id INTEGER PRIMARY KEY,
    name TEXT,
    category TEXT,
    price_tier TEXT,
    neighborhood TEXT,
    opening_time TEXT,
    closing_time TEXT,
    average_rating REAL,
    good_for_kids BOOLEAN
);
```
And another table for reviews, the code is shown below:
```sql
CREATE TABLE reviews (
    review_id INTEGER PRIMARY KEY,
    restaurant_id INTEGER,
    user_name TEXT,
    review_text TEXT,
    rating REAL,
    FOREIGN KEY (restaurant_id) REFERENCES restaurants(restaurant_id)
);
```
### Practice data
I created a mock data for restaurants using [mockaroo.com](https://mockaroo.com) and download it as a CSV file [restaurants.csv](https://github.com/dbdesign-students-fall2023/4-sql-crud-Catherineya/blob/8447252b9686400ebbd41321065010d95d4988ad/data/restaurants.csv). 

###Import data
I used the following code to import the data into the database:
```sql
.mode csv
.import restaurants.csv restaurants
```
### Queries
1. Find all cheap restaurants in a particular neighborhood (pick "Bushwick" as an example).
```sql
SELECT * FROM restaurants WHERE price_tier = "cheap" AND neighborhood = "Bushwick";
```
2. Find all restaurants in a particular genre (pick "French" as an example) with 3 stars or more, ordered by the number of stars in descending order.
```sql
SELECT * FROM restaurants WHERE category = "French" AND average_rating >= 3 ORDER BY average_rating DESC;
```
3. Find all restaurants that are open now.
```sql
SELECT * FROM restaurants WHERE opening_time <= strftime('%H:%M', 'now') AND closing_time >= strftime('%H:%M', 'now');
```
4. Leave a review for a restaurant (pick "The German Beer Hall" as an example).
```sql
INSERT INTO reviews (restaurant_id, user_name, review_text, rating) VALUES (1, "Yiwen", "The food is great!", 5);
```
5. Delete all restaurants that are not good for kids.
```sql
DELETE FROM restaurants WHERE good_for_kids = "false";
```
6. Find the number of restaurants in each NYC neighborhood.
```sql
SELECT neighborhood, COUNT(*) FROM restaurants GROUP BY neighborhood;
```
## Social media app

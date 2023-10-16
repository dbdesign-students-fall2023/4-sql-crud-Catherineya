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
.import data/restaurants.csv restaurants
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
### Table structure
I decided to create two tables, one for users, one for messages and one for stories following the intruction of the assignment. And the code is shown below:
```sql
CREATE TABLE users (
    user_id INTEGER PRIMARY KEY,
    user_name TEXT,
    user_email TEXT,
    user_password TEXT
);
```
And another table for posts, the code is shown below:
# the posts table stores both Messages and Stories. 
```sql
CREATE TABLE Posts (
    post_id INTEGER PRIMARY KEY,
    user_id INTEGER,
    receiver_id INTEGER,
    post_type TEXT,
    post_text TEXT,
    posttime DATETIME,
    visible BOOLEAN,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (receiver_id) REFERENCES users(user_id)
);
```
### Practice data
I created two mock data for users using [mockaroo.com](https://mockaroo.com) and download it as [users.csv](https://github.com/dbdesign-students-fall2023/4-sql-crud-Catherineya/blob/b9f3b6345be6125d8b286da6fe633b3b95c57b92/data/users.csv) and [post.csv](https://github.com/dbdesign-students-fall2023/4-sql-crud-Catherineya/blob/b9f3b6345be6125d8b286da6fe633b3b95c57b92/data/posts.csv).

###Import data
I used the following code to import the data into the database:
```sql
.mode csv
.import data/users.csv users
.import data/posts.csv posts
```
### Queries
1. Register a new user.
```sql
INSERT INTO users (user_name, user_email, user_password) VALUES ("Catherine", "Catherine@gmail.com", "123456");
```
2. Create a new Message sent by a particular User to a particular User.
```sql
INSERT INTO posts (user_id, receiver_id, post_type, post_text, posttime, visible) VALUES (1, 2, "messages", "You're so talented!", "2023/9/15  06:13:00", "true");
```
3. Create a new Story posted by a particular User.
```sql
INSERT INTO posts (user_id, post_type, post_text, posttime, visible) VALUES (1, "stories", "StoryTest!", "2023-10-01 10:10:10", "true");
```
4. Show the 10 most recent visible Messages and Stories, in order of recency.
```sql
SELECT * FROM posts WHERE visible = "true" ORDER BY posttime DESC LIMIT 10;
```
5. Show the 10 most recent visible Messages sent by a particular User to a particular User, in order of recency.
```sql
SELECT * FROM posts WHERE visible = "true" AND user_id = 1 AND receiver_id = 2 ORDER BY posttime DESC LIMIT 10;
```
6. Make all Stories that are more than 24 hours old invisible.
```sql
UPDATE posts SET visible = "false" WHERE post_type = "stories" AND posttime < datetime('now', '-1 day');
```
7. Show all invisible Messages and Stories, in order of recency.
```sql
SELECT * FROM posts WHERE visible = "false" ORDER BY posttime DESC;
```
8. Show the number of posts by each User.
```sql
SELECT user_name, COUNT(*) FROM posts JOIN users ON posts.user_id = users.user_id GROUP BY user_name;
```
9. Show the post text and email address of all posts and the User who made them within the last 24 hours.
```sql
SELECT post_text, user_email, user_name FROM posts JOIN users ON posts.user_id = users.user_id WHERE posttime > datetime('now', '-1 day');
```
10. Show the email addresses of all Users who have not posted anything yet.
```sql
SELECT user_email FROM users WHERE user_id NOT IN (SELECT user_id FROM posts);
```

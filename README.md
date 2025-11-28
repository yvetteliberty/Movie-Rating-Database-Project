# Movie-Rating-Database-Project
![](https://github.com/yvetteliberty/Movie-Rating-Database-Project/blob/main/movieimage.png) 

A beginner friendly SQL project that demonstrates how to create, populate, and analyze a movie rating database using MySQL.
This project helps me practice real-world database concepts such as schema design, CRUD operations, filtering, logical operators, joins, sorting, pagination, and analytical queries.

### 📌 Project Objectives

- Understand how to design a relational database schema.

- Create and manage tables using SQL.

- Insert sample data into multiple tables.

 - Write analytical SELECT queries to explore user behavior and movie ratings.

 - Apply filtering, logical operators, sorting, and pagination.

 -  Derive insights from combined data across tables.

###   🗂 Database Name
  MovieRatingDB
  
  ### 🧱 Database Schema

1️⃣ Users Table
 - user_id	INT (PK)	Unique user ID
 - name	VARCHAR(100)	User's full name
 - age	INT	Age of the user
 - gender	VARCHAR(10)	Male/Female/Other
 - location	VARCHAR(100)	User's city

2️⃣ Movies Table
 - movie_id	INT (PK)	Unique movie ID
 - title	VARCHAR(150)	Movie title
 - genre	VARCHAR(50)	Movie genre
 - release_year	INT	Year the movie premiered

3️⃣ Ratings Table
- rating_id	INT (PK)	Unique rating ID
- user_id	INT (FK)	References Users(user_id)
- movie_id	INT (FK)	References Movies(movie_id)
- rating	DECIMAL(2,1)	Rating score (e.g., 4.5)


### 🛠 MYSQL Setup Code

  Create the Database

CREATE DATABASE MovieRatingDB;
USE MovieRatingDB;

Create Users Table

    CREATE TABLE Users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    age INT,
    gender VARCHAR(10),
    location VARCHAR(100)
);

Create Movies Table

    CREATE TABLE Movies (
    movie_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(150),
    genre VARCHAR(50),
    release_year INT
    );

Create Ratings Table

    CREATE TABLE Ratings (
    rating_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    movie_id INT,
    rating DECIMAL(2,1),
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (movie_id) REFERENCES Movies(movie_id)
    );

###   📥 Insert Sample Data

 Insert Users
  INSERT INTO User (name, age, gender, location) VALUES
  ('John Doe', 28, 'Male', 'Lagos'),
('Mary Johnson', 34, 'Female', 'Abuja'),
('Samuel Peters', 22, 'Male', 'Kano'),
('Ada Obi', 30, 'Female', 'Enugu'),
('Grace Yusuf', 26, 'Female', 'Ibadan'),
('Emeka Uzo', 40, 'Male', 'Port Harcourt');

INSERT INTO movies (Title, release_year, genre, Directors) VALUES
('Inception', 2010, 'Sci-Fi', 'Christopher Nolan'),
('Black Panther', 2018, 'Action', 'Ryan Coogler'),
('Titanic', 1997, 'Romance', 'James Cameron'),
('The Matrix', 1999, 'Sci-Fi', 'Lana Wachowski'),
('Avatar', 2009, 'Adventure', 'James Cameron'),
('The Dark Knight', 2008, 'Action', 'Christopher Nolan');

INSERT INTO ratings (user_id, movie_id, rating_value, rating_date) VALUES
(1, 1, 4.5, '2024-06-01'),
(1, 3, 4.2, '2024-06-02'),
(2, 2, 5.0, '2024-06-03'),
(2, 5, 4.6, '2024-06-04'),
(3, 4, 4.0, '2024-06-05'),
(3, 6, 4.3, '2024-06-06'),
(4, 2, 4.8, '2024-06-07'),
(4, 8, 4.9, '2024-06-08');

### ER Diagram 
This diagram shows the relationship between  the three tables .
![](https://github.com/yvetteliberty/Movie-Rating-Database-Project/blob/main/ER%20DIAGRAM.jpg)


### 🔍 Data Exploration Queries
View All Users

    SELECT * FROM Users
         LIMIT 5;

View All Movies

      SELECT * FROM Movies
      LIMIT 5;

View All Ratings

      SELECT * FROM Ratings
       LIMIT 5;

### 🧠 Analytical Queries
- user older than 25:
  
      select *from user where age >25;

- movies release between 2002 and 2023:
  
      select* from movies where release_year between 2000 AND 2023;

- female users from Abuja:
  
      select *from  user where gender ='Female' and location ='Abuja';

- Rating greater than  or qual to 4
  
      select *from  ratings where rating_value >=4;
###  📊 Sorting, Pagination & DISTINCT

- Sort Movies by Release Year (Newest First)

      SELECT * FROM Movies
      ORDER BY release_year DESC;

- List Distinct Genres

      SELECT DISTINCT genre FROM Movies;

- Top 3 Highest Ratings

      SELECT * FROM Ratings
      ORDER BY rating DESC
      LIMIT 3;

- Paginate User Records (Skip 2, Show Next 2)

      SELECT * FROM Users
        LIMIT 2 OFFSET 2;

-  Sort Users Alphabetically

       SELECT * FROM Users
       ORDER BY name ASC;

### Join ,Aggregate Group BY Queries
  - 1.Top-rated movies 
 - 2.Most active users 
- 3.Average rating per genre 
 - 4.Ratings by location  

-Top-rated movies 
            
    select
     m.title,
    m.genre,
     m.release_year,round(AVG(rating_value),2)AS average_rating
       from  movies m JOIN ratings r ON m.movie_id = r.movie_id
      GROUP BY m.movie_id
       ORDER BY average_rating DESC
     LIMIT 10;

-  Most active users 

       select u.User_id, u.Name ,count(r.rate_id)AS Total_ratings
        FROM User u
        JOIN ratings r ON  u.User_id = r.User_id
       GROUP BY u.User_id
        ORDER BY Total_ratings DESC
         limit 10;


- Average rating per genre
  
           SELECT m.genre,round(AVG(r.rating_value),2) AS average_ratings
            FROM movies m
               JOIN ratings r ON m.movie_id = r.movie_id
              GROUP BY genre
           ORDER BY average_ratings DESC;

-  Ratings by location
 
              SELECT u.Location,count(r.rating_value) AS Total_rating
              FROM user u
               JOIN ratings r ON u.User_id = r.User_id
                GROUP BY u.Location
                order by Total_rating DESC;
 ###  🎯 Business Insight

 The SQL analysis shows that:

 - Adult users (25+) and female users in Abuja form highly engaged user groups.

- Recent films (2000–2023) and non-Sci-Fi genres dominate user interest.

- Top-rated movies and top-rating users are key drivers of platform engagement.

- Genres with high average ratings should be prioritized for recommendations.

-  Location patterns can be used for targeted marketing.

### ⭐Recommendations

- Focus on users aged 25+ who are the most active.

- Promote modern movies (2000–2023) since they attract the most ratings.

- Target Abuja users, especially females, who engage more.

- Highlight movies with average ratings of 4.0 and above.

- Prioritize popular genres other than Sci-Fi.

- Encourage active users with badges or rewards.

- Use location-based trends to improve content suggestions.
  
## Connect with me on my socials:
[Linkedln](www.linkedin.com/in/yvettemefendja)

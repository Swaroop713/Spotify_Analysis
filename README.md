# Spotify Advanced SQL Project 

![Spotify Logo](https://github.com/najirh/najirh-Spotify-Data-Analysis-using-SQL/blob/main/spotify_logo.jpg)

## Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using **SQL**. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced). The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

```sql
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```
## Project Steps

### 1. Data Exploration
Before diving into SQL, it’s important to understand the dataset thoroughly. The dataset contains attributes such as:
- `Artist`: The performer of the track.
- `Track`: The name of the song.
- `Album`: The album to which the track belongs.
- `Album_type`: The type of album (e.g., single or album).
- Various metrics such as `danceability`, `energy`, `loudness`, `tempo`, and more.

### 2. Querying the Data
After the data is inserted, various SQL queries can be written to explore and analyze the data. Queries are categorized into **easy**, **medium**, and **advanced** levels to help progressively develop SQL proficiency.

#### Easy Queries
- Simple data retrieval, filtering, and basic aggregations.
  
#### Medium Queries
- More complex queries involving grouping, aggregation functions, and joins.
  
#### Advanced Queries
- Nested subqueries, window functions, CTEs.
  
---

## EXPLORATORY DATA ANALYSIS

### Easy Level
1. Retrieve the names of all tracks that have more than 1 billion streams.
```sql
SELECT track FROM spotify
WHERE stream > 1000000000;
```
2. List all albums along with their respective artists.
```sql
SELECT DISTINCT album, artist
FROM spotify
order by 1;

```
3. Get the total number of comments for tracks where `licensed = TRUE`.
```sql
SELECT sum(comments) AS No_of_Comments
FROM spotify
WHERE licensed = 'TRUE';
```
4. Find all tracks that belong to the album type `single`.
```sql
SELECT track FROM spotify
WHERE album_type = 'single'; 
```
5. Count the total number of tracks by each artist.
```sql
SELECT artist, COUNT(track) AS total_songs
FROM spotify
GROUP BY 1
ORDER BY 2;
```

### Medium Level
1. Calculate the average danceability of tracks in each album.
```sql
SELECT album, AVG(danceability) AS avg_danceability
FROM spotify
GROUP BY 1
ORDER BY 2 DESC;
```
2. Find the top 5 tracks with the highest energy values.
```sql
SELECT track, MAX(energy) AS max_energy
FROM spotify
GROUP BY track
ORDER BY 2 DESC
LIMIT 5;
```
3. List all tracks along with their views and likes where `official_video = TRUE`.
```sql
SELECT track, 
SUM(views) AS total_views, 
SUM(likes) AS total_likes
FROM spotify
WHERE official_video = 'TRUE'
GROUP BY 1
ORDER BY 2 DESC;
```
4. For each album, calculate the total views of all associated tracks.
```sql
SELECT album, track, SUM(views)
FROM spotify
GROUP BY 1,2
ORDER BY 3 DESC;
```
5. Retrieve the track names that have been streamed on Spotify more than YouTube.
```sql
SELECT * FROM
(SELECT track,
COALESCE(SUM(CASE WHEN most_played_on = 'Youtube' THEN stream END),0) AS streamed_on_youtube,
COALESCE(SUM(CASE WHEN most_played_on = 'Spotify' THEN stream END),0) AS streamed_on_spotify
FROM spotify
WHERE 2 > 1
group by 1
) AS t1
WHERE streamed_on_spotify > streamed_on_youtube
AND streamed_on_youtube <> 0;
```

### Advanced Level
1. Find the top 3 most-viewed tracks for each artist using window functions.
```sql
WITH ranking_artist
AS 
(SELECT artist,
	   track,
	   SUM(views) AS total_views,
	   DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views) DESC) AS rank 
FROM spotify
GROUP BY 1,2
ORDER BY 1, 3 DESC)
SELECT * FROM ranking_artist
WHERE rank <=3;
```
2. Write a query to find tracks where the liveness score is above the average.
```sql
SELECT track, 
	   artist,
	   liveness
FROM spotify
WHERE liveness > (SELECT AVG(liveness)
				  FROM spotify
				  );
```
3. **Use a `WITH` clause to calculate the difference between the highest and lowest energy values for tracks in each album.**
```sql
WITH cte
AS
(SELECT 
	album,
	MAX(energy) as highest_energy,
	MIN(energy) as lowest_energery
FROM spotify
GROUP BY 1
)
SELECT 
	album,
	highest_energy - lowest_energery as energy_diff
FROM cte
ORDER BY 2 DESC
```
4. Find tracks where the energy-to-liveness ratio is greater than 1.2.
```sql
SELECT track, 
	   energy, 
	   liveness
FROM spotify
WHERE liveness/energy >=2;
```


Here’s an updated section for your **Spotify Advanced SQL Project and Query Optimization** README, focusing on the query optimization task you performed. You can include the specific screenshots and graphs as described.

---


## Technology Stack
- **Database**: PostgreSQL
- **SQL Queries**: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
- **Tools**: pgAdmin 4 (or any SQL editor), PostgreSQL (via Homebrew, Docker, or direct installation)

## How to Run the Project
1. Install PostgreSQL and pgAdmin (if not already installed).
2. Set up the database schema and tables using the provided normalization structure.
3. Insert the sample data into the respective tables.
4. Execute SQL queries to solve the listed problems.
5. Explore query optimization techniques for large datasets.

---

## Contributing
If you would like to contribute to this project, feel free to fork the repository, submit pull requests, or raise issues.

---

## THANKYOU
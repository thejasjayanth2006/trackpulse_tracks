# TrackPulse — Music Analytics SQL Project 
[Click Here to get Dataset](trackpulse_dataset.csv)

![TrackPulse Logo](trackpulse_logo_banner.png)

## Overview
This project involves analyzing a music dataset with various attributes about tracks, albums, and artists using SQL. It covers an end-to-end process of cleaning and restructuring a raw dataset, performing SQL queries of varying complexity (easy, intermediate, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights into music engagement and streaming trends.
```sql
-- create table
DROP TABLE IF EXISTS trackpulse_tracks;
CREATE TABLE trackpulse_tracks (
    track_id INT PRIMARY KEY,
    artist_name VARCHAR(255),
    track_name VARCHAR(255),
    album_name VARCHAR(255),
    album_type VARCHAR(50),
    video_title VARCHAR(255),
    channel_name VARCHAR(255),
    platform VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_sec FLOAT,
    mood_category VARCHAR(50),
    views BIGINT,
    likes BIGINT,
    comments BIGINT,
    streams BIGINT,
    engagement_rate FLOAT,
    like_ratio FLOAT,
    popularity_score FLOAT,
    energy_liveness_ratio FLOAT,
    is_licensed SMALLINT,
    is_official_video SMALLINT
);
```
## Project Steps

### 1. Data Exploration
Before diving into SQL, it's important to understand the dataset thoroughly. The dataset contains attributes such as:

- `artist_name`: The performer of the track.
- `track_name`: The name of the song.
- `album_name`: The album to which the track belongs.
- `album_type`: The type of album (e.g., single or album).
- `mood_category`: A derived label (Happy/Energetic, Calm/Positive, Angry/Intense, Sad/Mellow) based on valence and energy.
- Various metrics such as `danceability`, `energy`, `loudness`, `tempo`, and more.

## 2. Basic Data Queries

Before writing analytical queries, a few basic exploratory queries were run to get a feel for the dataset's shape and scale:

```sql
SELECT COUNT(*) FROM trackpulse_tracks;
SELECT COUNT(DISTINCT artist_name) FROM trackpulse_tracks;
SELECT COUNT(DISTINCT album_name) FROM trackpulse_tracks;
SELECT DISTINCT album_type FROM trackpulse_tracks;
SELECT MAX(duration_sec) FROM trackpulse_tracks;
SELECT MIN(duration_sec) FROM trackpulse_tracks;
SELECT DISTINCT platform FROM trackpulse_tracks;
```

These confirmed the total row count, the number of unique artists and albums, the distinct album types and platforms present, and the range of track durations — laying the groundwork before moving to grouped and analytical queries.

## 3. Querying the Data

After the data is inserted, various SQL queries can be written to explore and analyze the data. Queries are categorized into easy, intermediate, and advanced levels to help progressively develop SQL proficiency.

**Easy Queries**
- Simple data retrieval, filtering, and basic aggregations.

**Intermediate Queries**
- More complex queries involving grouping, aggregation functions, and joins.

**Advanced Queries**
- Nested subqueries, window functions, CTEs, and performance optimization.
  
## 4. Query Optimization

In advanced stages, the focus shifts to improving query performance. Some optimization strategies include:

- **Indexing**: Adding indexes on frequently queried columns.
- **Query Execution Plan**: Using `EXPLAIN ANALYZE` to review and refine query performance.
  
---

## 8 Practice Questions

### Easy Level
1. Retrieve the names of all tracks that have more than 1 billion streams.
```sql
SELECT track_name
FROM trackpulse_tracks
WHERE streams > 1000000000;
```

2. List all albums along with their respective artists.
```sql
SELECT DISTINCT album_name, artist_name
FROM trackpulse_tracks
ORDER BY artist_name;
```

3. Get the total number of comments for tracks where `is_licensed = TRUE`.
```sql
SELECT SUM(comments) AS total_comments
FROM trackpulse_tracks
WHERE is_licensed = TRUE;
```
4. Find all tracks that belong to the album type `single`.
```sql
SELECT track_name, artist_name
FROM trackpulse_tracks
WHERE album_type = 'single';
```

### Intermediate Level
1. Calculate the average danceability of tracks in each album.
```sql
SELECT album_name, ROUND(AVG(danceability)::numeric, 3) AS avg_danceability
FROM trackpulse_tracks
GROUP BY album_name
ORDER BY avg_danceability DESC;
```
2. For each album, calculate the total views of all associated tracks.
```sql
SELECT album_name, SUM(views) AS total_views
FROM trackpulse_tracks
GROUP BY album_name
ORDER BY total_views DESC;
```

### Advanced Level
1. Find the top 3 most-viewed tracks for each artist using window functions.
```sql
SELECT artist_name, track_name, views
FROM (
    SELECT artist_name, track_name, views,
           ROW_NUMBER() OVER (PARTITION BY artist_name ORDER BY views DESC) AS rn
    FROM trackpulse_tracks
) ranked
WHERE rn <= 3;
```
2. Retrieve track names where Spotify streams exceed YouTube views.
```sql
SELECT track_name, streams, views
FROM trackpulse_tracks
WHERE streams > views;
```
---
## Technology Stack
- **Database**: PostgreSQL
- **SQL Queries**: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
- **Tools**: pgAdmin 4 (or any SQL editor), PostgreSQL (via Homebrew, Docker, or direct installation)

## How to Run the Project
1. Install PostgreSQL and pgAdmin (if not already installed).
2. Set up the database schema and table using the provided `trackpulse_tracks` structure.
3. Import the cleaned dataset (`trackpulse_dataset.csv`) into the table.
4. Execute SQL queries to solve the listed problems.
5. Explore query optimization techniques for large datasets.
---

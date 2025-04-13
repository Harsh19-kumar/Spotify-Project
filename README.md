# Spotify Advanced SQL Project and Query Optimization P-6
Project Category: Advanced
[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

![Spotify Logo](https://github.com/najirh/najirh-Spotify-Data-Analysis-using-SQL/blob/main/spotify_logo.jpg)

## Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using **SQL**. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

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

### 4. Querying the Data
After the data is inserted, various SQL queries can be written to explore and analyze the data. Queries are categorized into **easy**, **medium**, and **advanced** levels to help progressively develop SQL proficiency.

  
#### Advanced Queries
- Nested subqueries, window functions, CTEs, and performance optimization.

### Advanced Level
1. Find the top 3 most-viewed tracks for each artist using window functions.
  ```sql
 with Artist_ranking
   as
(Select 
Artist,
track,
sum(views) as Total_view,
Dense_rank() OVER(partition by Artist 
order by sum(views)DESC) as Ranking 
 from spotify 
group by 1,2
 order by 1,3 Desc) 
select *  from Artist_ranking
where ranking <=3;



3. Write a query to find tracks where the liveness score is above the average.Top 5 tracks with liveness greater than the average ??
   ```sql
   SELECT 
    track, aRtist, liveness
FROM
    spotify
WHERE
    liveness > (SELECT 
            AVG(Liveness)
        FROM
            spotify) 
ORDER BY 
    Liveness DESC
LIMIT 5;
```
   

4. **Use a `WITH` clause to calculate the difference between the highest and lowest LOUDNESS values for tracks in each album.**
```sql
WITH cte
AS
(SELECT 
	album,
	MAX(energy) as highest_LOUDNESS,
	MIN(energy) as lowest_LOUDNESS
FROM spotify
GROUP BY 1
)
SELECT 
	album,
	highest_LOUDNESS - lowest_LOUDNESS as loudness_diff
FROM cte
ORDER BY 2 DESC
```

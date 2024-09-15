# Spotify-Analysis-Proj
## Introduction

This Spotify Analytics Data Engineering project utilizes AWS to build a robust data pipeline for analyzing user behavior, track performance, and artist metrics. Glue ETL handles data transformation, while Athena executes queries, and QuickSight provides real-time visualizations, offering insights into user interactions, track popularity, and album performance.

## Architecture

![Project Architecture](./Spotify%20Project%20Architecture.jpeg)

## Technologies Used

1.Scripting Language - SQL

2.Amazon Web Services

   - S3 Storage
   - Glue ETL
   - Glue Crawler
   - Athena
   - Quicksight

## Dataset Used 

This project utilizes two Spotify datasets sourced from Kaggle: one capturing user behavior (with columns like `Age`, `Gender`, `skip_rate`, `total_streams`, `total_saves`, and `total_shares`), and another focusing on artist, album, and track information (including `track_name`, `track_popularity`, `album_name`, `release_date`, `label`, and `album_popularity`). Both datasets have been enriched with additional metrics such as `total_album_streams`, `total_album_saves`, and `total_album_shares`, allowing for a more detailed analysis of user interactions, track performance, and artist popularity.

Original Datasets: 

https://www.kaggle.com/datasets/tonygordonjr/spotify-dataset-2023

https://www.kaggle.com/datasets/meeraajayakumar/spotify-user-behavior-dataset

## Data Model
![Project Data mode](./Data%20Model%20Spotify.jpeg)

The project includes a data model consisting of two distinct tables, each representing separate segments of Spotify data: one focused on user behavior and the other on artist, album, and track information. While these tables do not directly interact, as they originate from different datasets with unique focuses, they collectively provide a comprehensive view of Spotify's ecosystem. By analyzing each segment independently, the project captures valuable insights into both user engagement patterns and the performance of artists, albums, and tracks.

## Scripts Used

### Date Format Adjustment Script

Due to QuickSight's date format compatibility issues, I used the following SQL query to split and convert the `release_date` field into separate `year` and `month` columns, allowing for easier analysis.

```sql
SELECT 
    *,  
    split(release_date, '/')[3] AS release_date_year, -- Extract year
    CASE split(release_date, '/')[2]  -- Extract month (2nd element) and convert to month name
        WHEN '01' THEN 'January'
        WHEN '02' THEN 'February'
        WHEN '03' THEN 'March'
        WHEN '04' THEN 'April'
        WHEN '05' THEN 'May'
        WHEN '06' THEN 'June'
        WHEN '07' THEN 'July'
        WHEN '08' THEN 'August'
        WHEN '09' THEN 'September'
        WHEN '10' THEN 'October'
        WHEN '11' THEN 'November'
        WHEN '12' THEN 'December'
    END AS release_date_month -- Add calculated field for month name
FROM 
    datawarehouse;
```


## Issues Faced

**Date Format Incompatibility:** Despite adjusting the schema within QuickSight, the platform continued to reject the original date format. To resolve this, I split the date field into separate year and month columns, which allowed QuickSight to properly interpret the data. This solution enabled time-based analysis and ensured compatibility with QuickSight's date format requirements.

**Table Schema Recognition Issue:** QuickSight failed to automatically recognize the table schema, preventing direct use of SQL queries for analysis. To bypass this limitation, I leveraged calculated columns within QuickSight to generate the required metrics, allowing the analysis to proceed effectively despite the schema detection issue.

## Dashboard


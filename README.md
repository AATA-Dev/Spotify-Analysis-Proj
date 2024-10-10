# Spotify-Analysis-Proj
## Introduction

This Spotify Analytics Data Engineering project aims to build a scalable data pipeline using AWS, designed to deliver detailed insights into user behaviour and artist performance. For example, Spotify’s product team can utilise this pipeline to analyse how users interact with specific tracks, playlists, and albums. By using AWS Glue ETL for efficient data transformation, Athena for querying, and QuickSight for real-time visualisations, the team can track song popularity, listening habits, and album trends.

This system allows Spotify to monitor key metrics such as play count, skips, likes, and playlist additions, helping optimise recommendations and improve user retention. It also enables Spotify’s A&R and artist relations teams to provide artists with detailed analytics on their streaming performance, supporting the development of promotional strategies and boosting fan engagement.


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
![Project Data mode](./Project%20Data%20Model%20Spotify.jpeg)

The project features a data model with a standalone user table and separate, interconnected tables for artist, track, and album data. The user table captures behavioral data, while the artist, track, and album tables are linked through track IDs and artist IDs, offering a cohesive view of music-related information. Although the user data remains independent from the artist, track, and album data, this structure allows for focused analysis of both user engagement and music performance. Together, these tables provide a comprehensive view of Spotify’s ecosystem, highlighting user behavior alongside trends in artist, track, and album performance.

## Executive Summary 

To enhance user growth, retention, and revenue, several strategic recommendations can be implemented.

First, incentivizing Premium subscriptions by tailoring offers to the core demographic (ages 20-35) and younger users can drive growth. Focus on exclusive music content, personalized playlists, and lifestyle-relevant features such as offline listening for travel or leisure to make Premium more appealing. 

Additionally, expanding youth engagement through a student discount or youth-oriented Premium plan, verified by university credentials, would attract the 13-20 age group, who are price-sensitive but highly engaged with music. Content diversification is also key—while music remains dominant, promoting podcasts with highly personalized recommendations could encourage broader content consumption and increase platform stickiness.

Lastly, fostering fan-centric features like virtual fan meetups, exclusive content, or artist Q&As would strengthen the connection between users and their favorite artists, leading to higher engagement and retention. By focusing on driving subscriptions, optimizing content offerings, and deepening engagement with younger users, Spotify can improve both its user base and revenue streams.

Below is an overview of the user interactions 

![Dashboard Overview](./Dashboard%20Overview.jpg)


## Outcome Highlights and Recommendations

### User Insights
- The Free subscription plan is more popular than Premium, with a significant user base aged 20-35.
- Music dominates content preferences, while podcasts have room for growth.
  
**Recommendation:**  
Drive Premium conversions by highlighting features like offline listening and higher audio quality, especially for leisure and travel use.

### Track and Artist Performance
- Columbia Records and Warner Records lead in popular track representation.

**Recommendation:**  
Strengthen partnerships with top labels and provide more visibility to independent artists through curated playlists and promotional efforts.

### Content and Genre Focus
- Pop and K-pop are the leading genres, with Karaoke holding the most followers.

**Recommendation:**  
Increase engagement by offering exclusive content for Pop and K-pop fans and supporting emerging genres like ATL Hip Hop.

### Top Artist Analysis
- Taylor Swift leads as the most popular artist, with her album *“Red (Taylor's Version)”* being a top performer.

**Recommendation:**  
Leverage exclusive content and fan-centric features, such as virtual concerts and early album access, to incentivize Premium upgrades and boost artist engagement.


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

**Dataset Limitations**

Although I extended the dataset using generative processes and Excel, certain limitations remain. Access to sensitive user details—such as demographics, location, and payment history—continues to be restricted due to Spotify's privacy and data protection policies. This restriction limits the ability to explore more advanced personalisation insights, such as detailed audience segmentation and user-specific recommendations.

**Date Format Incompatibility:** 

Even after adjusting the schema within QuickSight, the platform continued to reject the original date format. To resolve this, I split the date field into separate year and month columns, which allowed QuickSight to properly interpret the data. This solution enabled time-based analysis and ensured compatibility with QuickSight's date format requirements.

**Table Schema Recognition Issue:** 

QuickSight failed to automatically recognize the table schema, preventing direct use of SQL queries for analysis. To bypass this limitation, I leveraged calculated columns within QuickSight to generate the required metrics, allowing the analysis to proceed effectively despite the schema detection issue.

**ETL Job Replication for Schema Resolution:** 

While leveraging calculated columns in QuickSight provided a temporary workaround for the schema recognition issue, it introduced limitations in handling larger datasets and more complex transformations. To ensure a more scalable and efficient solution, I replicated the data model using an ETL job in AWS Glue. This allowed for comprehensive pre-processing of the data, including schema restructuring and metric calculations, before it was loaded into Athena for querying. By offloading the transformations to Glue, I ensured that QuickSight could seamlessly interpret the data without further schema issues, enabling smoother, faster analysis with improved query performance. This solution resolved the schema recognition limitations while streamlining the overall data pipeline.


## Dashboard


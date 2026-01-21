# Documentation for Music Data
## Overview
Our project has a small music database and a REST API. The goal is to design a relational schema, load data into Supabase (PostgreSQL), run SQL queries, and test CRUDL operations with Postman. The database has artists and tracks, connects tracks to artists, and keeps chart rankings with platform statistics (Spotify/YouTube). The data comes from Kworb and MusicBrainz. We collected the raw data in Google Colab, cleaned it, made relational tables from them, and then used the final tables in Supabase.
- Kworb (kworb.net)
  - Charts and platform rankings (Spotify, YouTube, Apple Music, iTunes, Billboard Radio).
- MusicBrainz (musicbrainz.org)
  - Extra data about artists (country, type, genres, ratings).

## 1) Google Colab
We used Colab to pull the raw data.
Then we used the MusicBrainz API to get more info on the artists.
Note: We did not import the raw CSVs directly into Supabase, we cleaned the data into our final tables first so that we could use them easily (4 dims, 2 facts, 1 bridge).
So the process was: Raw data (Colab) → cleaned + normalized CSVs → Supabase 

## 2) What “cleaning and normalization” means
We did a few important fixes before importing:
- We created a key called name_key. This helps match the same artist across the 2 different sources we got the artist data from even if formatting or anything changes.
- We added a foreign key to the platforms table so we changed platform names to sets; billboard to billboard_radio.
- We made sure chart names match between tables. Example: the chart is stored as World Wide Songs in both charts and chart_entries.

## 3) Database model
We used 7 tables.
 4 dimensions, 2 facts, and one bridge which connects tracks and artists.
### Dimensions
- platforms: list of platforms (spotify, youtube, apple_music, itunes, billboard_radio)
- charts: list of charts per platform
- artists: artist info and metadata
- tracks: track info
### Bridge 
- track_artists: connects tracks and artists with many to many.
### Fact tables
- chart_entries: chart ranking rows
- Artist_platform_stats: monthly listeners etc.

## 4) Constraints 
- Primary keys
- Foreign keys for references
 (example: chart_entries.platform must exist in platforms)
- Unique key on artists.name_key to avoid duplicate artists.

## 5) SQL queries 
- SELECT with filtering and sorting
- JOINs (inner and left)
- GROUP BY and COUNT
- multi-table joins (charts → tracks → artists)
- top-10 queries

## 6) REST API (Postman)
Supabase REST API; 
We did CRUDL:
- Create (POST)
- Read (GET)
- Update (PATCH)
- Delete (DELETE)
- List (GET)

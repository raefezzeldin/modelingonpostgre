# Project: Data Modeling with Postgres

A startup named Sparkify wants to analyze user activities using their song and
user data. The current data is spread among several JSON files, making it hard
to query and analyze.

This project aims to create an ETL pipeline to load song and user data to a
Postgres database, making it easier to query and analyze data.

## Datasets

Data is currently collected for song and user activities, in two directories:
`data/log_data` and `data/song_data`, using JSON files.

### Song dataset format

```json
{
  "num_songs": 1,
  "artist_id": "ARGSJW91187B9B1D6B",
  "artist_latitude": 35.21962,
  "artist_longitude": -80.01955,
  "artist_location": "North Carolina",
  "artist_name": "JennyAnyKind",
  "song_id": "SOQHXMF12AB0182363",
  "title": "Young Boy Blues",
  "duration": 218.77506,
  "year": 0
}
```

### Log dataset format

```json
{
  "artist": "Survivor",
  "auth": "Logged In",
  "firstName": "Jayden",
  "gender": "M",
  "itemInSession": 0,
  "lastName": "Fox",
  "length": 245.36771,
  "level": "free",
  "location": "New Orleans-Metairie, LA",
  "method": "PUT",
  "page": "NextSong",
  "registration": 1541033612796,
  "sessionId": 100,
  "song": "Eye Of The Tiger",
  "status": 200,
  "ts": 1541110994796,
  "userAgent": "\"Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36\"",
  "userId": "101"
}
```

## Schema

### Fact tables

#### Songplays

Records in log data associated with song plays i.e. records with `page` set to
`NextSong`.

|   Column    |            Type             | Nullable |
| ----------- | --------------------------- | -------- |
| songplay_id | integer                     | not null |
| start_time  | timestamp without time zone | not null |
| user_id     | integer                     | not null |
| level       | character varying           | not null |
| song_id     | character varying(18)       |          |
| artist_id   | character varying(18)       |          |
| session_id  | integer                     | not null |
| location    | character varying           | not null |
| user_agent  | character varying           | not null |

Primary key: songplay_id

### Dimension tables

#### Users

Users in the app.

|   Column   |       Type        | Nullable |
| ---------- | ----------------- | -------- |
| user_id    | integer           | not null |
| first_name | character varying | not null |
| last_name  | character varying | not null |
| gender     | character(1)      | not null |
| level      | character varying | not null |

Primary key: user_id

#### Songs

Songs in music database.

|  Column   |         Type          | Nullable |
| --------- | --------------------- | -------- |
| song_id   | character varying(18) | not null |
| title     | character varying     | not null |
| artist_id | character varying(18) | not null |
| year      | integer               | not null |
| duration  | double precision      | not null |

Primary key: song_id

#### Artists

Artists in music database.

|  Column   |         Type          | Nullable |
| --------- | --------------------- | -------- |
| artist_id | character varying(18) | not null |
| name      | character varying     | not null |
| location  | character varying     | not null |
| latitude  | double precision      |          |
| longitude | double precision      |          |

Primary key: artist_id

#### Time

Timestamps of records in songplays broken down into specific units.

|   Column   |            Type             | Nullable |
| ---------- | --------------------------- | -------- |
| start_time | timestamp without time zone | not null |
| hour       | integer                     | not null |
| day        | integer                     | not null |
| week       | integer                     | not null |
| month      | integer                     | not null |
| year       | integer                     | not null |
| weekday    | integer                     | not null |

## Schema for Song Play Analysis
Using the song and log datasets, you'll need to create a star schema optimized for queries on song play analysis. This includes the following tables.

### Fact Table

#### songplays 
    -records in log data associated with song plays i.e. records with page NextSong
songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

### Dimension Tables

#### users 
    - users in the app
user_id, first_name, last_name, gender, level
#### songs 
    - songs in music database
song_id, title, artist_id, year, duration
#### artists 
    - artists in music database
artist_id, name, location, latitude, longitude
#### time 
    - timestamps of records in songplays broken down into specific units
start_time, hour, day, week, month, year, weekday

### Project Template
To get started with the project, go to the workspace on the next page, where you'll find the project template files. You can work on your project and submit your work through this workspace. Alternatively, you can download the project template files from the Resources folder if you'd like to develop your project locally.

In addition to the data files, the project workspace includes six files:

1- test.ipynb 
    displays the first few rows of each table to let you check your database.
2- create_tables.py 
    drops and creates your tables. You run this file to reset your tables before each time you run your ETL scripts.
3- etl.ipynb 
    reads and processes a single file from song_data and log_data and loads the data into your tables. This notebook contains detailed instructions on the ETL process for each of the tables.
4- etl.py 
    reads and processes files from song_data and log_data and loads them into your tables. You can fill this out based on your work in the ETL notebook.
5- sql_queries.py 
    contains all your sql queries, and is imported into the last three files above.
6- README.md 
    provides discussion on your project.

### Project Steps
Below are steps you can follow to complete the project:

Create Tables
Write CREATE statements in sql_queries.py to create each table.
Write DROP statements in sql_queries.py to drop each table if it exists.
Run create_tables.py to create your database and tables.
Run test.ipynb to confirm the creation of your tables with the correct columns. Make sure to click "Restart kernel" to close the connection to the database after running this notebook.

Build ETL Processes
Follow instructions in the etl.ipynb notebook to develop ETL processes for each table. At the end of each table section, or at the end of the notebook, run test.ipynb to confirm that records were successfully inserted into each table. Remember to rerun create_tables.py to reset your tables before each time you run this notebook.

Build ETL Pipeline
Use what you've completed in etl.ipynb to complete etl.py, where you'll process the entire datasets. Remember to run create_tables.py before running etl.py to reset your tables. Run test.ipynb to confirm your records were successfully inserted into each table.

Document Process
Do the following steps in your README.md file.

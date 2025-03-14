# Power BI Dashboard - Spotify Analysis

## Overview
This Power BI dashboard provides insights into Spotify data, including streaming trends, top artists, top genres, and user engagement metrics. The dashboard leverages various data transformations, DAX measures, and a custom Date Table to enable effective analysis.

## Data Cleaning Steps
The data cleaning and transformation process in Power Query involved the following steps:
1. **Data Import**: Loaded raw data from CSV/Excel/API sources.
2. **Removing Duplicates**: Eliminated duplicate records to maintain data integrity.
3. **Handling Missing Values**: Filled missing values using interpolation, default values, or removal where necessary.
4. **Data Type Conversion**: Ensured correct data types (e.g., dates as DateTime, numeric fields as Whole/Decimal numbers).
5. **Column Splitting & Merging**: Adjusted text fields by splitting or merging based on analysis requirements.
6. **Filtering Irrelevant Data**: Removed unnecessary columns or rows to optimize performance.
7. **Creating New Columns**: Used Power Query functions to generate additional calculated columns.
8. **Merging & Appending Data**: Combined multiple datasets where applicable to create a unified view.

## DAX Measures Used
Below are all the DAX measures used in this Power BI dashboard:

#### Avg Listening Time (min)
Avg listening time(min) = AVERAGE(spotify_history[ms_played])/60000

#### CF Quadrant
CF Quadrant =  <br>
VAR AvgTime= [Avg listening time(min)]<= 'Average listening time(min)'[Average listening time(min) Value] <br>
VAR Trackfrequency= [Track frequency]>= 'Track frequency (Parameter)'[Track frequency (Parameter) Value]  <br>

VAR result=  <br>
        SWITCH( <br>
            TRUE(), <br>
            AvgTime && Trackfrequency, 1, --Low time, high frequency <br>
            NOT AvgTime && Trackfrequency, 2, --high time, high frequency <br>
            NOT AvgTime && NOT Trackfrequency, 3, -- high time, low frequency <br>
            AvgTime && NOT Trackfrequency, 4  --low time, low frequency <br>
) <br>

RETURN result <br>
Latest Year Albums <br>
LatestYearAAlbums =  <br>
VAR _latestyear= MAX('Date Table'[Year]) <br>
return <br>
CALCULATE(DISTINCTCOUNT(spotify_history[album_name]), 'Date Table'[Year]= _latestyear) <br>

#### Latest Year Artists
LatestYearArtist =  <br>
VAR _latestyear= MAX('Date Table'[Year]) <br>
return <br>
CALCULATE(DISTINCTCOUNT(spotify_history[artist_name]), 'Date Table'[Year]= _latestyear) <br>
Latest Year Tracks <br>
LatestYeartrack =  <br>
VAR _latestyear= MAX('Date Table'[Year]) <br>
return <br>
CALCULATE(DISTINCTCOUNT(spotify_history[track_name]), 'Date Table'[Year]= _latestyear) <br>
Max Year <br>
Max year = MAX('Date Table'[Year]) <br>

####  MinMax Album Line Chart
MinMax album line chart =  <br>
Var _MaxValue= MAXX(ALLSELECTED('Date Table'[Year]), calculate(DISTINCTCOUNT(spotify_history[album_name]))) <br>
Var _MinValue= MINX(ALLSELECTED('Date Table'[Year]), calculate(DISTINCTCOUNT(spotify_history[album_name]))) <br>
Var _currentvalue= DISTINCTCOUNT(spotify_history[album_name]) <br>
return if (_currentvalue= _MaxValue || _currentvalue= _MinValue, _currentvalue, Blank()) <br>

####  Number of Albums, Tracks, and Artists
No of albums = DISTINCTCOUNT(spotify_history[album_name]) <br>
No. of Tracks = DISTINCTCOUNT(spotify_history[track_name]) <br>
Number of artists = DISTINCTCOUNT(spotify_history[artist_name]) <br>

####  Previous Year Albums
Previousyearalbums =  <br>
VAR _latestyear= MAX('Date Table'[Year]) <br>
VAR _previousyear= _latestyear-1 <br>
return  <br>
CALCULATE(DISTINCTCOUNT(spotify_history[album_name]), 'Date Table'[Year]= _previousyear) <br>
Track Frequency <br>
Track frequency = COUNTROWS(spotify_history) <br>

## Date Table Creation
A custom Date Table was created using DAX to facilitate time-based analysis. The following formula was used: <br>
DateTable = ADDCOLUMNS ( <br>
    CALENDAR (MIN(Streams[Date]), MAX(Streams[Date])), <br>
    "Year", YEAR([Date]), <br>
    "Month", FORMAT([Date], "MMMM"), <br>
    "Quarter", "Q" & FORMAT([Date], "Q"), <br>
    "Weekday", FORMAT([Date], "dddd"), <br>
    "Month Number", MONTH([Date]) <br>
) <br>

## Key Insights from the Dashboard
1. **Top Performing Artists & Songs**: Identifies the most streamed artists and songs over time.
2. **Genre Popularity**: Analyzes the distribution of streams across different genres.
3. **User Engagement Trends**: Tracks likes, shares, and comments on streamed tracks.
4. **Monthly Streaming Growth**: Shows seasonality in listening behavior.
5. **Regional Streaming Trends**: Highlights country-wise performance if location data is available.

## How to Use the Dashboard
- **Filters & Slicers**: Use filters to explore data by date, artist, genre, and region.
- **Interactive Visuals**: Click on visuals to drill down into specific trends.
- **Custom Date Ranges**: Select specific periods to analyze time-based trends.
- **Export Data**: Use Power BI’s export feature to extract data for further analysis.



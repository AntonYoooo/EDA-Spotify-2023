# EDA-Spotify-2023
This repository includes the documentation and analytical application revolving around the dataset of the most streamed Spotify Songs in 2023. 
## Brief Outline of the DataSet
The Dataset "Most Streamed Spotify Songs 2023" is a csv file that contains the list of the most famous songs found on the application "Spotify". It contains different data including track names, artists involved, release date, amount of streams, apple music presence, deezer presence, shazam charts, and other musical attributes such as danceability, bpm, etc. 
## Goal 
Perform Data Analysis to determine the correlation between data or attributes, uncover trends, and examine the relationship between variables, in order to formulate a conclusion as to how a track is likely to become popular. 

## Phase 1: Start + Identifying/Analyzing the Initial Dataset
- In this part, we simply analyze the initial dataset and check whether there are any missing values or errors that may hinder the coding process. 

**1 . a . ) In order to start, it is essential to first declare and import the necessary libraries in order for the program to work**
```python
#syntax for accessing pandas library in python
import pandas as pd 
#syntax for accessing Matplotlib library
import matplotlib.pyplot as plt
#syntax for acccessing seaborn library
import seaborn as sns
#syntax for accessing numpy library
import numpy as np
```
**1 . b . ) After declaring the libraries, the dataset is then accessed. Note that in this case, the csv file contains characters that are undefined or unrecognizable by the Panda's default encoding (UTF-8), which is why encoding latin1 is used for this case.**
```python
#the syntax encoding ='latin' ensures that no form of "UnicodeDecodeError" occurs when loading a file that potentially contains characters invalid in UTF-8 (Pandas' Default encoding)
df = pd.read_csv('spotify-2023.csv', encoding='latin') 
df
```

### Output: 

![image](https://github.com/user-attachments/assets/2f0666ef-9b40-436d-acb7-7288bc7a71f5)

**Note:** In the image provided, there are a total of 953 rows and 24 columns. 
<br> 

**1 . c . ) Now that the dataset is accessed, it will then undergo a process of checking before performing data analysis. First, we investigate the datatypes between the columns through this code:** 
```python 
# Identifies the data type of the columns 
df_DT = df.dtypes
df_DT
``` 
### Output: 
![image](https://github.com/user-attachments/assets/8307e5ef-caa7-41cf-a31b-431571e9135b)

**Note:** It can be observed that some of the columns in the dataset such as streams, in_deezer_playlists, and in_shazam charts are declared as "object", in which they should be numeric since they're values are numerical.
<br> 

**1 . d . )  After checking out the data types, we then now proceed in checking whether incomplete elements or duplicates are present within the dataset.** 
```python
# Creates a variable 'Incomplete_sets' which will store the sum of the rows with incomplete data
Incomplete_sets = pd.isnull(df).sum()
Incomplete_sets = Incomplete_sets[Incomplete_sets > 0] 

# Locates and stores the rows with duplicate track and artist name 
Duplicates = df.duplicated(['track_name', 'artist(s)_name']).sum() 

# Prints the number of columns with Incomplete Data and the Amount of Duplicate Tracks 
print("The Columns with Incomplete Data are:") 
print(Incomplete_sets)
print("The Amount of Tracks with a duplicate are:", Duplicates) 
```
### Output: 
![image](https://github.com/user-attachments/assets/95e04788-4574-4f14-84bd-881a41723106)

**Note:** According to the output, there are 50 blanks in the "in_shazam_charts" column and 95 blanks in the "key" column. Additionally, there are currently 4 tracks that have been duplicated within the dataset. 
<br> 

**1 . e . ) To further prove that there are duplicates within the track, the following code is then implemented:** 
```python
# Identifies the rows with duplicates on the 'track_name' and 'artist(s)_name' column
# Keep = False makes it so that all duplicates and its original entries will be included
duplicate_rows = df[df.duplicated(['track_name', 'artist(s)_name'], keep=False)]

# Display the duplicate rows
duplicate_rows
```
### Output: 
![image](https://github.com/user-attachments/assets/aaa14d47-e2f1-488c-9720-6398d662a20b)

## Phase 2: Data Cleaning 
- In this part, we apply Data Cleaning in order to ensure the accuracy and consistency of the dataset. 

**2 . a . ) To Start cleaning the data, we first remove the rows with empty or missing values within their columns. Alongside this, we'll also convert the 3 incorrect datatypes into their respective type (Numeric)**  
```python
#Creates a variable named new_df which removes all rows with incomplete data
new_df = df.dropna(how='any', axis=0) 

#Reassigns the value with its duplicates at columns track_name and artist(s)_name removed
new_df = new_df.drop_duplicates(subset = ['track_name', 'artist(s)_name']) 

#Converts column 'stream' into numerical data type while turning garbage values into NaN simultaneously 
new_df['streams'] = pd.to_numeric(new_df['streams'], errors = 'coerce') 

#Converts data type of columns 'in_deezer_playlists', and 'in_shazam_charts' into numeric 
new_df['in_deezer_playlists'] = pd.to_numeric(new_df['in_deezer_playlists'], errors = 'coerce') 
new_df['in_shazam_charts'] = pd.to_numeric(new_df['in_shazam_charts'], errors = 'coerce') 

#Removes the rows with NaN values in the column "stream"
new_df = new_df.dropna(subset=['streams'])
new_df
``` 
### Output: 
![image](https://github.com/user-attachments/assets/3f916836-b2ba-4d2a-98da-55afcb64eede)

**Note:** After cleaning the data, we are then left with 813 rows and 24 columns. 
<br>

## Phase 3: Basic Descriptive Statistics 
- For this part, we will be applying basic descriptive statistics on the stream, released_year, and artist_count columns in order to effectively visualize and define the data.

**3 . a . ) In performing descriptive statistics, we begin by calculating and determining the mean, median and standard deviation of the stream column in order to describe the data.**
```python
#Converts the stream column to numeric in order to access the column without errors
#Uses .loc in order to avoid the "SettingWithCopyWarning", by only modifying the new_df instead of the original df

#Calculations for Statistics
average_streams = new_df['streams'].mean() #Calculates the mean of the stream column 
track_no = new_df['streams'].count() #Counts the number of tracks or rows 
track_std = new_df['streams'].std() #Calculates the standard deviation of stream column
track_median = new_df['streams'].median() #Calculates the median of the stream column
#Creating Dataframe in order to present the basic descriptive statistics of stream column neatly
table_m = pd.DataFrame({'Number of tracks': [track_no], 
                        'Average Streams': [average_streams], 
                        'Stream Std.': [track_std],
                        'Stream Median': [track_median]})

#Prints the Dataframe/Table                         
table_m
``` 
### Output: 
![image](https://github.com/user-attachments/assets/fddc801a-e3db-4b06-8d49-6726b6f62c0f)

**Note:** Upon further inspection, it is noticeable that total tracks being 813, possesses an average stream of 4.68e+08 (~468 million), standard deviation of 5.23e+08 (~523 million), and a median of 2.63+e08 (~263 million).
<br> 

**3 . b . ) After performing descriptive statistics on the stream column, we then now proceed with the distribution on the column "released_year, formulated via the codeblock:**
```python
# To Identify what makes the outliers of the column 'released_year', we determine the Upper Boundary and Lower Boundary of that column
boundary_year = new_df['released_year'] 
AQ1 = boundary_year.quantile(0.25) 
AQ3 = boundary_year.quantile(0.75) 

# Find the InterQuartile Range to solve for UB and LB 
# Formulas for finding IQR, UB, and LB 
Year_IQR = AQ3 - AQ1 
Year_UB = AQ3 + (1.5)*Year_IQR 
Year_LB = AQ1 - (1.5)*Year_IQR 

# Plot a Histogram that shows the distribution on 'released_year' 
# Adds a displot on the histogram plotted (kernel density estimate being true) 
sns.histplot(data = new_df, x = 'released_year', bins = 100, kde = True) 
plt.title('Released Year of Tracks', fontweight = 'bold') 
plt.xlabel('Year') 
plt.ylabel('Amount of Tracks')
plt.show 

# Creates dots/scatter which will present the location of the UB and LB   
plt.scatter([Year_LB, Year_UB], [50, 50], color='blue', marker='o', s=30, label='Bounds')
plt.legend() 

# Annotating boundaries with different specifications (font size, alignment, color, and location) 
# X axis location for LB is LB itself while Y is at 30, X axis location for UB is UB itself while Y is at 60 
plt.text(Year_LB, 30, f'Year LB: {int(Year_LB)}', color='black', ha='center', fontsize=8)
plt.text(Year_UB, 60, f'Year UB: {int(Year_UB)}', color='black', ha='center', fontsize=8)
plt.legend()

# Prints the plot 
plt.show() 
```
### Output: 
![image](https://github.com/user-attachments/assets/2a07f0c7-b169-4900-abc8-abff761807f5)


**Note:** In determining the distribution, a histogram plot was created in order to create a "face" or a visual representation of the data procured. It can be noticed that upon reaching the years 2019 to 2023, the bar had a sudden spike, indicating that most of the top tracks found in the list possesses a release date within the range. Additionally, it can also be noticed that the 2 boundaries, Lower Bound and Upper Bound, indicate the range of data that considers a data as an "inlier" while anything that is beyond or before that are outliers.
<br> 

**3. c. ) We then now apply the same statistical treatment to the column "artist_count"** 
```python
# Determine the Upper Boundary and Lower Boundary of Artist Count
boundary_artistcount = new_df['artist_count'] 
BQ1 = boundary_artistcount.quantile(0.25) 
BQ3 = boundary_artistcount.quantile(0.75) 

# Find the InterQuartile Range to solve for UB and LB 
# Formulas for finding IQR, UB, and LB 
Artist_IQR = BQ3 - BQ1 
Artist_UB = BQ3 + (1.5) * Artist_IQR  # Use Artist_IQR here
Artist_LB = BQ1 - (1.5) * Artist_IQR  # Use Artist_IQR here

# Plot a Histogram that shows the distribution on 'artist_count'
# Adds a displot on the histogram plotted (kernel density estimate being true) 
sns.histplot(data = new_df, x = 'artist_count', bins = 40, kde = True) 
plt.title('Artist Count(s)') 
plt.xlabel('Amount of Artist(s)') 
plt.ylabel('Amount of Tracks')

# Creates dots/scatter which will present where the UB and LB is located  
plt.scatter([Artist_LB, Artist_UB], [50, 50], color='blue', marker='o', s=30, label='Bounds')
plt.legend() 

# Annotating boundaries with different specifications (font size, alignment, color, and location) 
# X axis location for LB is LB itself while Y is at 30, X axis location for UB is UB itself while Y is at 60 
plt.text(Artist_LB, 60, f'Artist LB: {Artist_LB:.1f}', color='black', ha='center', fontsize=8)  # Use Artist_LB
plt.text(Artist_UB, 60, f'Artist UB: {Artist_UB:.1f}', color='black', ha='center', fontsize=8)  # Use Artist_UB
plt.legend()

# Prints the Plot 
plt.show() 
```
### Output: 
![image](https://github.com/user-attachments/assets/a7e9ac6c-e2b0-41b4-8a35-dfb82c9c2ca5)

**Note:** Based on the data shown above, it can be observed that the highest number of artist count(s) when it comes to creating tracks are singular, while other counts are distributed to the values 2, 3, 4, 5. Similar to the previous data, an Upper Bound and Lower Bound was calculated and formulated in which the LB comprises a value of -0.5, indicating that there are no lower outliers, since having an artist count that is equal or less than 0 is impossible. On the other hand, the upper bound is found at a value of 3.5, suggesting that most of the artist count fall within the range of 1 to 3.
<br>

## Phase 4: Top Performers 
- For this part, we will be sorting out the top 5 tracks with the highest streams and the top 5 artists with the most number of tracks featured in the top spotify list

**4. a. ) In order to complete this part, we begin by determining the top 5 tracks based on the amount of streams.** 
```python
#Create variable x to store the top 5 tracks with the largest streams
x = new_df.nlargest(5, 'streams')

#Create Dataframe to present the top 5 most streamed tracks
table_streams = pd.DataFrame({'Artist(s)': x['artist(s)_name'], 'Track Name': x['track_name'], 'Streams': x['streams']})

#Prints the Dataframe 
table_streams
```
### Output: 
![image](https://github.com/user-attachments/assets/fe6fdc85-e58e-4122-8c80-5111f0c37e8a)

**Note:** The track with the highest streams is (1) Shape of you by Ed Sheeran, followed with (2) Sunflower by Post Malone and Swae Lee, (3) One Dance by Drake, Wizkid and Kyla, (4) Stay by Justin Bieber and Kid Laroi, and (5) Believer by Imagine Dragons.
<br>


**4. b. ) Next, we determine the top performers based on the amount of times a song of theirs were included in the top spotify list.** 
```python 
# Create a variable "split" which will contain the splitted and expanded version of the column 'artist(s)_name'
# This will identify and expand the entries in the "artist(s)_name" column seperated by a comma "," in the dataset
splitted = new_df['artist(s)_name'].str.split(', ', expand = True) 

# This will stack the splitted and expanded artist into a single column  
stacked = splitted.stack() 

# Identify and store the values of the top 5 most frequent artist names appeared in "most_frequent" 
most_frequent = stacked.value_counts().nlargest(5) 

# Create DataFrame to present the artist names and the amount of times their tracks showed up in the dataset
# Index corresponds to the name of the artists while values corresponds to their number of tracks
table_frequency = pd.DataFrame({'Artist': most_frequent.index, 'No. of Tracks': most_frequent.values})

#Prints the DataFrame 
table_frequency 
```
### Output: 
![image](https://github.com/user-attachments/assets/3dd75c26-5a82-43d1-983c-7fa4151a2ac4)

**Note:** The Artist with the highest number of tracks is (36)Bad Bunny, followed by (32)Taylor Swift, (26)The Weeknd, (23)Kendrick Lamar, and (21)Feid. 
<br>

## Phase 5: Temporal Trends 
- For this part, we will be analyzing the trends in the number of tracks released based on both years and months in order to see patterns or fluctuations when it comes to release schedule.

**5. a. ) Start with plotting the tracks released yearly by utilizing seaborn's histogram plot** 
```python
# Use the syntax groupby in order to Group the data by released_year and count the number of tracks
Yearly_Tracks = new_df.groupby('released_year').size().reset_index(name = 'track_count') 

# Creates a barplot using seaborn to represent the data 'Yearly_Tracks' 
sns.barplot(data = Yearly_Tracks, x = 'released_year', y = 'track_count') 
plt.title('Tracks Released Per Year', fontweight = 'bold') 
plt.xlabel('Year')
plt.ylabel('Tracks Released')

# Sets the ticks/increment of years to avoid crowding on the x axis 
# Sets the range from 0 (Initial or the Oldest Year released) to the latest with an increment of 5
# Applies Slicing to grab every 5th element within the range (E.g., 1930, 1935, 1940, 1945, 1950 --> 1930, 1950) 
# The Years shown in the barplot indicates the most notable years of releases (5 Years increment is only applied when tracks are actually released within that 5 year, meaning if no tracks are released like the years 1931-1957, it automatically jumps to 1958)
plt.xticks(ticks=range(0, len(Yearly_Tracks), 5), labels=Yearly_Tracks['released_year'][::5])

# Prints the Plot 
plt.show()
```
### Output: 
![image](https://github.com/user-attachments/assets/3e037b82-a611-4cd5-aa06-5e6f0873ca22)

**Note:** Similarly with the analysis presented on 3.b., the release year of the majority of the tracks can be found in between the range of 2019 to 2023.
<br>

**5. b. ) Next is plotting the tracks based on months. Seaborn's histogram will again be utilized here.** 
```python
# Use the syntax groupby in order to Group the data by released_year and count the number of tracks
Monthly_Tracks = new_df.groupby('released_month').size().reset_index(name = 'track_count') 

# Creates a barplot using seaborn to represent the data 'Monthly_Tracks' 
sns.barplot(data = Monthly_Tracks, x = 'released_month', y = 'track_count') 
plt.title('Tracks Released Per Month', fontweight = 'bold') 
plt.xlabel('Year')
plt.ylabel('Tracks Released')

# Prints the Plot 
plt.show()
```
### Output: 
![image](https://github.com/user-attachments/assets/25bfced6-9dac-41aa-b77a-4b04e0d828a9)

**Note:** It can be noticed that the month with the most releases is the 1st (January) and 5th (May) month while the least release is on the 8th (August) month. Possible reasons as to why January and May see the most release is due to the fact that January indicates the start of a new year while May is the kickstart of summer and festivals. As for August, the lower number of releases may be correlated to the fact that this month is the end of summer, when activities tend to die down.
<br>

## Phase 6: Genre and Music Characteristics 
- For this part, the correlation between streams and various musical attributes are examined in order to identify which among the attributes have the most impact on the stream column 

**6. a. ) To determine the correlation between the variables, we first calculate for their correlational value, then organize them via a correlation table. We will be utilizing seaborn's heatmap in order to make it visually appealing. Additionally, we will also be creating a dataframe which will present the correlational values by order, to make it more easily understandable.** 
```python
# Creates a list of columns containing the correlated attributes and streams 
Correlated_Columns = ['streams', 'bpm', 'danceability_%', 
                      'valence_%', 'energy_%', 'acousticness_%', 
                      'instrumentalness_%', 'liveness_%', 'speechiness_%']  

# Filters out columns from new_df based only on the correlated columns 
Filtered_Correlation = new_df[Correlated_Columns] 

# Calculates the correlation of each columns 
Correlation_Stats = Filtered_Correlation.corr() 

# Plots a heatmap with custom settings for a more effective visualization 
plt.figure(figsize= (8, 8))
plt.title("Correlation between Streams and Musical Attributes", fontsize=16, fontweight = 'bold')
sns.heatmap(Correlation_Stats, annot = True, linewidth = 1, linecolor = 'black', cmap = 'Purples')
plt.show()

# Creates a variable attribute_values which will store the correlation statistics on descending order 
attribute_values = Correlation_Stats['streams'].drop('streams').sort_values(ascending=False)

# Creates a Dataframe that will present the correlation statistics 
Correlation_Table = pd.DataFrame({'Attributes': attribute_values.index, 
                        'Correlation with Stream': attribute_values.values})

# Prints the Heatmap + Table 
print(Correlation_Table)
```
### Output: 
![image](https://github.com/user-attachments/assets/3fcc21be-c7d3-4db1-837f-f40031fa5977)
![image](https://github.com/user-attachments/assets/3bc8d05b-d02d-4ce7-9d0e-306617f0f3c1)

**Note:** Based on the heatmap provided, all attributes seem to have weak negative correlations with the stream column, indicating that there is a minimal impact on their inverse relationship. To conclude, the largest correlation present is the speechiness, possessing a -0.1 coefficient while the weakest would be acousticness, being -0.005.
<br>

**6. b. ) After determining the correlation values of each, we'll then closely examine the correlational values on the attributes "danceability" and "energy", and "valence" and "acousticness".**
```python
danceenergy_corr = Correlation_Stats.loc['danceability_%', 'energy_%']
valenceacoustic_corr = Correlation_Stats.loc['valence_%', 'acousticness_%']

# Create Dataframe that represents the correlation between dance and energy, and valence and acoustic
# Heatmaps won't accept 1 Dimensional arrays, therefore it is essential to turn it into a 2d by using Dataframe
corr1 = pd.DataFrame([[danceenergy_corr]], columns = ['danceability_%'], index = ['energy_%']) 
corr2 = pd.DataFrame([[valenceacoustic_corr]], columns = ['valence_%'], index = ['acousticness_%']) 

# Plots the heatmap that represents the correlation of Energy and Danceability 
plt.figure(figsize=(2, 2))
sns.heatmap(corr1, annot=True, linewidth=1, linecolor='black', cmap='Purples')
plt.title('Correlation Between Energy and Danceability')

#Prints Plot for Energy and Danceability
plt.show()

# Plot the heatmap that represents the correlation of valence and acousticness 
plt.figure(figsize=(2, 2))
sns.heatmap(corr2, annot=True, linewidth=1, linecolor='black', cmap='Purples')
plt.title('Correlation between Valence and Acousticness')

#Prints Plot for Valence and Acousticness 
plt.show()
```
### Output: 
![image](https://github.com/user-attachments/assets/8944b689-825e-46d6-b18e-5335e4be7704)

**Note:** Upon examination, it can be concluded that danceability and energy possesses a weak positive correlation, indicating a direct relationship. As for the correlation between valence and acousticness, it possesses a value of -0.062, indicating a very weak negative correlation or weak inverse relationship.
<br> 

## Phase 7: Platform Popularity 
- For this part, we will be analyzing and comparing the number of tracks that appear in spotify_playlists, apple_playlists, and deezer_playlists in order to see which platform is the most favorable. 

**7. a. ) In order to display and see the portion that each platform takes, we will be utilizing a pie chart in order to determine which among the 3 possesses the highest percentage value. Additionally, we will also be adding a dataframe which will take into account the exact number and percentage of each platform.** 
```python
# Filters out the platform columns, applies summation, and stores it in their respective variables 
sum_spotplay = new_df['in_spotify_playlists'].sum() 
sum_deezerplay = new_df['in_deezer_playlists'].sum() 
sum_appleplay = new_df['in_apple_playlists'].sum() 

# Creates Label for each category in the piechart 
Labels = ['Spotify Playlists', 'Deezer Playlists', 'Apple Playlists'] 

# Stores the sum of playlists in each platform 
All_Sum = (sum_spotplay, sum_deezerplay, sum_appleplay) 

#Plots a Piechart that describes the percentage of the amount of playlist in the categories spotify, deezer and apple
plt.pie(All_Sum, labels = Labels, autopct ='%1.0f%%', radius = 1.5) 
plt.title("Percentage of the Total Playlist", fontweight = 'bold', pad = 50)
plt.show()

# Formula for calculating the percentage of each category 
Total_Sum = sum_spotplay + sum_deezerplay + sum_appleplay 
Percent_Spot = (sum_spotplay/Total_Sum)*100 
Percent_Deezer = (sum_deezerplay/Total_Sum)*100 
Percent_Apple = (sum_appleplay/Total_Sum)*100 
Percent_Total = (Total_Sum/Total_Sum)*100 

# Creates a dataframe that will show the categories and its count and percentage, which contributes to the overall pie chart
Playlist_Data = pd.DataFrame({
    'Playlists': ['Spotify Playlists', 'Deezer Playlists', 'Apple Playlists', 'Total Playlists'],
    'Count': [sum_spotplay, sum_deezerplay, sum_appleplay, Total_Sum],
    'Percentage (Exact)': [f"{Percent_Spot:.1f}%", f"{Percent_Deezer:.1f}%", f"{Percent_Apple:.1f}%", f"{Percent_Total:.1f}%"]})

#Prints the dataframe 
Playlist_Data
```
### Output: 
![image](https://github.com/user-attachments/assets/7bc35d13-066a-419b-9a90-9a1538a9e87e)

**Note:** As seen in the pie chart, Spotify Playlists dominate with a percentage of 97.1% of the total playlist, amounting to 3,953,504 counts, while the 1.7% or ~2% goes to the Deezer Playlists with a count of 69,618. Finally, the remaining 1.2% or ~1% goes to Apple Playlist with a count totalling up to 48,719.**
<br> 

## Phase 8: Advanced Analysis
- For this part, we will be applying advanced analysis in order to determine the patterns on the Key per Streams. We will also be examining the artists with the most playlists or charts in order to identify trends according to the artist's genre.

**8. a. ) In order to display the streams by key via barplot, it is essential to utilize the groupby syntax, to group the column stream by key and mode, and sum the streams within the respective group.** 
```python
# Group by both 'key' and 'mode' and sum streams for each group
Total_Key = new_df.groupby(['key', 'mode'])['streams'].sum().reset_index()

# Figure Size of the Plot
plt.figure(figsize=(8, 8))

# Creates a Barplot that represents the data "Total_Key" 
sns.barplot(data=Total_Key, x='key', y='streams', hue = 'mode', palette ='rocket')

# Shows Gridlines
plt.grid()

# Creates a Title and Displays it 
plt.title("Keys of Streams", fontweight = 'bold') 
```
### Output: 
![image](https://github.com/user-attachments/assets/07964695-6e7a-41e2-b8ab-67690c9e72bc)

**Note:** Based on the barplot above, it can be noticed that certain major keys such as C#, D, G#, G, and F, possesses higher streams in comparison to the others keys such as A, A#, F#, B, E, and D. This suggests that these major keys are more likely to be associated with higher popularity among the masses. As for the minor keys, it is observed that C#, B, E, F#, and F are on the higher side in comparison to G, G#, D, D#, A, and A#, indicating that the previously stated keys with higher streams, are more popular among the masses. Additionally, it can also be observed that Major Keys are preferred over the minor keys due to a noticeable difference when comparing streams of the two keys.
<br> 

**8. b. ) After determining the keys of streams, we then proceed with determining the top 10 artists based on Chart Frequency. Here, we will be checking their total chart appearances by using groupby and summation. The following codeblock shows the actual process of creating the visualization.** 
```python
# Splits and expands the column 'artist(s)_name' by ', '
artist_split = new_df['artist(s)_name'].str.split(', ', expand=True)

# Stacks the splitted and expanded column into a singular column 
stacked_artists = artist_split.stack().reset_index(drop=True)

# Create variable "Charts_Data" which stores a copy of the clean dataframe in order to avoid having "SettingwithCopyWarning" Error 
# Ensures that it does not modify the original dataframe 
Charts_Data = new_df[['in_apple_charts', 'in_spotify_charts', 'in_deezer_charts', 'in_shazam_charts']].copy() 

# Adds a column 'Artist' which will be filled with the values of stacked_artists 
Charts_Data['Artists'] = stacked_artists 

# Group data by artists and sum the total chart appearance 
Artist_Charts = Charts_Data.groupby('Artists')[['in_apple_charts', 'in_spotify_charts', 'in_deezer_charts', 'in_shazam_charts']].sum() 

# Reset index to make it sequential again 
Artist_Charts = Artist_Charts.reset_index()

# Adds column 'Total Chart' which contains the summation of the artists' charts
Artist_Charts['Total Charts'] = Artist_Charts[['in_apple_charts', 'in_spotify_charts', 'in_deezer_charts', 'in_shazam_charts']].sum(axis=1)

# Sort the artists from highest amount to lowest amount 
# Resets index and removes old index 
Sorted_ArtistCharts = Artist_Charts.sort_values(by='Total Charts', ascending=False).reset_index(drop=True)

# Grabs only the top 10 Artists from the sorted chart 
Top10ArtistChart = Sorted_ArtistCharts.head(10)

# Plot the top 10 artists by their total chart appearances
# Set figuresize to 16 X 9 
plt.figure(figsize=(16, 9)) 
plt.bar(Top10ArtistChart['Artists'], Top10ArtistChart['Total Charts'], color='indigo')
plt.xlabel("Top 10 Artists", labelpad = 15)
plt.ylabel("Total Chart")
plt.title("Top 10 Artists based on Chart Frequency", fontweight = 'bold')
plt.grid()

#Prints the Plot 
plt.show()
```

### Output: 
![image](https://github.com/user-attachments/assets/5c365bdf-764c-4dcf-a2bb-1998caac3f20)


**Note:** According to the chart, (1) Taylor Swift was shown to be the most frequent artist, followed by (2) The Weeknd, (3) Peso Pluma, (4) Bad Bunny, (5) Bizarrap, (6) SZA, (7) Quevedo, (8) Myke Towers, (9) Feid, and finally (10) Eslabon Armado 
<br>

**8. c . ) Let's now check the top 10 Artists by Total Playlist. In here, there's no longer a need to redo the syntax of splitting and stacking since it was already created in the previous codeblock. This ensures consistency, conciseness and avoids redundancy.** 
```python
# Creates a copy of the clean dataframe to avoid modifying the original one (To ensure that no SettingwithCopyWarning takes place) 
Playlist_Data = new_df[['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].copy()

# Reuse "stacked_artists" based on the previous codeblock 
Playlist_Data['Artists'] = stacked_artists 

# Group by artists and sum the total playlist appearances across all platforms
Artist_Playlists = Playlist_Data.groupby('Artists')[['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].sum()

# Reset the index to make it sequential again
Artist_Playlists = Artist_Playlists.reset_index()

# Adds a column 'Total Playlists' which contains the summation of the artists' playlists
Artist_Playlists['Total Playlists'] = Artist_Playlists[['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].sum(axis=1)

# Sort the artists by their total playlist count in descending order
# Resets index and removes old index 
Sorted_ArtistPlaylists = Artist_Playlists.sort_values(by='Total Playlists', ascending=False).reset_index(drop=True)

# Grab only the top 10 artists from the sorted playlist data
Top10ArtistPlaylists = Sorted_ArtistPlaylists.head(10)

# Plot the top 10 artists by their total playlist appearances
# Figuresize is 16 X 9 
plt.figure(figsize=(16, 9))
plt.bar(Top10ArtistPlaylists['Artists'], Top10ArtistPlaylists['Total Playlists'], color='Indigo')
plt.xlabel("Top 10 Artists", labelpad = 15)
plt.ylabel("Total Playlists Count")
plt.title("Top 10 Artists by Total Playlist", fontweight = 'bold')
plt.grid()

#Prints the Plot
plt.show()
```
### Output: 
![image](https://github.com/user-attachments/assets/adb1e691-dee0-4711-a755-e1785f19b46b)

**Note:** In here, we notice that one of the top 10 Artists seems to be in garbage values. However, in reality, this is an encoding error that took place from the excel itself. The artist name should be "Tiësto" but was written as "Tiï¿½ï¿½sto" in the excel/datafile itself. This is not a data error but a formatting issue. Other than that, the data itself should be consistent. Thus, the top 10 artist by total playlist should be (1) The Weeknd, (2) Taylor Swift, (3) SZA, (4) Bad Bunny, (5) Grupo Frontera, (6) Eminem, (7) Tiësto, (8) Travis Scott, (9) Metro Boomin, and (10) Junior
<br> 


## Summary
#### Phase 1 
- 
#### Phase 2 
#### Phase 3 
#### Phase 4 
#### Phase 5 
#### Phase 6 
#### Phase 7 
#### Phase 8 

## Author(s) 
  ▸ Darren Gaerlan 

## Revision Timeline 
### Version 0.1 - 10/30/2024 
  ▸ Creation of Repository 
### Version 0.2 - 11/7/2024 
  ▸ Updated Repository by Adding a Brief Outline and Goal 
### Version 0.3 - 11/8/2024
  ▸ Updated Repository by Adding 

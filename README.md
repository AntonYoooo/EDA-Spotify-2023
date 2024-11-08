# EDA-Spotify-2023
This repository includes the documentation and analytical application revolving around the dataset of the most streamed Spotify Songs in 2023. 
## Brief Outline of the DataSet
The Dataset "Most Streamed Spotify Songs 2023" is a csv file that contains the list of the most famous songs found on the application "Spotify". It contains different data including track names, artists involved, release date, amount of streams, apple music presence, deezer presence, shazam charts, and other musical attributes such as danceability, bpm, etc. 
## Goal 
Perform Data Analysis to determine the correlation between data or attributes, uncover trends, and examine the relationship between variables, in order to formulate a conclusion as to how a track is likely to become popular. 

## Phase 1: Start + Identifying/Analyzing the initial Dataset
In this part, we simply analyze the initial dataset and check whether there are any missing values or errors that may hinder the coding process. 

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

Note: In the image provided, there are a total of 953 rows and 24 columns. 

**1 . c . ) Now that the dataset is accessed, it will then undergo a process of checking before performing data analysis. First, we investigate the datatypes between the columns through this code:** 
```python 
# Identifies the data type of the columns 
df_DT = df.dtypes
df_DT
``` 
### Output: 
![image](https://github.com/user-attachments/assets/8307e5ef-caa7-41cf-a31b-431571e9135b)

Note: It can be observed that some of the columns in the dataset such as streams, in_deezer_playlists, and in_shazam charts are declared as "object", in which they should be numeric since they're values are numerical. 

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

Note: According to the output, there are 50 blanks in the "in_shazam_charts" column and 95 blanks in the "key" column. Additionally, there are currently 4 tracks that have been duplicated within the dataset. 

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
In this part, we apply Data Cleaning in order to ensure the accuracy and consistency of the dataset. 

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

Note: After cleaning the data, we are then left with 813 rows and 24 columns. 

## Phase 3: Basic Descriptive Statistics 
**3 . a . )**
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

**3 . b . )** 
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
plt.title('Released Year of Tracks') 
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
plt.show() 
```
### Output: 
![image](https://github.com/user-attachments/assets/edc3793c-5ff9-4c15-8432-6afc15c231c1)

**3. c. )** 
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
plt.show() 
```
### Output: 
![image](https://github.com/user-attachments/assets/8d36c24e-72d2-445c-85a6-0a61b9d0a864)

## Phase 4: Top Performers 
**4. a. )** 
```python
#Create variable x to store the top 5 tracks with the largest streams
x = new_df.nlargest(5, 'streams')
#Create Dataframe to present the top 5 most streamed tracks
table_streams = pd.DataFrame({'Track Name': x['track_name'], 'Streams': x['streams']})
table_streams
```
### Output: 
![image](https://github.com/user-attachments/assets/fe6fdc85-e58e-4122-8c80-5111f0c37e8a)

**4. b. )** 
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
table_frequency 
```
### Output: 
![image](https://github.com/user-attachments/assets/3dd75c26-5a82-43d1-983c-7fa4151a2ac4)

## Phase 5: Temporal Trends 
**5. a. )** 
```python
# Use the syntax groupby in order to Group the data by released_year and count the number of tracks
Yearly_Tracks = new_df.groupby('released_year').size().reset_index(name = 'track_count') 

# Creates a barplot using seaborn to represent the data 'Yearly_Tracks' 
sns.barplot(data = Yearly_Tracks, x = 'released_year', y = 'track_count') 
plt.title('Tracks Released Per Year') 
plt.xlabel('Year')
plt.ylabel('Tracks Released')

# Sets the ticks/increment of years to avoid crowding on the x axis 
# Sets the range from 0 (Initial or the Oldest Year released) to the latest with an increment of 5
# Applies Slicing to grab every 5th element within the range (E.g., 1930, 1935, 1940, 1945, 1950 --> 1930, 1950) 
# The Years shown in the barplot indicates the most notable years of releases (5 Years increment is only applied when tracks are actually released within that 5 year, meaning if no tracks are released like the years 1931-1957, it automatically jumps to 1958)
plt.xticks(ticks=range(0, len(Yearly_Tracks), 5), labels=Yearly_Tracks['released_year'][::5])

plt.show()
```
### Output: 
![image](https://github.com/user-attachments/assets/1571ee41-a092-4a1b-91d9-b388bd204b6c)

**5. b. )** 
```python
# Use the syntax groupby in order to Group the data by released_year and count the number of tracks
Monthly_Tracks = new_df.groupby('released_month').size().reset_index(name = 'track_count') 

# Creates a barplot using seaborn to represent the data 'Monthly_Tracks' 
sns.barplot(data = Monthly_Tracks, x = 'released_month', y = 'track_count') 
plt.title('Tracks Released Per Month') 
plt.xlabel('Year')
plt.ylabel('Tracks Released')

plt.show()
```
### Output: 
![image](https://github.com/user-attachments/assets/415346aa-ffa7-49ac-8780-977324a408a4)

## Phase 6: Genre and Music Characteristics 
**6. a. )** 
```python
#
Correlated_Columns = ['streams', 'bpm', 'danceability_%', 
                      'valence_%', 'energy_%', 'acousticness_%', 
                      'instrumentalness_%', 'liveness_%', 'speechiness_%']  

# Filters out columns from new_df based only on the correlated columns 
Filtered_Correlation = new_df[Correlated_Columns] 

# Calculates the correlation of each columns 
Correlation_Stats = Filtered_Correlation.corr() 

# Plots a heatmap with custom settings for a more effective visualization 
plt.figure(figsize= (8, 8))
sns.heatmap(Correlation_Stats, annot = True, linewidth = 1, linecolor = 'black', cmap = 'Purples')
plt.show()

# Creates a variable Influence which will store the correlation statistics on descending order 
influence = Correlation_Stats['streams'].drop('streams').sort_values(ascending=False)

# Creates a Dataframe that will present the correlation statistics 
Correlation_Table = pd.DataFrame({'Attributes': influence.index, 
                        'Influence': influence.values})
print(Correlation_Table)
```
### Output: 
![image](https://github.com/user-attachments/assets/1d9935df-daf3-4fc6-b54a-ae26243ac52e)
![image](https://github.com/user-attachments/assets/3bc8d05b-d02d-4ce7-9d0e-306617f0f3c1)

**6. b. )**
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
plt.show()

# Plot the heatmap that represents the correlation of valence and acousticness 
plt.figure(figsize=(2, 2))
sns.heatmap(corr2, annot=True, linewidth=1, linecolor='black', cmap='Purples')
plt.title('Correlation between Valence and Acousticness')
plt.show()
```
### Output: 
![image](https://github.com/user-attachments/assets/8944b689-825e-46d6-b18e-5335e4be7704)

## Phase 7: Platform Popularity 
**7. a. )** 
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

## Phase 8: Advanced Analysis
**8. a. )** 

## Revision Timeline 
### Version 0.1 - 10/30/2024 
  ▸ Creation of Repository 
### Version 0.2 - 11/7/2024 
  ▸ Updated Repository by Adding a Brief Outline and Goal 
### Version 0.3 - 11/8/2024
  ▸ Updated Repository by Adding 

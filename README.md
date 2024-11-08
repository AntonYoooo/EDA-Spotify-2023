# EDA-Spotify-2023
This repository includes the documentation and analytical application revolving around the dataset of the most streamed Spotify Songs in 2023. 
## Brief Outline of the DataSet
The Dataset "Most Streamed Spotify Songs 2023" is a csv file that contains the list of the most famous songs found on the application "Spotify". It contains different data including track names, artists involved, release date, amount of streams, apple music presence, deezer presence, shazam charts, and other musical attributes such as danceability, bpm, etc. 
## Goal 
Perform Data Analysis to determine the correlation between data or attributes, uncover trends, and examine the relationship between variables, in order to formulate a conclusion as to how a track is likely to become popular. 

## Phase 1: Start + Identifying/Analyzing the initial Dataset
##### 1 . a . ) In order to start, it is essential to first declare and import the necessary libraries in order for the program to work
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
##### 1 . b . ) After declaring the libraries, the dataset is then accessed. Note that in this case, the csv file contains characters that are undefined or unrecognizable by the Panda's default encoding (UTF-8), which is why encoding latin1 is used for this case. 
```python
#the syntax encoding ='latin' ensures that no form of "UnicodeDecodeError" occurs when loading a file that potentially contains characters invalid in UTF-8 (Pandas' Default encoding)
df = pd.read_csv('spotify-2023.csv', encoding='latin') 
df
```

### Output: 
![image](https://github.com/user-attachments/assets/2f0666ef-9b40-436d-acb7-7288bc7a71f5)

Note: In the image provided, there are a total of 953 rows and 24 columns. 

##### 1 . c . ) Now that the dataset is accessed, it will then undergo a process of checking before performing data analysis. First, we investigate the datatypes between the columns through this code: 
```python 
# Identifies the data type of the columns 
df_DT = df.dtypes
df_DT
``` 
### Output: 
![image](https://github.com/user-attachments/assets/8307e5ef-caa7-41cf-a31b-431571e9135b)

Note: It can be observed that some of the columns in the dataset such as streams, in_deezer_playlists, and in_shazam charts are declared as "object", in which they should be numeric since they're values are numerical. 

##### 1 . d . )  After checking out the data types, we then now proceed in checking whether incomplete elements or duplicates are present within the dataset. 
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

##### 1 . e . ) To further prove that there are duplicates within the track, the following code is then implemented: 
```python
duplicate_rows = df[df.duplicated(['track_name', 'artist(s)_name'], keep=False)]

# Display the duplicate rows
duplicate_rows
```
### Output: 
![image](https://github.com/user-attachments/assets/aaa14d47-e2f1-488c-9720-6398d662a20b)

## Phase 2: Data Cleaning 


## Revision Timeline 
### Version 0.1 - 10/30/2024 
  ▸ Creation of Repository 
### Version 0.2 - 11/7/2024 
  ▸ Updated Repository by Adding a Brief Outline and Goal 
### Version 0.3 - 11/8/2024
  ▸ Updated Repository by Adding 

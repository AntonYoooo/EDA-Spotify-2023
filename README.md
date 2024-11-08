# EDA-Spotify-2023
This repository includes the documentation and analytical application revolving around the dataset of the most streamed Spotify Songs in 2023. 
## Brief Outline of the DataSet
The Dataset "Most Streamed Spotify Songs 2023" is a csv file that contains the list of the most famous songs found on the application "Spotify". It contains different data including track names, artists involved, release date, amount of streams, apple music presence, deezer presence, shazam charts, and other musical attributes such as danceability, bpm, etc. 
## Goal 
Perform Data Analysis to determine the correlation between data or attributes, uncover trends, and examine the relationship between variables, in order to formulate a conclusion as to how a track is likely to become popular. 

### Phase 1: Start 
##### 1.a. ) In order to start, it is essential to first declare and import the necessary libraries in order for the program to work
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
##### 1.b. ) After declaring the libraries, the dataset is then accessed. Note that in this case, the csv file contains characters that are undefined or unrecognizable by the Panda's default encoding (UTF-8), which is why encoding latin1 is used for this case. 
```python
#the syntax encoding ='latin' ensures that no form of "UnicodeDecodeError" occurs when loading a file that potentially contains characters invalid in UTF-8 (Pandas' Default encoding)
df = pd.read_csv('spotify-2023.csv', encoding='latin') 
df
```

### Output: 
![image](https://github.com/user-attachments/assets/2f0666ef-9b40-436d-acb7-7288bc7a71f5)



## Revision Timeline 
### Version 0.1 - 10/30/2024 
  ▸ Creation of Repository 
### Version 0.2 - 11/7/2024 
  ▸ Updated Repository by Adding a Brief Outline and Goal 

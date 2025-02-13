# GC-Final-Project

## Setup Instructions
There should be minimal setup beyond needing a local host of postgresSQL with a 'safecars' database already made. The repository includes a 'credentials-template' that you should duplicate, fill out with your credentials, and rename to 'credentials'. All other data is now included with the reposity in the data folder, with all json files being made from the NHTSA api. Should you wish to refresh these, delete all `.json` files and it will rebuild them using the api. The `safercars_data.csv` should not be deleted and must be redownloaded from the NHTSA website for updating.

## Code Documentation
### Extraction
The data sources are in two forms, the NHTSA FARS api, and the NHTSA safercars_data.csv available from their website. These data sources include data on fatal crashes in the US and vehicle safety ratings respectively.
Within the code, queries are made to start the process initially by pulling the list of makes within the FARS api. Using this list, we can use the resulting makeID to query for each make's models. Upon reaching make and models, we can use it to query for that make and model's bodytype. While not used in calculations, it is helpful for human reading of what type of car is being listed.
Now that we have a dataframe of all possible vehicles, we then query for all crash data within the api's usable ten year range of 2010-2020. While the api states its data goes from 2010-onward, the later years had issues of being out of range occasionally, so we took the most stable years. Using each years crash data, we pull out the fatalities involved in each accident and add them to the related car's year total. 

### Transformation
Having extraced a dataframe of all possible vehicles and their related fatalities numbers, all that is left is to clean and merge with the safety ratings csv. Cleaning involved dropping any car with zero crashes, under a minimum threshold of crashes, duplicates, null values, and limiting our final dataframe to the 11 most popular makes to avoid excessive skewing. These remaining cars then have their associated safety ratings attached, and another round of duplicate/null dropping follows.

### Load
With this completed dataframe, we have all the we set out to obtain. Using sql-alchemy, it is then pushed into a postgres SQL database that we hosted locally. This is used to bring the data into PowerBI.

### Analysis
Within the notebook is also some Exploratory Data Analysis, where various data items are plotted and mapped to find patterns and correlations.

## Presentation Slide Deck:
https://docs.google.com/presentation/d/1FCz0XQruXElWC9H1_janz6jSncX3aw8s7TIySwCzaT8/edit?usp=sharing

This readme will be further filled out before presentation.

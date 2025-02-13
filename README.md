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

## Presentation Outline
### Summary
Our intended audience for this would prospective car buyers, car manufactueres, car insurance agencies, and safety agencies. Any party with a vested interest in finding common factors between safety ratings and vehicle crash fatalities. The quested we asked is how much weight do these tests have on the chances of a crash being fatal? While many factors contribute the crash's chance of fatality, this one could be overlooked or oversimplified. Would it be more impactful than most might think, or does the nature of nearly all cars on the road already having the max of 5 stars in the safety ratings muddle the true weight of this variable? The latter is certainly true, having the majority of all cars being top rated means that all crashes are from high-rated cars. Although the data we used also has the star ratings for each part of the car, and a higher rating for 'rollover safety' led to drastically fewer fatalities.
Our key points will the unbalanced weight of the safety ratings being an integer 0-5, with the lowest seen in real data being 4. That even with this, the more specific ratings can help break up that imbalance to show the importance of rollover ratings. Concluding that just fatalities and overall ratings are not enough to make predictions.
The first visualization will be used to show a lack of meaningful correlation between fatalities and overall stars.
The next two shows the fatalities by model, revealing the most fatal models being the most popular, and even after trimming them, still are a matter of road saturation.
The next switches to fatalities by make, showing that overall brands are more even than individual models.
Following is the visualization to show the difference in fatalities by star ratings, for overall stars and all the other categories. This demonstrates the rollover impact.
Lastly, a graph to show the increase in fatalities to stress the importance to try and analyze and predict to solve this problem.

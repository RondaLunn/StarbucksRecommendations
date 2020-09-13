# StarbucksRecommendations
Jupyter notebook with methods to determine effective offers to recommend to Starbucks app users. 

## Project
As part of the Udacity Data Science Nanodegree, I created a method of recommending offer types for users of the Starbucks app. A sample dataset was provided with information regarding users, offer types, and interactions between users and the app during a test of various offers sent to app users. You can see the code and datasets for this project here. Libraries used include pandas, numpy, math, json, and matplotlib.

## Files
Starbucks_Capstone_notebook.ipynb - jupyter notebook containing the code for this project.
portfolio.json - The data for offer information.
profile.json - Teh data for user information.
transcript.json - The data for the events occuring during the test period. 
README.md - This file

## Data
portfolio - 10 offers, 6 columns (channels, difficulty, duration, id, offer_type, reward)
profile -17,000 users, 5 columns (age, became_member_on, gender, id, income)
transcript - 306,534 events, 4 columns (event, person, time, value)

## Cleaning 
### Event Distribution
This creates a dataframe which shows the counts of each event type for each user. There are 17000 total users in this dataframe.
* Sort the transcript dataframe by person.
* Get the unique event types.
* Create new dataframe with each unique person value as the index.
* For each event type: 
    * Create a dataframe of events of that event type and set person as index.
    * Set the event_name as the column name.
    * Replace the event_name value with 1.
    * Create a new dataframe with the count of the event type for each person.
    * Join the dataframe to the event_count_df.
* Fill nulls with 0.
* Return the new dataframe.

### Event Details
Details for each event occurring during the test period. 
* From the transcript dataframe, sort values by person and time and reset the index.
* Break out the value column in transcript to retrieve offer id, transaction amount, and reward amounts.
* Concatenate event value details and drop original values column.
* Fill nulls with 0
* Merge 'offer id' and 'offer_id' columns.
    * Fill 'offer id' nulls with values from 'offer_id'
    * Drop 'offer_id column
* Join portfolio dataframe to event_detail_df on 'offer id' to add offer details.

### Event Counts
create_events_df: 
A dataframe was created which shows the counts of each event type for each user. There are 17000 total users in this dataframe.
* Sort the transcript dataframe by person.
* Get the unique event types.
* Create new dataframe with each unique person value as the index.
* For each event type: 
    * Create a dataframe of events of that event type and set person as index.
    * Set the event_name as the column name.
    * Replace the event_name value with 1.
    * Create a new dataframe with the count of the event type for each person.
    * Join the dataframe to the event_count_df.
* Fill nulls with 0.
* Return the new dataframe.

Counts for each event type for each user. 
* Sum total amount spent per person (profit) and total reward received per person (loss)
* Set profit column to sum of amount column
* Set loss to sum of reward column

### User-Offer Matrix
Shows counts of each offer type by user and event type
* Group event_detail_df by person, event, and offer_type, get the count of offer_type, and unstack with null values set to 0.

## Make Recommendations
find_similar_users - Takes in a user and finds other users with similar demographic information.
* Get the user from the profile dataset
* Get the user's age, gender, and income
* Filter the dataframe by user's gender and specified age and income ranges. 
* Return ids of the similar users. 

create_user_group - Takes in demographic information and outputs a list of users meeting those demographic specifications.
* Filter the dataframe by the specified age range, gender(s), and income range. 
* Return array of ids for users meeting the specifications.

recommend_group_offers - Sorts the offer types by how well a given group responds to the offer.
* For each user in the group, get the information from the user_matrix and the user_count dataframes.
* Get completed offers and viewed offers for the user for each offer type. Set to 0 if none exist. 
* Calculate weighted value of each offer type by 1 minus the absolute value of the difference between completed offers and viewed offers divided by the received offers. 
* Add each weight to the total weight for the offer type. 
* Sort the offer types by weight
* Return the sorted array and a dictionary of the offers and their weights. 

## Evaluate Results
check_recommendation_accuracy - Checks the accuracy of the recommendations by checking the group prediction against each user.
Calculate the root mean squared error to check accuracy of recommendations
* Create a dictionary to map each offer type to a number.
* Create a prediction array mapping each offer type in the recommendation array to the dictionary. 
* Find squared error for each user and append it to errors array.
* Find the mean of the errors array. 
* Return the square root of the mean squared error. 

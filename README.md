# Data_135_yelp_rec

  Yelp has provided a dataset that includes data on its users and businesses in eight different
metropolitan areas. For the purposes of our project,we have created a filterable recommendation
system for 26,000 Yelp users located in Oregon. The end function for our program takes in the
unique user ID, the user’s preference for price, rating, and a boolean of whether the user wants a
restaurant or not, outputting a business recommendation that both fulfills the criteria and is tailored
towards the user’s past likings.

  The problem we are trying to solve is to create a more efficient recommendation system
compared to the non-tailored recommendations that Yelp currently provides. Much of the business
recommendations on the current platform consist of businesses that pay Yelp to be promoted as
well as top-performing businesses that are not catered towards the user’s preferences. For this
reason, we believe our system is more efficient providing recommendations that encompass the
whole range of businesses within the user’s given area as well as businesses that our system expects
the user to like based upon their past reviews.

  Our recommendation system is built using Item-Based Collaborative Filtering. There are
multiple files within the github, but our main file “Item Based CF” contains the most up to date
code for our program. For this reason, we will walk you through how the code in this file works.

  We begin by importing a couple of tools for future data manipulation as well as tools for
reading the json files into dataframes. Since we will be providing users the option to filter their
recommendations by price point as well as whether they only want a restaurant to be recommended,
we have created columns indicating whether a business contains such attributes.

  In Item Based Collaborative Filtering, our first steps are figuring out which items are most
similar to one another. In this case, the businesses constitute what we define as items. The way we
will measure this similarity is by calculating the Cosine Similarity Score between each of the
businesses through their normalized rating. That is, if there exists a correlation between how users
rate two different businesses, these businesses will be assigned a higher similarity score. First
however, we must normalize all the ratings of the users. This is because some users tend to be more
optimistic or pessimistic than others, so to achieve the most accurate similarity score we must
subtract the mean score of a user from all of their business reviews.

  Next we create a n x n matrix (where n is defined as the number of different businesses)
calculating the similarity between each business. In order to speed up the runtime we have made a
number of different adjustments in this step including storing columns into arrays, storing our values
in a matrix rather than a data table, and importing code that calculates the cosine similarity score
using the library numba, jit. This has significantly sped up our runtime compared to before, but even
still this cell took close to an hour and half to run on my machine (if you would like to run the code
on your own machine but don’t want to wait that long, you can consider only creating
recommendations for users with 100 or more reviews. More information on how to do this is within
the code).

  Once we have calculated our similarity matrix, we next want to create predictions for how a
user might rate the different businesses. The ‘user_predictions’ function takes in the user_id and the
number of neighbors to use for its prediction, outputting an array of predicted scores. It does so by
filtering the possible businesses that are similar to the business of interest then creating a weighted
prediction for each business sequentially.

  Our next function ‘recommend’ takes in the user_id and the business we will be
recommending to this specific user, creating a data table that consists of information about the user
and the business of interest. This function is mostly used in order to fetch data about the
recommended business for the user.

  Lastly our ‘restaurant_recommender’ function puts everything together. This function takes
in the user_id as well as their filter preferences (price, rating, restaurant) and after filtering the data to
only include businesses that fits the user’s preferences, calls the ‘user_predictions’ function to create
an array of business scores. Using this array, we take the highest score within that array and
recommend the corresponding business. We then call the ‘recommend’ function to output data
concerning the user and the recommended business.

<h1><b> Project Report - Group 3 ETL Project </b></h1>
 
 
<h3> Extract </h3>

When brainstorming where our group wanted to source datasets for our project, we identified Kaggle as the best option to explore. After exploring what resources Kaggle had to offer and briefly discussing what we wanted to do, we opted to look into what type of datasets were available on movies. 

When scrolling through Kaggle's public datasets we found two CSV files:

MoviesOnStreamingPlatforms_updated.csv
IMDB_movies.csv 

‘MoviesOnStreamingPlatforms_updated’ is described on Kaggle as “an amalgamation of data that was scraped from Reelgood.com.” The file is essentially a comprehensive list of movies available on various streaming platforms. Meanwhile, ‘IMDb_movies’ is described on Kaggle by its publisher as “81k+ movies and 175k+ cast members scraped from IMDb.”

Both files matched the criteria we were looking for while seemingly also being compatible for a merge based on movie title. 

<h3> Transform</h3>

In respect to data cleaning, we merged our two csv files: MoviesOnStreamingPlatforms_updated.csv, and IMDB_movies.csv. 

These datasets were merged by using the pd.merge function which allows for a merge on two Pandas dataframes in Jupyter Notebook:

Loaded CSV files into Jupyter Notebook :

IMDB_route = "Resources/movies.csv"
stream_route = "Resources/Stream.csv"

Stored them as variables under the names of imdb_db & stream_db  by use of pd.read_csv:  

imdb_db = pd.read_csv(IMDB_route)
stream_db = pd.read_csv(stream_route)

Completed the Merge 

merged_df = stream_df.merge(imdb_df, left_on='Title', right_on='Title')

In order for a successful merge into one dataframe, a transform by use of the .rename function was needed. A merge would not prove successful until the title characters matched character for character. Therefore, we renamed the ‘Title’ column in imdb_db to ‘title’ by use of the code below. 

imdb_df = imdb_df.rename(columns={"title": "Title"})

The dissimilarity in lower-case/upper-case titles in regard to the newly merged dataset would prove to be a recurring issue. This time in the form of a querying issue. To fix this issue, we simply renamed all of our columns to lowercase versions of themselves. 

merged_df = merged_df.rename(columns={'Title': 'title', 'Age':'age', 'IMDb':'IMDb', 'Rotten Tomatoes': 'rotten tomatoes',  'Runtime': 'runtime', 'Netflix': 'netflix', 'Hulu': 'hulu', 'Prime Video': 'prime', 'Disney+': 'disney'})

Lastly, the title of ‘disney +’ led to several issues throughout our transformations. We simply renamed the column ‘disney’ to serve as the title of the streaming service throughout our dataset in order to navigate this issue.

Because we had quite a bit of extra time, we decided to break some of our original merged file into a number of different tables, as seen in our Jupyter Notebook. 

<h3> Load </h3>
 
Finally, these databases were imported into pgAdmin using SQLAlchemy. The user will need to create database in pgAdmin titled postgres for the file to run smoothly, though they can easily change the database name in our rds_connection_string variable. The user will then need to input their username and password from pgAdmin. With this done, the files should easily import into pgAdmin. Several queries have been provided for the user, but they are more than welcome to search for their favorite movie to find it on their favorite streaming platform. 

The reason we chose to use the relational database PostgreSQL was it was the database we were all most familiar. 
 

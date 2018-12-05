# ETL_PROJECT



Assignment : ETL Project for Movie Ratings

Craig Michael Taflin, Monisha Jain, Hiral Patel.

October 27, 2018 

Abstract: 
Extraction–transformation–loading (ETL) tools are pieces of software responsible for the extraction of data from several sources, its cleansing, customization, reformatting, integration, and insertion into a data warehouse. Building a data warehouse requires focusing closely on understanding three main areas: the source area, the destination area, and the mapping area (ETL processes). In this paper we will try to navigate through the efforts done to conceptualize the ETL processes. These projects try to represent the conceptual design of ETL processes. For this assignment we were given 3 sessions of from our course schedule to put together as much techniques of ETL, every effort  has been made to specify the details; however, there is room to be creative which we take in to consideration in the future project. 

Motivation:
The emerging technology of ETL process was weaved in to our course of Data Science as a brief, due to the easy to understand and applicable concept of the same we were encouraged to create a small project based ETL and understand the End-to-End flow of the process. We are thrilled to put together the knowledge of the same and work on this project.

The Assignment 
So the assignment is to build an application which Extracts the data from a database, apply some cleaning strategies which a transformation of original data in to our own requirement and “Load” in context to our project is to display the data as per our requirement. We downloaded Movies Dataset, which includes various data in form of .csv files, from kaggle.com
Python, sqlalchemy and SQLite are the tools we used to integrate our project as a whole.
The files which we used for ETL process are:
credits. csv
keywords. csv
links. csv
links_small. csv
links_small. csv
movies_metadata. csv
ratings. csv
ratings_small. csv

Once we  create the ETL implementation, the next step is to demonstrate that it works. The final code will display the number of users with their ID, shows the the movie reviewed by them also user gets to know the ratings each movie received from other viewers.
Based on the ratings, user can decide to pick the next movie to invest the time 	for. Also, our database provides a user to look for any specific movie in the database. Even with minimum input, our final database is capable of displaying all the information for a movie for any user.

1.Extraction
The first step in any ETL scenario is data extraction. The ETL extraction step is responsible for extracting data from the source systems. Each data source has its distinct set of characteristics that need to be managed in order to effectively extract data for the ETL process. 
During extracting data from different data sources, our team was well aware of connection to database sources: SQLite understanding the data structure of sources, and (c) know how to handle the sources. In the initial extraction, it is the first time to get the data from the different operational sources to be loaded into the data warehouse.

#Import first data source- LINKS SMALL
links_file = "../ETL_Project_original_Data/links_small.csv"
links_df = pd.read_csv(links_file)

#links_df
#links_df.dtypes
#links_df.columns
#Import second data source- RATINGS 
ratings_file = "../ETL_Project_original_Data/ratings_small.csv"
ratings_df = pd.read_csv(ratings_file)
#ratings_df.dtypes

#Import 3rd data source- MOVIE METADATA 
movie_meta_file = "../ETL_Project_original_Data/movies_metadata.csv"
movie_meta_df = pd.read_csv(movie_meta_file)

2. Transformation
The second step in any ETL scenario is data transformation. The transformation step tends to make some cleaning and conforming on the incoming data to gain accurate data which is correct, complete, consistent, and unambiguous. This process includes data cleaning, transformation, and integration. As a cleaning process we removed some unnecessary columns from a table


movie_meta_df['imdbId']=pd.Series(movie_meta_df['imdb_id'].str[3: ],index=movie_meta_df.index, dtype=None)

#Reduce columns to valuable to task
movie_meta_df_revised=movie_meta_df[["title","genres","vote_count","vote_average","imdbId","overview",\
                                            "release_date","budget","revenue", "id","adult","spoken_languages",
                                            "original_language"]]

#movie_meta_df_revised.dtypes
movie_meta_df_revised.head()

links_ratings_merge = pd.merge(links_df,ratings_df,on ="movieId")
movie_data = links_ratings_merge.drop('timestamp',1)
movie_data


3. Loading
Loading data to the target multidimensional structure is the final ETL step. In this step, extracted and transformed data is written into the dimensional structures actually accessed by the end users and application systems. Loading step includes both loading dimension tables and loading fact tables.

#sqlalchemy when working with Classes
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
Base = declarative_base()
from sqlalchemy import Column, Integer, String, Float

Create Engine and Pass in MySQL Connection
PyMySQL 
import pymysql
pymysql.install_as_MySQLdb()

engine=create_engine("mysql:Address ")
conn= engine.connect ()

#Base Class (movie_meta)
Create Class
class movie_meta (Base):
    	__tablename__ = ' movie_meta'
    	 imdbID= Column(Integer, primary_key=True)
    	genres= Column(String(255))
    	Vote_count= Column(Integer)
    	Vote_average= Column(Integer)
	overview= Column(String(255))
	release_date = Column(String(255))
	budget= Column(Integer)
	revenue= Column(Integer)
#Base Class(
	Send Data Frame to Pre-set up SQL table
movie_meta_df_revised.to_SQL(movie_meta, conn)
	second_df.to_SQL(SQL Table, conn)




* Project owner: Group 1 DSF_PT08P2
* Student Names
 1. Gilbert Cheruiyot - Group Leader
 2. Mercy Ayub
 3. James Muthee
 4. Edwin George
 5. Reagan Adajo
 6. Angela Mwanzia
 7. Millicent Cheptoi
 8. Daniel Muigai
    
* Student Pace: part time
* Instructor name: Samuel Karu & Daniel Ekale


## MOVIE STUDIO DATA ANALYSIS,INSIGHTS AND RECOMMENDATIONS

# Project Overview

This project will analyze film trends, including popular genres and budget strategies, to provide actionable insights for studio leadership. The ultimate goal is to guide strategic decisions and ensure effective resource allocation, positioning the studio for competitive success in the market.


# Business Problem

Our company recognizes the growing trend among major industry players in producing original video content and intends to establish a new movie studio to capitalize on this opportunity. However, we currently lack the necessary expertise and insights into filmmaking. To effectively guide the studio's development, we have been assigned the task of investigating the types of films that are currently excelling at the box office. The challenge lies in not only identifying these successful film trends but also converting this information into practical recommendations that the studio's leadership can use to make informed decisions about future film productions. Failure to do so may result in misguided investments and an inability to compete in the market.

# Business Objectives

  * What are the trends in movie release dates and what impact do they have on revenue?
  * Is there a relationship between production budget and profitability and ROI of a movie? i.e Does a higher production budget automatically result to  a higher profitability and vice versa.
  * Determine which film genres are currently performing best at the box office.
  * Look at trends in audience preferences and box office performance over recent years.
  * Understand Audience Demographics by identifying the demographics of audiences for top-performing genres.
  * Evaluate production costs associated with different genres.
  * Understand the relationship between production cost and revenue streams of movies
  * Evaluate the if movie rating affect production costs.
  * Provide actionable insights for the new movie studio.
  
# Sources of Data 

In the folder `zippedData` are movie datasets from:

  * [Box Office Mojo](https://www.boxofficemojo.com/)
  * [IMDB](https://www.imdb.com/)
  * [Rotten Tomatoes](https://www.rottentomatoes.com/)
  * [TheMovieDB](https://www.themoviedb.org/)
  * [The Numbers](https://www.the-numbers.com/)

Because it was collected from various locations, the different files have different formats. Some are compressed CSV (comma-separated values) or TSV (tab-separated values) files that can be opened using spreadsheet software or `pd.read_csv`, while the data from IMDB is located in a SQLite database.

![movie data erd](https://raw.githubusercontent.com/learn-co-curriculum/dsc-phase-2-project-v3/main/movie_data_erd.jpeg)

Note that the above diagram shows ONLY the IMDB data. You will need to look carefully at the features to figure out how the IMDB data relates to the other provided data files.

# Data
The data used for this project include:
  * [movie_gross]( )
  * [movie_budget]( )
  * [movie_info]( )
  * [im.db]( )
  

# Data Understanding

# The Libraries 
The files are built-in and can only be accessed once they are imported into the library.
The libraries used in the project are listed below:

* import pandas as pd
* import numpy as np
* import sqlite3
* import seaborn as sns
* import matplotlib.pyplot as plt
* import matplotlib_inline
* import matplotlib.image as mpimg
* import statsmodels.api as sm
* import zipfile
* import os
* from scipy import stats
* from scipy.stats import pearsonr, ttest_ind
* from statsmodels.stats.diagnostic import linear_rainbow
* sns.set_style()

# Loading of the data
The data was loaded in two ways: from CSV files and from a database. The CSV files were located in the unzipped directory, where ./Data/dsc-phase-2-project-v3-main.zip was extracted. Within this directory, the zippedData folder contained various datasets. Additionally, data was loaded directly from the database.

   * movie_budget = pd.read_csv("unzipped/dsc-phase-2-project-v3-main/zippedData/tn.movie_budgets.csv.gz")
   * movie_gross = pd.read_csv("unzipped/dsc-phase-2-project-v3-main/zippedData/bom.movie_gross.csv.gz")
   * movie_info = pd.read_csv("unzipped/dsc-phase-2-project-v3-main/zippedData/rt.movie_info.tsv.gz", sep='\t', encoding='ISO-8859-1')

* #create a connection to the database
  conn = sqlite3.connect("im.db")

  #create a cursor
  cur = conn.cursor()

  #Check the table names for our database
  pd.read_sql("""SELECT name FROM sqlite_master WHERE type='table';""", conn)


# Description of data
* Movie_info  
   * All the ID column details are 1560 with none missing, datatypes are integer(1) and object(11)
   * The number of rows and columns are 1560 , 12 in number. 
   * The mean of the unique data in movie_info was 1007.30.
   * The standard deviation of the ID column in movie_info was 579.16. This shows how data deviates from the mean.
   * The minimum value of ID is 1 with the maximum being 2000
  
* Movie_budget
   * The data has 5782 rows with none with missing values.
   * The data has 5 columns with text type of data while the ID column has integer data type.
   * ID column in Movie_budget has 5782 unique values counted.
   * The mean of the unique data in movie_budget was 50.37.
   * The standard deviation of the ID column in movie_budget was 28.82. This shows how data deviates from the mean.
   * The minimum value of ID is 1 with the maximum being 100.
  
* Movie_gross
    * The data has 3387 rows with title column having no missing values. Title is the unique identifier in the dataset.
    * The data has 3 columns(title,studio,foreign_gross) with text type of data, the domestic_gross and year columns have floats and integer data types, respectively.
    * The standard deviation of domestic gross for the movies was about 67 million dollars, showing a large spread
    * The 75th percentile of the domestic gross was 2.79 million dollars with the median at 1.2 million dollars. This shows that the 75% of the movies have a domestic gross less than 2.79 million dollars. 25% of the movies have a domestic gross of less than 1.2 million dollars.
    * The minimum domestic gross value for the movies was 100 dollars , the maximum domestic gross value stood at 936.7 million dollars.


# Cleaning of the data 
* Movie_info
    * Finding the sum of the missing values, calculating its percentage  and dropping columns with missing values above 50%.
    * Studio, box_office and currency columns have the highest number of missing values, all above 50% of the values.
    * The categorical columns the missing values are filled using the mode
    * Splitting of the genre list to refer to  specific genres, dropping the genre column and renaming of the genre list column to genre 
    * Convert of the dates to Date Time Format
  
* Movie_budget
    * The release date being converted to DateTime Format
    * The removal of the dollar sign from the numerical columns
  
* Movie_gross
    * The commas from the 'foreign_gross' column  were removed and the column converted to numeric
    * Convert to foreign_gross to numeric
    * The rows with missing values in 'studio' and 'domestic_gross' columns were removed
    * Replacing the Missing Values in 'foreign_gross' with the median
  
* IMDB Database
    * The extraction of the column names from the movie_basics table 
    * Joining the Movie_basics table and the Movie_ratings table using Movie id which is the primary key.
    * Convert to a dataframe
    * Checking for the missing values, tre runtime and genres column had missing values which were dropped.
    * Removed the columns with duplicate names.

* A merge between the clean Movie_info dataset with Movie_budget dataset

# Visualization(s)

### 1. Relationship between Domestic Gross and Foreign Gross

![Domestic Gross vs Foreign Gross](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Scatter%20Domestic%20and%20Foreign%20Gross.png)

### 2. Scatter Plot with Regression Line of Domestic Gross and Worldwide Gross

![Domestic Gross vs Worldwide Gross](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Scatter%20plot%20regression%20line%20domestic%20gross%20and%20Worldwide%20gross.png)

### 3. Heatmap Showing Correlation of the Numerical variables

![Heatmap of Numerical Variables](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Heatmap%20for%20Runtime%20Production%20bdgt%20Domestic%20Foreign%20Gross.png)

### 4.  Bivariate Analysis of Total Revenue

![Bivariate Analysis of Total Revenue](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Heatmap%20for%20Runtime%20Production%20bdgt%20Domestic%20Foreign%20Gross.png)

### 5. Multivariate Analysis of Production budget and Domestic Gross

![Multivariate Analysis of Production budget and Domestic Gross](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Multivariate%20Analysis%20of%20production%20budget%20and%20domestic%20gross.png)

### 6. Total Revenue and Production Budget by Movie Release Month

![Revenue by Release Month](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Total%20Revenue%20and%20Production%20Budget%20by%20Release%20month.png)

### 7. Analysis of Production Budget vs Profitability

![Production Budget vs Profitability](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/production%20budget%20vs%20profitability.png)

### 8. Average Movie Domestic Revenue by Genre

![Domestic Revenue by Genre](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Average%20Domestic%20Gross%20Revenue%20by%20Genre.png)

### 9. Analysis of Movie Rating vs Domestic Gross

![Movie Rating vs Domestic Gross](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Movie%20rating%20vs%20Domestic%20Gross.png)

### 10. Analysis of Movie Rating vs Worldwide Gross

![Movie Rating vs Worldwide Gross](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Movie%20Rating%20vs%20Worldwide%20Gross.png)

### 11. Analysis of Movie Runtime and Movie Ratings

![Movie Runtime vs Movie Ratings](https://github.com/gkipkirui1/DSF_PT08P2_Phase_2_Project_Grp_1_Final/blob/main/Image%20Visualizations/Runtime%20vs%20Rating.png)



# Conclusions

# 1. Insights from Correlation Analysis
         
* Correlation between the foreign gross and domestic gross had a strong positive. This implies that an increase in domestic gross could also be reflected with an increase in foreign gross.


* Domestic gross and the year have a weak correlation. Equally foreign gross and the year have a weak relationship. For predictive modelling, the positive correlation between the domestic gross and foreign gross will be an important relationship to consider. 


* Production budget has a positive correlation with both domestic gross and worldwide gross, with the later being stronger at 0.7392. This relationship implies that production budgets of movies have a relatively strong and positive relationship with both domestic and gross earning.


* Runtime has a weak relationship with production budget, domestic and worldwide gross. The correlation between runtime and production budget was 0.0040. Equally, the correlation between runtime with both domestic and worldwide gross was negative at -0.000906 and -0.003864, depicting a very weak relationship hence the revenue values as a function of runtime cannot be modeled using a linear model.


* Similarly, runtime as a function of movie production budget cannot be accurately modeled using a linear model because the p-value of 4.13e-09 obtained is significantly smaller than the usual significance level of 0.05.


* There is a positive linear relationship between domestic gross and foreign gross  as well as domestic gross and worldwide gross. Hence a linear model is suitable to model domestic gross, foreign gross and worldwide gross revenues for the movies.


* The higher production budgets generally correlate with higher total revenue, most movies fall within a lower budget and revenue range. 

*  There is a moderate positive correlation between production budget and profitability, but a very weak negative correlation between production budget and ROI.

# 2. Insights on Genre Revenue Trends in Domestic and Worldwide Markets

* Musical and Performing Arts consistently outperforms other genres in both domestic and worldwide gross revenue.


* Horror and Science Fiction and Fantasy also show strong performance in both domestic and worldwide markets.


* Classics and Documentary have a higher average domestic gross compared to their worldwide performance.


* Certain genres, such as Drama (orange) and Science Fiction and Fantasy (green), are more likely to achieve higher total revenue and profit, as these points appear more frequently among the higher values

# 3. Insights from Production Budget and Genre Analysis

*  Genres such as Action and Adventure and Science Fiction and Fantasy are associated with both high production budgets


*  In contrast, genres like Documentary and Special Interest tend to have smaller budgets and lower domestic revenue


*  Most movies have relatively low production budgets and domestic grosses, with only a few movies achieving very high values in either category.



# 4. Insights on Movie Ratings

* High-rated movies(with ratings >=8) tend to be slightly shorter on average, but they're actually more likely to be over 2 hours long with a percentage of 9.9% compared to 7.9% for low rated movies (with ratings < 5)


* The standard deviation of High rated movies of 39.04 is significant. The minimum movie runtime was 4 minutes long with the maximum at 1440. The 1440 minutes suggests existence of outliers in the dataset since the average average runtime for high rated movies was 88.62 minutes.


* The standard deviation of low rated movies of 19.04 is significant, implying big variations in the run time. The minimum movie runtime was 4 minutes long with the maximum at 480. The 480 minutes suggests existence of outliers in the dataset. The average runtime for low rated movies was 92.71 minutes.



# Recommendations

1.  We are recommending to the company to prioritize releasing more movies during the months of June, July and December. December is the most profitable month and therefore aiming at this timeframe could maximize on box office revenue.


2.  We recommend to the company to utilize lower budget or experimental films in months like January and September which have lower revenue to maintain a consistent presence.


3.  We also recommend to the company to increase marketing efforts leading up to and during January and September to maximize opening-weekend success and overall revenue.


4.  Release High-Potential Films in November-December as it shows strong performance in revenue. We would recommend to the company to release family-oriented films and holiday-themed movies during this timeframe.

5.  We recommend the company diversify its film portfolio by including more investments in lower-budget projects to enhance overall profitability and mitigate financial risk. Allocating a portion of funds to smaller, strategically selected productions that offer high returns relative to their cost will support this goal.

6.  We recommend the company implement stricter budget controls and optimize resource allocation to focus spending on elements that enhance the film's appeal without unnecessarily inflating costs.


7.  We recommend the company develop tailored marketing campaigns for lower-budget films, utilizing cost-effective promotional strategies to enhance revenue potential.


8.  We recommend to the company to consider producing shorter films for wider audience appeal while also investing in select, high-quality longer films that can captivate audiences and drive engagement.


9.  We recommend allocation of more resources to produce a mix of shorter, high-rating-friendly films and select longer, high-quality projects that will meet the expectations of more targeted audiences.

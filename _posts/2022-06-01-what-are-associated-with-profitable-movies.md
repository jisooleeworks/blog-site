---
layout: post
title: What are associated with profitable movies?
feature-img: "assets/img/2022-06-01-what-are-associated-with-profitable-movies/cinema-seats.jpg"
---

Using a [TMDb movie data set](https://www.kaggle.com/datasets/juzershakir/tmdb-movies-dataset) containing properties of movies released between 1960 and 2015, I explored what are related to financial success of movies, with visualizations. 

In assessing financial success, I used **Return on Investment (ROI)**, a measure commonly used in describing profitability. It is calculated by: ROI = (gross revenue - cost)/cost. Of the properties, I was most interested in **budget, genres, and vote scores**. Specifically, I investigated:
1. Did high budget movies achieve high ROI?
2. Did specific genres lead to high ROI?
3. Did high score movies achieve high ROI?

The work process was:
* [Data overview](#ch1)
* [Data wrangling](#ch2)
* [Univariate exploration](#ch3)
* [Q1: Did high budget movies achieve high ROI?](#ch4)
* [Q2: Did specific genres lead to high ROI?](#ch5)
* [Q3: Did high score movies achieve high ROI?](#ch6)
* [Conclusions (Summary, Limitations)](#ch7)

I will walk through the process here. You can see code and all details [in my GitHub](https://github.com/jisooleeworks/blog-code/blob/main/tmdb_post.ipynb).

# Data overview<a class="anchor" id="ch1"></a>
After loading the data set, I looked at a couple of fundamental aspects. 

```python
df.head()
```

<table border = '1' class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>imdb_id</th>
      <th>popularity</th>
      <th>budget</th>
      <th>revenue</th>
      <th>original_title</th>
      <th>cast</th>
      <th>homepage</th>
      <th>director</th>
      <th>tagline</th>
      <th>keywords</th>
      <th>overview</th>
      <th>runtime</th>
      <th>genres</th>
      <th>production_companies</th>
      <th>release_date</th>
      <th>vote_count</th>
      <th>vote_average</th>
      <th>release_year</th>
      <th>budget_adj</th>
      <th>revenue_adj</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>135397</td>
      <td>tt0369610</td>
      <td>32.985763</td>
      <td>150000000</td>
      <td>1513528810</td>
      <td>Jurassic World</td>
      <td>Chris Pratt|Bryce Dallas Howard|Irrfan Khan|Vi...</td>
      <td>http://www.jurassicworld.com/</td>
      <td>Colin Trevorrow</td>
      <td>The park is open.</td>
      <td>monster|dna|tyrannosaurus rex|velociraptor|island</td>
      <td>Twenty-two years after the events of Jurassic ...</td>
      <td>124</td>
      <td>Action|Adventure|Science Fiction|Thriller</td>
      <td>Universal Studios|Amblin Entertainment|Legenda...</td>
      <td>6/9/15</td>
      <td>5562</td>
      <td>6.5</td>
      <td>2015</td>
      <td>1.379999e+08</td>
      <td>1.392446e+09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>76341</td>
      <td>tt1392190</td>
      <td>28.419936</td>
      <td>150000000</td>
      <td>378436354</td>
      <td>Mad Max: Fury Road</td>
      <td>Tom Hardy|Charlize Theron|Hugh Keays-Byrne|Nic...</td>
      <td>http://www.madmaxmovie.com/</td>
      <td>George Miller</td>
      <td>What a Lovely Day.</td>
      <td>future|chase|post-apocalyptic|dystopia|australia</td>
      <td>An apocalyptic story set in the furthest reach...</td>
      <td>120</td>
      <td>Action|Adventure|Science Fiction|Thriller</td>
      <td>Village Roadshow Pictures|Kennedy Miller Produ...</td>
      <td>5/13/15</td>
      <td>6185</td>
      <td>7.1</td>
      <td>2015</td>
      <td>1.379999e+08</td>
      <td>3.481613e+08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>262500</td>
      <td>tt2908446</td>
      <td>13.112507</td>
      <td>110000000</td>
      <td>295238201</td>
      <td>Insurgent</td>
      <td>Shailene Woodley|Theo James|Kate Winslet|Ansel...</td>
      <td>http://www.thedivergentseries.movie/#insurgent</td>
      <td>Robert Schwentke</td>
      <td>One Choice Can Destroy You</td>
      <td>based on novel|revolution|dystopia|sequel|dyst...</td>
      <td>Beatrice Prior must confront her inner demons ...</td>
      <td>119</td>
      <td>Adventure|Science Fiction|Thriller</td>
      <td>Summit Entertainment|Mandeville Films|Red Wago...</td>
      <td>3/18/15</td>
      <td>2480</td>
      <td>6.3</td>
      <td>2015</td>
      <td>1.012000e+08</td>
      <td>2.716190e+08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>140607</td>
      <td>tt2488496</td>
      <td>11.173104</td>
      <td>200000000</td>
      <td>2068178225</td>
      <td>Star Wars: The Force Awakens</td>
      <td>Harrison Ford|Mark Hamill|Carrie Fisher|Adam D...</td>
      <td>http://www.starwars.com/films/star-wars-episod...</td>
      <td>J.J. Abrams</td>
      <td>Every generation has a story.</td>
      <td>android|spaceship|jedi|space opera|3d</td>
      <td>Thirty years after defeating the Galactic Empi...</td>
      <td>136</td>
      <td>Action|Adventure|Science Fiction|Fantasy</td>
      <td>Lucasfilm|Truenorth Productions|Bad Robot</td>
      <td>12/15/15</td>
      <td>5292</td>
      <td>7.5</td>
      <td>2015</td>
      <td>1.839999e+08</td>
      <td>1.902723e+09</td>
    </tr>
    <tr>
      <th>4</th>
      <td>168259</td>
      <td>tt2820852</td>
      <td>9.335014</td>
      <td>190000000</td>
      <td>1506249360</td>
      <td>Furious 7</td>
      <td>Vin Diesel|Paul Walker|Jason Statham|Michelle ...</td>
      <td>http://www.furious7.com/</td>
      <td>James Wan</td>
      <td>Vengeance Hits Home</td>
      <td>car race|speed|revenge|suspense|car</td>
      <td>Deckard Shaw seeks revenge against Dominic Tor...</td>
      <td>137</td>
      <td>Action|Crime|Thriller</td>
      <td>Universal Pictures|Original Film|Media Rights ...</td>
      <td>4/1/15</td>
      <td>2947</td>
      <td>7.3</td>
      <td>2015</td>
      <td>1.747999e+08</td>
      <td>1.385749e+09</td>
    </tr>
  </tbody>
</table>

```python
df.info()
```

<div class = "notebook_output">
<pre>&lt;class &#39;pandas.core.frame.DataFrame&#39;&gt;
RangeIndex: 10866 entries, 0 to 10865
Data columns (total 21 columns):
 #   Column                Non-Null Count  Dtype  
---  ------                --------------  -----  
 0   id                    10866 non-null  int64  
 1   imdb_id               10856 non-null  object 
 2   popularity            10866 non-null  float64
 3   budget                10866 non-null  int64  
 4   revenue               10866 non-null  int64  
 5   original_title        10866 non-null  object 
 6   cast                  10790 non-null  object 
 7   homepage              2936 non-null   object 
 8   director              10822 non-null  object 
 9   tagline               8042 non-null   object 
 10  keywords              9373 non-null   object 
 11  overview              10862 non-null  object 
 12  runtime               10866 non-null  int64  
 13  genres                10843 non-null  object 
 14  production_companies  9836 non-null   object 
 15  release_date          10866 non-null  object 
 16  vote_count            10866 non-null  int64  
 17  vote_average          10866 non-null  float64
 18  release_year          10866 non-null  int64  
 19  budget_adj            10866 non-null  float64
 20  revenue_adj           10866 non-null  float64
dtypes: float64(4), int64(6), object(11)
memory usage: 1.7+ MB
</pre>
</div>

The data set had 10866 records with 21 columns. Some columns, like ‘cast’ and ‘genres’, contained multiple values separated by pipe characters. With regard to budget and revenue, the data set had both original and inflation adjusted value column. I chose to use the inflation adjusted ones. 

# Data wrangling<a class="anchor" id="ch2"></a>
In this step, I dealt with:
* Duplicates
* Missing values
* Unused columns
* Incorrect values
* Extraction of a variable of interest (i.e., ROI)
* Data type
* Outliers

## Duplicates
First of all, I examined if there were any duplicate records by checking for (1) duplicate id values, and (2) records with the same 'title', 'director', 'release_year', and 'runtime'.

### Based on 'id'

```python
df[df.duplicated(['id'], keep = False)]
```
<table style="border:1;height:280px;" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>imdb_id</th>
      <th>popularity</th>
      <th>budget</th>
      <th>revenue</th>
      <th>original_title</th>
      <th>cast</th>
      <th>homepage</th>
      <th>director</th>
      <th>tagline</th>
      <th>keywords</th>
      <th>overview</th>
      <th>runtime</th>
      <th>genres</th>
      <th>production_companies</th>
      <th>release_date</th>
      <th>vote_count</th>
      <th>vote_average</th>
      <th>release_year</th>
      <th>budget_adj</th>
      <th>revenue_adj</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2089</th>
      <td>42194</td>
      <td>tt0411951</td>
      <td>0.59643</td>
      <td>30000000</td>
      <td>967000</td>
      <td>TEKKEN</td>
      <td>Jon Foo|Kelly Overton|Cary-Hiroyuki Tagawa|Ian...</td>
      <td>NaN</td>
      <td>Dwight H. Little</td>
      <td>Survival is no game</td>
      <td>martial arts|dystopia|based on video game|mart...</td>
      <td>In the year of 2039, after World Wars destroy ...</td>
      <td>92</td>
      <td>Crime|Drama|Action|Thriller|Science Fiction</td>
      <td>Namco|Light Song Films</td>
      <td>3/20/10</td>
      <td>110</td>
      <td>5.0</td>
      <td>2010</td>
      <td>30000000.0</td>
      <td>967000.0</td>
    </tr>
    <tr>
      <th>2090</th>
      <td>42194</td>
      <td>tt0411951</td>
      <td>0.59643</td>
      <td>30000000</td>
      <td>967000</td>
      <td>TEKKEN</td>
      <td>Jon Foo|Kelly Overton|Cary-Hiroyuki Tagawa|Ian...</td>
      <td>NaN</td>
      <td>Dwight H. Little</td>
      <td>Survival is no game</td>
      <td>martial arts|dystopia|based on video game|mart...</td>
      <td>In the year of 2039, after World Wars destroy ...</td>
      <td>92</td>
      <td>Crime|Drama|Action|Thriller|Science Fiction</td>
      <td>Namco|Light Song Films</td>
      <td>3/20/10</td>
      <td>110</td>
      <td>5.0</td>
      <td>2010</td>
      <td>30000000.0</td>
      <td>967000.0</td>
    </tr>
  </tbody>
</table>

Two records were detected, identical for all the variables. So, an extra was deleted. 

### Based on 'title', 'director', 'release_year', and 'runtime'

```python
df[df.duplicated(['original_title', 'director', ], keep = False)]\
    [['original_title', 'director', 'release_year', 'runtime']].sort_values(by=['original_title'])
```

<table style="border:1;height:260px;" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>original_title</th>
      <th>director</th>
      <th>release_year</th>
      <th>runtime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1400</th>
      <td>9</td>
      <td>Shane Acker</td>
      <td>2009</td>
      <td>79</td>
    </tr>
    <tr>
      <th>6514</th>
      <td>9</td>
      <td>Shane Acker</td>
      <td>2005</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4337</th>
      <td>Bottle Rocket</td>
      <td>Wes Anderson</td>
      <td>1994</td>
      <td>13</td>
    </tr>
    <tr>
      <th>8547</th>
      <td>Bottle Rocket</td>
      <td>Wes Anderson</td>
      <td>1996</td>
      <td>91</td>
    </tr>
    <tr>
      <th>4451</th>
      <td>Frankenweenie</td>
      <td>Tim Burton</td>
      <td>2012</td>
      <td>87</td>
    </tr>
    <tr>
      <th>7943</th>
      <td>Frankenweenie</td>
      <td>Tim Burton</td>
      <td>1984</td>
      <td>29</td>
    </tr>
    <tr>
      <th>4063</th>
      <td>Madea's Family Reunion</td>
      <td>Tyler Perry</td>
      <td>2002</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6701</th>
      <td>Madea's Family Reunion</td>
      <td>Tyler Perry</td>
      <td>2006</td>
      <td>110</td>
    </tr>
    <tr>
      <th>5202</th>
      <td>Saw</td>
      <td>James Wan</td>
      <td>2003</td>
      <td>9</td>
    </tr>
    <tr>
      <th>7011</th>
      <td>Saw</td>
      <td>James Wan</td>
      <td>2004</td>
      <td>103</td>
    </tr>
  </tbody>
</table>

The output shows that some movies have another movie with the same title/director/release_year. With online search, I learned that this happens sometimes; e.g., a director makes a 'mini' version before the full scale. So I kept all the records. 

## Missing values
The 'genre' had only 23 nulls, but there were many records with 0 value for the budget and/or revenue. I had to remove all the records that belonged to any of the following:
* null for 'genre'
* 0 for 'budget_adj'
* 0 for 'revenue_adj'

## Incorrect values
As a minimum effort to ensure correctness of budget and revenue values, I inspected data points at both ends (the lowest and highest).

### Top budget movie

```python
df[df['budget_adj'] == df['budget_adj'].max()]
```

<table style="border: 1; height: 68px;" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>budget</th>
      <th>revenue</th>
      <th>original_title</th>
      <th>genres</th>
      <th>vote_average</th>
      <th>release_year</th>
      <th>budget_adj</th>
      <th>revenue_adj</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2244</th>
      <td>46528</td>
      <td>425000000</td>
      <td>11087569</td>
      <td>The Warrior's Way</td>
      <td>Adventure|Fantasy|Action|Western|Thriller</td>
      <td>6.4</td>
      <td>2010</td>
      <td>425000000.0</td>
      <td>11087569.0</td>
    </tr>
  </tbody>
</table>
Although the above says that 'The Warrior's Way' is the highest budget movie by 425 million, online search revealed that the budget of it is 42.5 million. After this correction, the highest budget became \$368 millon of 'Pirates of the Caribbean: On Stranger Tides,' which is correct. 

### Top revenue movie
'Avartar' came out as the highest revenue movie, and online search confirmed it. 

### Bottom budget movie
I displayed several lowest budget movies, and got puzzled with extremely low values like \$1, \$50, or '\$100. I did online search and found that:
* 'Primer (2004)' is considered as the lowest budget movie (https://en.wikipedia.org/wiki/Low-budget_film)
* Many of the low budget values are wrong; for example, the budget of 'Lost & Found (1999)' is $30 million, not $1.3, 'Joyful Noise (2012)', $25 millon, not $23, 'Weekend (2011)', $0.14 millon, not $7755, etc. 
With the above findings, I inclined to believe that \$8081, budget of 'Primer (2004)', should be the lowest. I removed movies whose budget was lower than it. 

### Bottom revenue movie
Again, online search made me distrust data points whose revenue was lower than the revenue of 'Best Man Down' ($1840.6). They were removed. 

## Data type
```python
df.loc[:, 'genres'] = df['genres'].str.strip().str.split("|")
```
Values of 'genres' transformed from string to list, e.g., Action|Adventure|Science Fiction --> [Action, Adventure, Science Fiction], for convenience in later analysis.

## Outliers
Fundamentally, I identified records that might inhibit finding a general direction by looking into each variable, and removed them. 

### Budget

![Boxplot of budget]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/ol_budget.png)

Several records on the top stand out, but I considered them still continuous. 

### Number of movies by genres
For each genre, I calculated the number of movies involving it.

```python
# Generate a long format dataframe 
df_long = df.explode('genres')

# Count the number of inclusion of each genre.
df_long['genres'].value_counts()
```

<div class = "notebook_output">
<pre>
Drama              1746
Comedy             1347
Thriller           1197
Action             1078
Adventure           746
Romance             660
Crime               649
Science Fiction     518
Horror              460
Family              423
Fantasy             393
Mystery             344
Animation           200
Music               134
History             129
War                 119
Western              52
Documentary          34
Foreign              13
TV Movie              1
Name: genres, dtype: int64
</pre>
</div>

I noted that the bottom four genres are involved by a quite small number of movies, less than 100. The number of Western-involved movies is fewer than the half of War-involved movies. I decided to exclude these genres. 

### Vote scores
![Boxplot of vote score]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/ol_vote.png)

Judging that it is so far away from the nearest, I removed the data point with the lowest value. 

### ROIs
![Boxplot of ROI]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/ol_ROI.png)

The top two data points were considered too far away that I removed them. 

To sum up, the data wrangling step removed 7138 records leaving 3728 records in total. The largest removal, 7011 records, was due to value missingness.

# Univariate Exploration<a class="anchor" id="ch3"></a>
I surveyed the variables, budget, ROI, genres, and vote score, individually, before looking at relationships between them. 

## Budget
The budget is long right tailed, ranging from \$8081 to \$368 millon (redline for median). 

![Budget histogram (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/uni_budget.png)

For later analysis, I categorized values into four groups as follows. 

```python
qt = df['budget_adj'].quantile([0.25, 0.5, 0.75])

def binning4(x):
    if x < qt[0.25]:
        return "Low"
    elif (x >= qt[0.25]) & (x < qt[0.5]):
        return "Moderate"
    elif (x >= qt[0.5]) & (x < qt[0.75]):
        return 'Little High'
    elif x >= qt[0.75]:
        return 'High'
    
df['budget_level'] = df['budget_adj'].apply(binning4)
```
![Median and Mean of Budget by Budget Level (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/uni_budget_level.png)

## ROI
The ROI column is extremely long tailed, ranging from -1 to 699. More than 3500 out of 3728 fall into between -1 and 22.3 as shown below. 

![ROI histogram (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/uni_roi_histogram.png)

I categorized the ROI in the same way to the budget. 

![Median and Mean of ROI by ROI Level (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/uni_roi_levels.png)

## Genres
I plotted the percentage of movies involving each genre. It shows that almost the half of movies involved 'Drama', and 'Comedy' is the next most involved genre by about %35. 

![Percentage of movies by genre (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/uni_genres.png)

## Vote score
The vote score seems normally distributed (mean = 6.2, SD = 0.8).

![Histogram of vote scores (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/uni_vote.png)

# Q1: Did high budget movies achieve high ROI?<a class="anchor" id="ch4"></a>
I plotted a scatter plot (top), ROI distribution by budget level (bottom-left), and median value by budget level (bottom right). 

![Budget and ROI plots (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/bi_budget_ROI_plots.png)

It is hard to find a relationship in the scatter plot. But, the boxplot by the budget level shows that ROI values of low budget movies span a wide range, up to 500, the largest ROI value in the dataset. The larger budget groups have the narrower spread of ROIs. As well, the comparison of median values shows that the low and high budget levels have a little higher median values than the moderate and little high budget levels.

Meanwhile, I created a contigency table and plotted it as below.

![Number of movies by ROI level for each budget level]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/bi_budget_ROI_con.png)

It is found that low budget movies achieve the low and high level ROIs more frequently than the moderate and little high level ROIs. As opposed to it, high budget movies obtain the moderate and little high level ROIs more frequenlty.

# Q2: Did specific genres lead to high ROI?<a class="anchor" id="ch5"></a>
To compare ROIs between the genres, I calculated two types of means:
* Simple mean: Simply add up all ROIs and divide it with the total number of movies involving that genre. 
* Weighted mean: Assign weights based on the number genre types involved in a movie. A ROI is multiplied by 1/number_of_genres and the sum is divided by the sum of 1/number_of_genres values. 

For example, let's assume we have a long format dataframe of three movies as below:

| id | genre | ROI | number of genres|
|----|-------|-----|-----------------|
| 1  |drama  | 3   |2|
| 1  |comedy | 3   |2|
| 2  |comedy | 5   |1|
| 3  |drama  | 2   |1|

Simple means are:
- drama, (3+2) / 2 = 2.5
- comedy, (3+5) / 2 = 4

Weighted means are:
- drama, (3x1/2+2x1/1) / (1/2+1) = (1.5+2) / 1.5 = 2.33
- comedy, (3x1/2+5x1/1) / (1/2+1) = (1.5+5) / 1.5 = 4.33

The below shows results of the two methods. The results are not exactly the same (especailly, 'Horror' has a larger difference between the two results than other genrens), but  order is the same. 

![Mean of ROIs by genre (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/bi_genre.png)

The results hints that 'Horror' tends to bring quite larger ROIs compared to the others. The next profitable genre is 'Music' by round 4 or 5 smaller ROI than Horror. ROI values gradually decrease after 'Music, and thus the difference between 'Music' and 'History' is only about 3.5. 

Although I believe that I obtained an acceptable finding with my approach, it is a shame that effect of combination of genres was not studied. This issue will be discussed a bit more in 'Limitations' section at the end. 

# Q3: Did high score movies achieve high ROI?<a class="anchor" id="ch6"></a>

![Vote score distribution by ROI level (Cleaned Dataset)]({{ site.baseurl }}/assets/img/2022-06-01-what-are-associated-with-profitable-movies/bi_vote.png)

The chart shows that the high ROI level tends to bring a little higher median of vote scores (0.4 higher than the little high level, and 0.7 than the low level).

# Conclusions<a class="anchor" id="ch7"></a>

## Summary
With the exploration, I found association between 1) budget size and ROI, 2) genres and ROI, and 3) vote average score and ROI. To sum up:
- I found contrary behavior between the low and high budget groups, in terms of their ROIs. Low budget movies are more likely to be big fail or big success, and high budget movies tend to end up in the middle. This behavior seems a consequence. Considering different capabilities in reaching the audience and the definition of ROI, this behavior seems natural. 
- The genre type that resulted in the highest ROI on average is 'Horror', and the smallest ROI, 'History',  among 17 genre types (excluding 'Western', 'Documentary', 'Foreign', and 'TV Movie'). 
- It was found that the higher ROI level group tend to associate with higher mean of the vote average.

Based on these findings, it is considered that production of low budget, horror movies that receive high score from the audience is likely to lead to high ROI.

## Limitations
The exploration has several limitations as follows:
- Only about a third of the original datapoints were used in the analysis, since the rest had missing or outlying values. As well, there was no further work to check if any pattern exists in missing or outlying values.
- In finding association between genres and ROIs, potential effect of genre combination was not explored, and it makes the result somewhat insecure. For instance, let's assume the following dataset of four movies:

| id | genres| ROI |
|----|-------|-----|
|1|Drama|2|
|2|Drama, Family|8|
|3|Drama, Horror|8|
|4|Family|2|

Calculation of weighted means (Drama, 5; Family, 4; Horor, 8) indicates that the highest ROI genre is Horror. However, it is not sure if this is the power of the Horror genre or result of pairing with Drama, since it does not have movies that involve only Horror.
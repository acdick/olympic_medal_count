# Olympic Medal Count

How many total medals will a country win at an Olympic Games?

## Open Data Sources

Data for this project was collected from two sources.

### 120 Years of Olympic History: Athletes and Results

https://www.kaggle.com/heesoo37/120-years-of-olympic-history-athletes-and-results

Two CSV files are available containing athlete results and Olympic country identifiers (NOCs).

271,116 athlete records contain the following data:

* ID
* Name
* Sex
* Age
* Height
* Weight
* Team
* NOC
* Games
* Year
* Season
* City
* Sport
* Event
* Medal

230 regions are referenced by:

* NOC
* region
* notes

### Olympic Sports and Medals, 1896-2014

https://www.kaggle.com/the-guardian/olympic-games

Three CSV files contain data about NOC countries and winners of the summer and winter Olympic games.

201 NOC countries are referenced:

* Country
* Code
* Population
* GDP per Capita

31.2k summer and 5,770 medal winners:

* Year
* City
* Sport
* Discipline
* Athlete
* Country
* Gender
* Event
* Medal

### Collecting the Data into a Pandas DataFrame

The data sources were loaded into a SQL database, queried and joined to create a unified data set.

Individual records were grouped by team and year to generate aggregate country information.

Derived information for each year includes:

* Total athletes per team
* Percentage of total athletes competing per team
* Total females per team
* Ratio of females to total athletes per team
* Total sports competed per team
* Percentage of total sports compted per team
* Mean age of competitors per team
* Mean height of competitors per team
* Mean weight of competitors per team
* Total medals won per team
* Total gold medals won per team
* Total silver medals won per team
* Total bronze medals won per team
* Team win percentage of all medals available

The cleaned Pandas DataFrame has 3,091 team records with 17 columns.

![DataFrame](/Plots/DataFrame.png)

![Medals](/Plots/Medals.png)


## Exploratory Data Analysis

The data was explored by viewing the distributions of individual competitors, team compositions and sports participation.

### Individual Competitors

The physical attributes (age, height, weight) of individual competitors follows a normal distribution.

![Age](/Plots/Age.png)

![Height](/Plots/Height.png)

![Weight](/Plots/Weight.png)

### Team Composition

The team compositions are skewed in terms of total athletes and female ratio.

Most teams send few athletes to the Olympics, while only a few teams reach over 500 competitors.

![Total Athletes](/Plots/Total_Athletes.png)

Most teams historically have a low ratio of females.

In many of the initial years of the Olympics, only males competed.

![Female Ratio](/Plots/Female_Ratio.png)

### Sports Participation

Most teams compete in only a few sports, consistent with few athletes per team.

![Sports Competed](/Plots/Sports_Competed.png)

Some teams are able to compete in all sports offered at a given Olympics, but most participate in less than 25%.

![Sports Percentage](/Plots/Sports_Percentage.png)


## Multi-Linear Regression

The raw and derived variables were correlated with each other to understand which variables would be best suited for regression.

Three multi-linear regression models were developed to explain and predict the variance of the historic data.

### Variable Correlations

![Variables Pairplot](/Plots/Variables_Pairplot.png)

![Dependent Variables Pairplot](/Plots/Dependent_Variables_Pairplot.png)

![Correlation Heatmap](/Plots/Correlation_Heatmap.png)

![Categorical Country Correlation](/Plots/Categorical_Country_Correlation.png)

### Regression Model Fit

![Multi_Linear_Regression](/Plots/Multi_Linear_Regression.png)
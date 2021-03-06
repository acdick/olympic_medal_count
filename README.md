# Multilinear Regression of Olympic Medal Count

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

31.2k summer and 5,770 winter medal winners are separately recorded:

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

The cleaned Pandas DataFrame has 3,091 team records with 15 data columns.

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

A multi-linear regression model was developed to explain and predict the variance of the historic data.

### Variable Correlations

The independent and dependent variables were plotted against each other in a pairplot to visualize potential relationships.

![Variables Pairplot](/Plots/Variables_Pairplot.png)

The correlations between the variables were plotted in a correlation heatmap to identify correlated variables.

* Physical attributes of individual competitors (age, height, weight) has low correlation with total medals won.
* Similarly, year and female ratio has low correlation with total medals won.
* Finally, golds, silvers, bronzes and win percentage is highly correlated with total medals won, but cannot be included in a predictive model because they represent posterior knowledge.

![Correlation Heatmap](/Plots/Correlation_Heatmap.png)

The independent variables that were ultimately used in the regression are total athletes, sports competed and athletes per sport.

![Dependent Variables Pairplot](/Plots/Dependent_Variables_Pairplot.png)

Countries were transformed into categorical variables and those most highly correlated with the target variable were investigated in the linear regression model.

![Categorical Country Correlation](/Plots/Categorical_Country_Correlation.png)

### Regression Model Fit

**Initial Model: Total Athletes**

An initial regression model examined the relationship between total medals won and total athletes by team.

* Total athletes by team explains 65.3% of the variance in total medal count.
* The coefficient of 0.1962 means that a team would be expected to win an additional medal for every five athletes.
* A model that uses all 10 dependent variables only explains 70.6% of the variance.

![OLS Model 1](/Plots/OLS_Model_1.png)

**Augmented Model: Total Athletes + (Sports Competed + Athletes per Sport)**

An augmented regression model included the total number of sports competed and the number of athletes per sport (interaction).

* This model explained 70.7% of the variance in medal count, but athletes per sport contributed 0.6%.
* The negative correlation of -2.2385 means that a team would be expected to wins two fewer medals for every two athletes.
* Similarly, three fewer medals would be earned for every four athletes per sport.

![OLS Model 2](/Plots/OLS_Model_2.png)

**Final Model: Total Athletes + (Sports Competed + Athletes per Sport) + Specific Countries**

The final regression model includes individual countries as categorical variables.

* The interaction of the number of athletes per sport was removed due to its low relevance in explaining the target variable.
* The countries most correlated with the target variable were investigated.
* Total athletes, sports competed and the 10 most correlated countries explain 79.2% of the variance.
* Including only the USA, USSR, East Germany, former USSR and France explains 79.0% of the variance.
* The USA would be expected to win 51 more medals than other countries, while the USSR would win 148 more.

![OLS_Model_3](/Plots/OLS_Model_3.png)

## Future Improvements

A linear regression model can be improved by investigating additional variables available in the dataset.

* A categorical variable to differentiate models won in the summer or winter Olympics
* A categorical variable to investigate the effect of being a host country on winning medals
* A categorical variable to divide years into eras (i.e. when females began to compete, end of the Cold War, etc.)

### Heteroscedasticity of Residuals

The final regression model exhibits heteroscedasticity of residuals across total athletes, sports completed and specific countries.

![Heteroscedasticity_Total_Athletes](/Plots/Heteroscedasticity_Total_Athletes.png)

![Heteroscedasticity_Sports_Competed](/Plots/Heteroscedasticity_Sports_Competed.png)

![Heteroscedasticity_USA](/Plots/Heteroscedasticity_USA.png)

### Normality of Residuals

The normality of the residuals of the multi-linear regression model should be improved.

![QQ_Plot](/Plots/QQ_Plot.png)

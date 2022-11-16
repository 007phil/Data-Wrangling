Top Youtubers and Trending Videos Analysis
================
Group 8: Xu Li, Yo-Wen Chang, Peixin Lin, Ruiying Liang, Runqing Liu,
Hans Yan, Kok Kent Chong

![alt text here](1668558779423.jpg) # Introduction

YouTube is an American online video-sharing and social media platform
owned by Google. It is the world’s most popular and most used video
platform today, with more than one billion monthly users who
collectively watch more than one billion hours of videos each day.

YouTube has had an unprecedented social impact, influencing popular
culture and internet trends and creating multimillionaire celebrities.
To get an insight into the trending culture and topics, we will analyze
the dataset of most subscribed YouTube channels, including categories,
number of subscribers and views, etc. In addition, YouTube has a list of
trending videos that is updated constantly, which we could use to take
one more step in analyzing its trending videos to see what is typical
between them. On the other hand, these insights could also be useful for
creators who want to increase the popularity of their videos on YouTube.

Our analytics focus on three key topics: 1)Engagement difference by
Youtubers’ nationalities & categories 2)Viewers’ negative sentiment by
their countries & videos categories 3)Viewership Habits by Countries

Characteristics of trending videos or youtubers are carefully examined
with insights and findings summarized in the end.

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✔ tibble  3.1.6     ✔ purrr   0.3.4
    ## ✔ readr   2.1.1     ✔ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ lubridate::as.difftime() masks base::as.difftime()
    ## ✖ readr::col_factor()      masks scales::col_factor()
    ## ✖ lubridate::date()        masks base::date()
    ## ✖ purrr::discard()         masks scales::discard()
    ## ✖ dplyr::filter()          masks stats::filter()
    ## ✖ lubridate::intersect()   masks base::intersect()
    ## ✖ dplyr::lag()             masks stats::lag()
    ## ✖ lubridate::setdiff()     masks base::setdiff()
    ## ✖ lubridate::union()       masks base::union()

    ## Loading required package: NLP

    ## 
    ## Attaching package: 'NLP'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     annotate

    ## 
    ## Attaching package: 'psych'

    ## The following objects are masked from 'package:ggplot2':
    ## 
    ##     %+%, alpha

    ## The following objects are masked from 'package:scales':
    ## 
    ##     alpha, rescale

    ## Loading required package: colorspace

    ## Loading required package: grid

    ## VIM is ready to use.

    ## Suggestions and bug-reports can be submitted at: https://github.com/statistikat/VIM/issues

    ## 
    ## Attaching package: 'VIM'

    ## The following object is masked from 'package:datasets':
    ## 
    ##     sleep

    ## Loading required package: carData

    ## 
    ## Attaching package: 'car'

    ## The following object is masked from 'package:psych':
    ## 
    ##     logit

    ## The following object is masked from 'package:purrr':
    ## 
    ##     some

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     recode

    ## 
    ## Attaching package: 'reshape'

    ## The following objects are masked from 'package:tidyr':
    ## 
    ##     expand, smiths

    ## The following object is masked from 'package:lubridate':
    ## 
    ##     stamp

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     rename

# Dataset1 Import and Preprocessing

## Data Description

``` r
#load data
youtuber <- read.csv("youtube200.csv", header=T)
head(youtuber)
```

    ##   Country               ChannelName      Category MainVideoCategory
    ## 1      IN                  T-Series Gaming & Apps             Music
    ## 2      US ABCkidTV - Nursery Rhymes Gaming & Apps         Education
    ## 3      IN                 SET India Gaming & Apps             Shows
    ## 4      US                 PewDiePie Gaming & Apps            Gaming
    ## 5      US                   MrBeast Gaming & Apps     Entertainment
    ## 6      US               Like Nastya                  People & Blogs
    ##                    username followers     Maintopic CountTopics        Topic1
    ## 1                  T-Series 220000000 Music of Asia           4 Entertainment
    ## 2 ABCkidTV - Nursery Rhymes 138000000        Movies           3 Entertainment
    ## 3                 SET India 137000000        Movies           4 Entertainment
    ## 4                 PewDiePie 111000000     Lifestyle           4        Gaming
    ## 5                   MrBeast  98100000     Lifestyle           3 Entertainment
    ## 6               Like Nastya  97300000         Hobby           2     Lifestyle
    ##          Topic2     Topic3                Topic4 Topic5 Topic6 Topic7 Topic8
    ## 1 Music of Asia      Music                Movies                            
    ## 2         Music     Movies                                                  
    ## 3      TV shows      Music                Movies                            
    ## 4   Action game  Lifestyle Action-adventure game                            
    ## 5     Lifestyle Technology                                                  
    ## 6         Hobby                                                             
    ##        Likes BoostIndex EngagementRate EngagementRate60days        Views
    ## 1 1602680172         83     0.03346313          0.010879483 196000000000
    ## 2  220990135         63     0.64171570          0.116003684 133000000000
    ## 3  174875243         79     0.00120627          0.002365693 122000000000
    ## 4 2191405542         88     0.06342613          0.044846075  28424113942
    ## 5 1731833461         60     0.72920955          0.570270225  16242634269
    ## 6  280877652         79     1.02676067          0.231388060  80111555805
    ##      ViewsAvg   Avg1Day Avg3Day    Avg7Day   Avg14Day   Avg30day   Avg60day
    ## 1   2095329.1  152244.8 2134570  1809830.4  2306178.2  1676329.8  2295416.1
    ## 2  70271260.2 1837916.0 1837916  4891832.0  7052576.5 12654329.7 15722844.7
    ## 3    109572.9        NA  586040   280127.6   343788.1   353601.9   322033.6
    ## 4   7718344.8        NA      NA  3497395.0  3094440.4  3620273.5  4454119.6
    ## 5  98762497.2        NA      NA 29941021.0 29941021.0 29941021.0 53434733.0
    ## 6 138163704.5        NA 3946868  5829929.0  9480138.2 13756907.7 22108751.1
    ##    CommentsAvg              YoutubeLink
    ## 1 4.493984e+03 UCq-Fj5jknLsUf-MWSy4_brA
    ## 2 1.467003e+02 UCbCmjCuTUZos6Inko4u57UQ
    ## 3 7.624432e+01 UCpEhnqL0y41EpW2TvWAHD7Q
    ## 4 3.583978e+04 UC-lHJZR3Gqxm24_Vd_AJ5Yw
    ## 5 1.134324e+05 UCX6OQ3DkcsbYNE6H8uQQuVA
    ## 6 2.924901e-01 UCJplp5SjeGSdVdwsfb9Q7lQ

``` r
#data description
describe(youtuber)
```

    ##                      vars   n         mean           sd       median
    ## Country*                1 200 1.640000e+01 1.030000e+01 1.500000e+01
    ## ChannelName*            2 200 9.950000e+01 5.787000e+01 9.950000e+01
    ## Category*               3 200 4.140000e+00 1.890000e+00 4.000000e+00
    ## MainVideoCategory*      4 200 7.420000e+00 2.920000e+00 7.500000e+00
    ## username*               5 200 9.950000e+01 5.787000e+01 9.950000e+01
    ## followers               6 200 3.947750e+07 2.262278e+07 3.255000e+07
    ## Maintopic*              7 200 1.163000e+01 5.230000e+00 1.200000e+01
    ## CountTopics             8 200 3.000000e+00 1.090000e+00 3.000000e+00
    ## Topic1*                 9 200 6.860000e+00 3.550000e+00 8.000000e+00
    ## Topic2*                10 200 9.240000e+00 4.670000e+00 1.000000e+01
    ## Topic3*                11 200 8.350000e+00 6.650000e+00 5.500000e+00
    ## Topic4*                12 200 2.730000e+00 3.420000e+00 1.000000e+00
    ## Topic5*                13 200 1.360000e+00 1.510000e+00 1.000000e+00
    ## Topic6*                14 200 1.050000e+00 3.900000e-01 1.000000e+00
    ## Topic7*                15 200 1.000000e+00 7.000000e-02 1.000000e+00
    ## Topic8*                16 200 1.000000e+00 7.000000e-02 1.000000e+00
    ## Likes                  17 200 1.813016e+08 3.084215e+08 7.041162e+07
    ## BoostIndex             18 200 6.497000e+01 1.649000e+01 7.000000e+01
    ## EngagementRate         19 199 4.600000e-01 1.070000e+00 1.200000e-01
    ## EngagementRate60days   20 200 8.000000e-02 1.800000e-01 3.000000e-02
    ## Views                  21 200 2.070922e+10 2.126891e+10 1.631175e+10
    ## ViewsAvg               22 199 1.663930e+07 4.082130e+07 2.911653e+06
    ## Avg1Day                23 117 1.653715e+05 4.385792e+05 2.630938e+04
    ## Avg3Day                24 153 4.580053e+05 9.845389e+05 7.633722e+04
    ## Avg7Day                25 172 9.767794e+05 2.721150e+06 1.495268e+05
    ## Avg14Day               26 182 1.744766e+06 6.046917e+06 2.505723e+05
    ## Avg30day               27 191 2.190457e+06 6.533621e+06 3.817325e+05
    ## Avg60day               28 200 2.714340e+06 7.017056e+06 5.394513e+05
    ## CommentsAvg            29 199 1.373315e+04 2.960144e+04 1.209180e+03
    ## YoutubeLink*           30 200 1.005000e+02 5.788000e+01 1.005000e+02
    ##                           trimmed          mad        min          max
    ## Country*             1.687000e+01 1.927000e+01 1.0000e+00 2.800000e+01
    ## ChannelName*         9.950000e+01 7.413000e+01 1.0000e+00 1.990000e+02
    ## Category*            4.180000e+00 2.970000e+00 1.0000e+00 9.000000e+00
    ## MainVideoCategory*   7.240000e+00 2.220000e+00 1.0000e+00 1.700000e+01
    ## username*            9.950000e+01 7.413000e+01 1.0000e+00 1.990000e+02
    ## followers            3.475688e+07 9.414510e+06 2.4000e+07 2.200000e+08
    ## Maintopic*           1.132000e+01 4.450000e+00 1.0000e+00 2.400000e+01
    ## CountTopics          2.980000e+00 1.480000e+00 0.0000e+00 8.000000e+00
    ## Topic1*              6.730000e+00 1.480000e+00 1.0000e+00 1.600000e+01
    ## Topic2*              9.270000e+00 4.450000e+00 1.0000e+00 1.700000e+01
    ## Topic3*              7.840000e+00 6.670000e+00 1.0000e+00 2.300000e+01
    ## Topic4*              1.840000e+00 0.000000e+00 1.0000e+00 1.500000e+01
    ## Topic5*              1.000000e+00 0.000000e+00 1.0000e+00 1.000000e+01
    ## Topic6*              1.000000e+00 0.000000e+00 1.0000e+00 5.000000e+00
    ## Topic7*              1.000000e+00 0.000000e+00 1.0000e+00 2.000000e+00
    ## Topic8*              1.000000e+00 0.000000e+00 1.0000e+00 2.000000e+00
    ## Likes                1.089301e+08 8.018352e+07 0.0000e+00 2.191406e+09
    ## BoostIndex           6.765000e+01 1.334000e+01 1.0000e+00 8.800000e+01
    ## EngagementRate       2.400000e-01 1.600000e-01 0.0000e+00 1.058000e+01
    ## EngagementRate60days 4.000000e-02 3.000000e-02 0.0000e+00 1.520000e+00
    ## Views                1.702780e+10 9.141752e+09 0.0000e+00 1.960000e+11
    ## ViewsAvg             7.594484e+06 4.154363e+06 6.8061e+02 4.239235e+08
    ## Avg1Day              6.339084e+04 3.900628e+04 0.0000e+00 3.472638e+06
    ## Avg3Day              2.133606e+05 1.131776e+05 0.0000e+00 6.596001e+06
    ## Avg7Day              4.064249e+05 2.216884e+05 0.0000e+00 2.994102e+07
    ## Avg14Day             5.995327e+05 3.714984e+05 0.0000e+00 6.877732e+07
    ## Avg30day             8.008941e+05 5.622374e+05 0.0000e+00 6.877732e+07
    ## Avg60day             1.091419e+06 7.997906e+05 0.0000e+00 5.383523e+07
    ## CommentsAvg          6.241340e+03 1.792730e+03 0.0000e+00 1.995235e+05
    ## YoutubeLink*         1.005000e+02 7.413000e+01 1.0000e+00 2.000000e+02
    ##                             range  skew kurtosis           se
    ## Country*             2.700000e+01 -0.22    -1.38 7.300000e-01
    ## ChannelName*         1.980000e+02  0.00    -1.22 4.090000e+00
    ## Category*            8.000000e+00 -0.26    -0.65 1.300000e-01
    ## MainVideoCategory*   1.600000e+01  0.48     0.09 2.100000e-01
    ## username*            1.980000e+02  0.00    -1.22 4.090000e+00
    ## followers            1.960000e+08  3.95    22.45 1.599672e+06
    ## Maintopic*           2.300000e+01  0.27    -0.24 3.700000e-01
    ## CountTopics          8.000000e+00  0.57     1.92 8.000000e-02
    ## Topic1*              1.500000e+01  0.05    -0.62 2.500000e-01
    ## Topic2*              1.600000e+01 -0.11    -0.81 3.300000e-01
    ## Topic3*              2.200000e+01  0.36    -1.22 4.700000e-01
    ## Topic4*              1.400000e+01  1.94     2.60 2.400000e-01
    ## Topic5*              9.000000e+00  4.19    16.52 1.100000e-01
    ## Topic6*              4.000000e+00  8.37    73.10 3.000000e-02
    ## Topic7*              1.000000e+00 13.93   193.03 0.000000e+00
    ## Topic8*              1.000000e+00 13.93   193.03 0.000000e+00
    ## Likes                2.191406e+09  3.50    15.06 2.180869e+07
    ## BoostIndex           8.700000e+01 -1.48     2.05 1.170000e+00
    ## EngagementRate       1.058000e+01  5.89    44.76 8.000000e-02
    ## EngagementRate60days 1.520000e+00  5.45    35.50 1.000000e-02
    ## Views                1.960000e+11  4.51    27.85 1.503939e+09
    ## ViewsAvg             4.239228e+08  6.09    50.62 2.893745e+06
    ## Avg1Day              3.472638e+06  4.85    28.56 4.054666e+04
    ## Avg3Day              6.596001e+06  3.64    15.33 7.959525e+04
    ## Avg7Day              2.994102e+07  7.48    72.65 2.074858e+05
    ## Avg14Day             6.877732e+07  8.38    83.64 4.482273e+05
    ## Avg30day             6.877732e+07  6.87    58.95 4.727562e+05
    ## Avg60day             5.383523e+07  5.09    29.90 4.961808e+05
    ## CommentsAvg          1.995235e+05  3.32    12.46 2.098390e+03
    ## YoutubeLink*         1.990000e+02  0.00    -1.22 4.090000e+00

## Missing Values Checking

``` r
vis_miss(youtuber)
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
gg_miss_var(youtuber,show_pct = TRUE) 
```

    ## Warning: It is deprecated to specify `guide = FALSE` to remove a guide. Please
    ## use `guide = "none"` instead.

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-3-2.png)<!-- -->

``` r
#Missing data
pct_miss(youtuber) #overall percent of missing data
```

    ## [1] 3.133333

``` r
pct_miss(youtuber$Avg1Day)
```

    ## [1] 41.5

``` r
pct_miss(youtuber$Avg3Day)
```

    ## [1] 23.5

``` r
pct_miss(youtuber$Avg7Day)
```

    ## [1] 14

``` r
pct_miss(youtuber$Avg14Day)
```

    ## [1] 9

The percentages of missing data in Avg1Day and Avg3Day is higher than
20%, especially for Avg1Day.These two variables are not suitable for
further analysis as the percentages of missing data is high, making
attempt to impute values will not make much sense.

``` r
matrixplot(youtuber, sortby = "Views")
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

    ## 
    ## Click in a column to sort by the corresponding variable.
    ## To regain use of the VIM GUI and the R console, click outside the plot region.

Here, the black and white variables represent a heatmap of values within
each column, and red indicates a missing value. This can be useful for
identifying if missing patterns are dependent on observed data values.
It seems there is no distinct pattern for the missing values.

# Descriptive Statistics

``` r
ggplot(youtuber, aes(x=followers)) + 
 geom_histogram(aes(y=..count..), colour="black", fill="#A52A2A")+
 geom_density(alpha=.2, fill="#FF6666")+ scale_x_continuous(labels = function(x) format(x, scientific = TRUE))+theme_classic()+theme(axis.text = element_text(size = 20),axis.title = element_text(size = 20))
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
Majority of the accounts have 25-30M followers. Most of them have less
than 100M followers and about 50% of them have less than 50M

``` r
ggplot(youtuber, aes(x=Views)) + 
 geom_histogram(aes(y=..count..), colour="black", fill="#A52A2A")+
 geom_density(alpha=.2, fill="#FF6666")+ scale_x_continuous(labels = function(x) format(x, scientific = TRUE))+theme_classic()+theme(axis.text = element_text(size = 20),axis.title = element_text(size = 20))
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->
Like the followers, the views are also left skewed with most of the
accounts having about 25 Billion views from all the video posted thus
far

``` r
ggplot(youtuber, aes(x=Likes)) + 
geom_histogram(aes(y=..count..), colour="black", fill="#A52A2A", bins = 50)+ scale_x_continuous(labels = function(x) format(x, scientific = TRUE))+
theme_classic()+theme(axis.text = element_text(size = 20),axis.title = element_text(size = 20))
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
On average, the top 200 youtube channels have about 233M likes
respectively on their channels

``` r
# remove column if the country column is empty
youtuber_country = youtuber[!(is.na(youtuber$Country) | youtuber$Country==""), ]
# top 200 youtubers by country
topyoutuber_country = 
  youtuber_country %>% 
  head(200)  %>%
  group_by(Country) %>%
  summarize( Count=n()
             ,Percent=round(Count/nrow(.)*100,digits=2)
           ) %>%
  arrange(desc(Count)) %>%
  head(10) 

ggplot(topyoutuber_country, aes(x = reorder(Country,-Count), y = Count)) +
    geom_bar(stat="identity", color='black',fill='#A52A2A')+
    geom_text(aes(label=Count), vjust = -0.5, colour = "black") +
    scale_x_discrete(name ="Country") +
    theme_classic()+theme(axis.text = element_text(size = 15),axis.title = element_text(size = 15))
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
# create language dataframe
language = data.frame (
  countrylist  = c("US","IN","BR","KR","MX","RU","BY","CA","CL","NO"),
  Language = c("English","English","Portuguese","Korean","Spanish","Russian","Russian","English","Spanish","Norwegian")
)

# join the language with the country and display the result in the bar chart
topyoutuber_country %>%
  left_join(language,by = c("Country" = "countrylist")) %>%
  group_by(Language) %>%
  summarize( Count_Youtuber=sum(Count)) %>%
  arrange(desc(Count_Youtuber)) %>%
  ggplot(aes(x = reorder(Language,-Count_Youtuber), y = Count_Youtuber)) +
    geom_bar(stat="identity", color='black',fill='#A52A2A')+
    geom_text(aes(label=Count_Youtuber), vjust = -0.5, colour = "black") +
    scale_x_discrete(name ="Language") +
    theme_classic()+theme(axis.text = element_text(size = 15),axis.title = element_text(size = 15))
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
According to the findings, top2 countries for top200 youtubers are
Indian and US. Moreover, creating channels in English-speaking countries
is more likely to reach a global audience.

``` r
df <- youtuber %>% 
  group_by(MainVideoCategory) %>% # Variable to be transformed
  count() %>% 
  ungroup() %>% 
  mutate(perc = `n` / sum(`n`)) %>% 
  arrange(perc) %>%
  mutate(labels = scales::percent(perc))

ggplot(df, aes(x = MainVideoCategory, y = n, fill = n)) +
    geom_bar(binwidth=1, stat="identity") +theme_light() +
    scale_fill_gradient(low="#A52A2A", high="#FA8072",limits=c(0,320)) +
    theme(axis.title.y=element_text(angle=0))+ 
    coord_polar()+ 
    aes(x=reorder(MainVideoCategory, n)) 
```

    ## Warning: Ignoring unknown parameters: binwidth

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
More than 50% of the top 200 Youtubers fall under the category of
“Music” and “Entertainment”, with gaming coming third.

# Key Topic 1:Engagement difference by Youtubers’ nationalities & categories

``` r
youtuber$like_rate=youtuber$Likes/youtuber$Views
youtuber_top_country<- youtuber %>%
 filter(Country=='US'|Country=='IN'|Country=='BR'|Country=='KR'|Country=='MX'|Country=='RU') %>%
 select_all()
ggplot(data=youtuber_top_country,aes(x=Country, y=like_rate, size=followers))+geom_boxplot()+geom_point(color='#A52A2A')
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
The like rate distribution by countries is skewed to the lower level
with only a few distinguished youtubers (outliers) with higher like
rate.Besides that, many youtubers with a higher like-rate have less
followers.

``` r
options(repr.plot.width=15, repr.plot.height=8)
youtuber_top_Cate<- youtuber %>%
 filter(MainVideoCategory=='Music'| MainVideoCategory=='Entertainment'|MainVideoCategory=='Education'|MainVideoCategory=='Gaming'|MainVideoCategory=='People & Blogs') %>%
 select_all()

ggplot(data=youtuber_top_Cate,aes(x=MainVideoCategory, y=like_rate, size=followers))+geom_boxplot()+geom_point(color='#A52A2A')
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->
Among the top 5 categories, the categories with highest like rate are
Gaming videos, which is also with a high variance in like rate. The
category with lowest average like rate are ducation videos, whose
distribution is relatively concentrated (small variance)

``` r
library(corrplot)
```

    ## corrplot 0.92 loaded

``` r
options(repr.plot.width=16, repr.plot.height=16)
youtuber_numeric<-youtuber[,c(6,8,17:29)]
youtuber_numeric<-youtuber_numeric[,colSums(is.na(youtuber_numeric))==0]
cor_data<-cor(youtuber_numeric)
corrplot(corr =cor_data, p.mat = cor_data,method = "square",
         type="upper",
         tl.pos="lt", tl.cex=1, tl.col="black",tl.srt = 45,tl.offset=0.5,
         insig="label_sig",sig.level = c(.001, .01, .05),
         pch.cex = 0.8,pch.col = "black",col = colorRampPalette(c("grey", "#D22B2B"))(5))
corrplot(corr = cor_data, method = "number",
         type="lower",add=TRUE,
         tl.pos = "n",cl.pos = "n",diag=FALSE,
         number.digits = 3,number.cex = 0.5,col = "black",number.font = NULL)
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->
Views and followers have the highest correlation. Further analytics on
the correlation will be conducted below.

``` r
options(repr.plot.width=10, repr.plot.height=8)
ggplot(data=youtuber_top_Cate,aes(x=log(followers), y=Views, color=MainVideoCategory))+geom_point(size=3)+
  stat_smooth(method='lm',formula=y~x)+scale_color_brewer(palette='Reds')
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
options(repr.plot.width=10, repr.plot.height=8)
ggplot(data=youtuber_top_country,aes(x=log(followers), y=Views, color=Country))+geom_point(size=3)+geom_smooth(method='lm')+scale_color_brewer(palette='Reds')
```

    ## `geom_smooth()` using formula 'y ~ x'

    ## Warning in qt((1 - level)/2, df): NaNs produced

    ## Warning in max(ids, na.rm = TRUE): no non-missing arguments to max; returning
    ## -Inf

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

``` r
#anova
ano_cate<-aov(Views/log(followers)~MainVideoCategory,data=youtuber_top_Cate)
summary(ano_cate)
```

    ##                    Df    Sum Sq   Mean Sq F value Pr(>F)
    ## MainVideoCategory   4 8.876e+18 2.219e+18    1.75  0.142
    ## Residuals         150 1.902e+20 1.268e+18

``` r
ano_cty<-aov(Views/log(followers)~Country,data=youtuber_top_country)
summary(ano_cty)
```

    ##              Df    Sum Sq   Mean Sq F value Pr(>F)
    ## Country       5 3.175e+18 6.349e+17    0.35  0.882
    ## Residuals   122 2.216e+20 1.817e+18

Grouping by category, number of youtuber’s followers has positive
correlation with their number of views. Youtubers on Education category
get higher number of views than other categories when they have the same
number of followers. Gaming youtubers get the lowest number of views
when having the similar magnitude followers. Grouping by country, With
the same magnitude followers, Youtubers’ views are highest in India,
followed by US, BR and the Korean(lowest) The ANOVA tests indicate the
difference of views/log(followers) is significant for different
categories and countries.

# Dataset2 Import and Preprocessing

## Data Loading

Our original data consists of 5 different videos collected from Kaggle.
US_raw, GB_raw, CA_raw and IN_raw are the records of trending videos in
United States, Great British, Canada and India respectively. Dataframe
youtuber consists the information about the top 200 most popular
youtubers. Besides, our original data only contain a categoryID but miss
the name of each category, it is essential to find what category each
categroyID is corresponding to. For that purpose, other four json files
are also read.

``` r
#read csv
US_raw <- read.csv("US_youtube_trending_data.csv")
```

    ## Warning in scan(file = file, what = what, sep = sep, quote = quote, dec = dec, :
    ## embedded nul(s) found in input

``` r
GB_raw <- read.csv("GB_youtube_trending_data.csv")
CA_raw <- read.csv("CA_youtube_trending_data.csv")
```

    ## Warning in scan(file = file, what = what, sep = sep, quote = quote, dec = dec, :
    ## embedded nul(s) found in input

``` r
IN_raw <- read.csv("IN_youtube_trending_data.csv")
youtuber <- read.csv("top_200_youtubers.csv")

#read json
us_cat_name <- fromJSON("US_category_id.json")
gb_cat_name <- fromJSON("GB_category_id.json")
ca_cat_name <- fromJSON("CA_category_id.json")
in_cat_name <- fromJSON("IN_category_id.json")
```

##D ata Preprocessing Before diving deeper into data for further
analysis, it is significant to check the quality of the data. In this
step, we did some dirty data work including dropping irrelevant columns,
formatting date and time, add other necessary columns, joining csv with
json to category name and delete duplicate rows whose title stayed on
the trending list for days.

``` r
US <- US_raw
GB <- GB_raw
CA <- CA_raw
IN <- IN_raw

#drop irrelevant columns
US <- subset(US, select = -c(video_id, channelId,thumbnail_link,description))
GB <- subset(GB, select = -c(video_id, channelId,thumbnail_link,description))
CA <- subset(CA, select = -c(video_id, channelId,thumbnail_link,description))
IN <- subset(IN, select = -c(video_id, channelId,thumbnail_link,description))

#add country column
US[,"country"] <- "US"
GB[,"country"] <- "GB"
CA[,"country"] <- "CA"
IN[,"country"] <- "IN"

#date time format
US$publishedAt <- ymd_hms(US$publishedAt)
US$trending_date <- ymd_hms(US$trending_date)
GB$publishedAt <- ymd_hms(GB$publishedAt)
GB$trending_date <- ymd_hms(GB$trending_date)
CA$publishedAt <- ymd_hms(CA$publishedAt)
CA$trending_date <- ymd_hms(CA$trending_date)
IN$publishedAt <- ymd_hms(IN$publishedAt)
IN$trending_date <- ymd_hms(IN$trending_date)
```

``` r
#Match Category names to Category IDs from US_category_id.json file

#extract ids of categories into a vector
list <- us_cat_name[["items"]]
items <- length(list)

US$categoryId <- as.character(US$categoryId)
ids <- numeric(items)
for(i in seq_along(list)){
  ids[i] <- us_cat_name[["items"]][[i]][[3]]
}

#extract names of categories into a vector
names <- character(items)
for(i in seq_along(list)){
  names[i] <- us_cat_name[["items"]][[i]][[4]][[1]]
}

#join dataframe with category name and drop categoryId
cat <- as.data.frame(cbind(ids,names))
US <- US %>%
  left_join(cat, by=c("categoryId"="ids")) %>%
  dplyr::rename("category"="names") %>%
  select(-categoryId)
```

``` r
#Match Category names to Category IDs from GB_category_id.json file

#extract ids of categories into a vector
gb_list <- gb_cat_name[["items"]]
gb_items <- length(gb_list)

GB$categoryId <- as.character(GB$categoryId)
gb_ids <- numeric(gb_items)
for(i in seq_along(gb_list)){
  gb_ids[i] <- gb_cat_name[["items"]][[i]][[3]]
}

#extract names of categories into a vector
gb_names <- character(gb_items)
for(i in seq_along(gb_list)){
  gb_names[i] <- gb_cat_name[["items"]][[i]][[4]][[1]]
}

#join dataframe with category name and drop categoryId
gb_cat <- as.data.frame(cbind(gb_ids,gb_names))
gb_cat[nrow(gb_cat)+1,]=c(29,"Nonprofits & Activism")
GB <- GB %>%
  left_join(gb_cat, by=c("categoryId"="gb_ids")) %>%
  dplyr::rename("category"="gb_names") %>%
  select(-categoryId)
```

``` r
#Match Category names to Category IDs from CA_category_id.json file

#extract ids of categories into a vector
ca_list <- ca_cat_name[["items"]]
ca_items <- length(ca_list)

CA$categoryId <- as.character(CA$categoryId)
ca_ids <- numeric(ca_items)
for(i in seq_along(ca_list)){
  ca_ids[i] <- ca_cat_name[["items"]][[i]][[3]]
}

#extract names of categories into a vector
ca_names <- character(ca_items)
for(i in seq_along(ca_list)){
  ca_names[i] <- ca_cat_name[["items"]][[i]][[4]][[1]]
}

#join dataframe with category name and drop categoryId
ca_cat <- as.data.frame(cbind(ca_ids,ca_names))
ca_cat[nrow(ca_cat)+1,]=c(29,"Nonprofits & Activism")
CA <- CA %>%
  left_join(ca_cat, by=c("categoryId"="ca_ids")) %>%
  dplyr::rename("category"="ca_names") %>%
  select(-categoryId)
```

``` r
#Match Category names to Category IDs from IN_category_id.json file

#extract ids of categories into a vector
in_list <- in_cat_name[["items"]]
in_items <- length(in_list)

IN$categoryId <- as.character(IN$categoryId)
in_ids <- numeric(in_items)
for(i in seq_along(in_list)){
  in_ids[i] <- in_cat_name[["items"]][[i]][[3]]
}

#extract names of categories into a vector
in_names <- character(in_items)
for(i in seq_along(in_list)){
  in_names[i] <- in_cat_name[["items"]][[i]][[4]][[1]]
}

#join dataframe with category name and drop categoryId
in_cat <- as.data.frame(cbind(in_ids,in_names))
in_cat[nrow(in_cat)+1,]=c(29,"Nonprofits & Activism")
IN <- IN %>%
  left_join(in_cat, by=c("categoryId"="in_ids")) %>%
  dplyr::rename("category"="in_names") %>%
  select(-categoryId)
```

``` r
#Add a column indicating if the trending video is created by the top 200 youtubers
US$top200youtuber <- FALSE
US[which(US$channelTitle %in% youtuber$Channel.Name),]$top200youtuber <- TRUE

GB$top200youtuber <- FALSE
GB[which(GB$channelTitle %in% youtuber$Channel.Name),]$top200youtuber <- TRUE

CA$top200youtuber <- FALSE
CA[which(CA$channelTitle %in% youtuber$Channel.Name),]$top200youtuber <- TRUE

IN$top200youtuber <- FALSE
IN[which(IN$channelTitle %in% youtuber$Channel.Name),]$top200youtuber <- TRUE
```

``` r
#union 4 datasets toether
ALL = rbind(CA,US,GB,IN)
```

``` r
#Distinct data, delete duplicated titles which stayed on the trending list for days
ALL <- ALL %>%
  group_by(title,publishedAt,channelTitle,category,country,tags,comments_disabled,top200youtuber) %>%
  distinct() %>%
  summarize(trendingtimes = n(),
            likes=max(likes),
            dislikes=max(dislikes),
            view_count=max(view_count),
            comment_count=max(comment_count))
```

    ## `summarise()` has grouped output by 'title', 'publishedAt', 'channelTitle',
    ## 'category', 'country', 'tags', 'comments_disabled'. You can override using the
    ## `.groups` argument.

``` r
# convert comments_disabled true/false into 1/0
ALL <- ALL %>% 
  mutate(comments_disabled, disabled = ifelse(comments_disabled=="True", 1, 0))

head(ALL)
```

    ## # A tibble: 6 × 14
    ## # Groups:   title, publishedAt, channelTitle, category, country, tags,
    ## #   comments_disabled [6]
    ##   title publishedAt         channelTitle category country tags  comments_disabl…
    ##   <chr> <dttm>              <chr>        <chr>    <chr>   <chr> <chr>           
    ## 1 "​ @S… 2022-10-09 10:32:54 Infinitum N… Enterta… IN      shan… False           
    ## 2 "​​​ 🔥… 2021-11-06 04:21:40 bxrry shorts Gaming   CA      shor… False           
    ## 3 "​​​ 🔥… 2021-11-06 04:21:40 bxrry shorts Gaming   US      shor… False           
    ## 4 "​​​ 🔥… 2022-03-08 21:42:07 bxrry shorts Gaming   CA      shor… False           
    ## 5 "​​​ 🔥… 2022-03-08 21:42:07 bxrry shorts Gaming   US      shor… False           
    ## 6 " 20… 2021-08-20 11:29:53 Ponmutta Me… Enterta… IN      ponm… False           
    ## # … with 7 more variables: top200youtuber <lgl>, trendingtimes <int>,
    ## #   likes <int>, dislikes <int>, view_count <int>, comment_count <int>,
    ## #   disabled <dbl>

# Key topic 2: Viewers’ negative sentiment by their countries & videos categories

The first interesting topic we want to discover is whether the YouTube
viewers negative sentiment are different across countries and videos
categories. In our datasets, there are two variables that are associated
with controversial opinions and negative sentiment. One is `disklie`,
which stores the count of dislikes for each video. The other is
`comments_disabled`, which indicates whether a video allow its viewer to
leave a comment. To better expose the viewers attitudes towards the
videos, we define the metric of dislike percentage as the number of
dislikes multiply by the sum of the number of dislikes and likes. After
creating this metric, we also group our dataset by category and country,
to summarize the average dislike percentage and comments-disabled
percentage for each category and country.

``` r
# dislike percentage 
ALL$dislikePerc <- 0
ALL[which(ALL$likes!=0),] <- ALL[which(ALL$likes!=0),] %>%
  mutate(dislikePerc = dislikes/(likes+dislikes))
    
# group by category 
groupedVideos <- ALL %>%
              group_by(category) %>%
              summarize("dislikePercMean" = mean(dislikePerc), 
                        "disabledPercMean" = mean(disabled))

# group by category and country
groupedVideos2 <- ALL %>%
              group_by(category,country) %>%
              summarize("Dislike Percentage" = mean(dislikePerc), 
                        "Comments-disabled Percentage" = mean(disabled))
```

    ## `summarise()` has grouped output by 'category'. You can override using the
    ## `.groups` argument.

Next, we want to visualize the grouped dataset we just created to unlock
insight of how negative sentiment vary across video categories and
trending list countries.

``` r
# create color palette
coul <- brewer.pal(4, "Reds") 
coul <- colorRampPalette(coul)(15)
coul <- rev(coul)

# plot "Dislike Percentage by Category and Country"
ggplot(groupedVideos, aes(dislikePercMean, reorder(category, dislikePercMean), fill = category, label=percent(dislikePercMean,accuracy=0.1))) +
  geom_bar(col = "black", stat = "identity") +
  scale_fill_manual(values=coul) +
  #geom_line(aes(viewMean, reorder(category, dislikePercMean)), group = 1, lwd = 1, lty = 3, col="grey") +
  labs(y = "Category",
       x = "Dislike Percentage",
       title='dislike% = # of dislike/(# of dislike + # of like)',
       subtitle='News&Politics has the highest dislike percentage') +
  geom_text(hjust = -0.02, size=2.5) +
  theme_classic()
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

``` r
#ggsave("1.png",width = 8,height = 5)

# plot "Disabled-Comments Percentage by Category and Country"
ggplot(groupedVideos, aes(disabledPercMean, reorder(category, disabledPercMean), fill = category, label=percent(disabledPercMean,accuracy=0.1))) + 
  geom_bar(col = "black", stat = "identity") +
  scale_fill_manual(values=coul) +
  labs(y = "Category",
       x = "Disabled-Comments Percentage",
       title='comment-disabled% = comment-disabled videos/total videos',
       subtitle='News&Politics has the highest comment-disabled percentage') +
  geom_text(hjust = -0.02, size=2.5) +
  theme_classic()
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-29-2.png)<!-- -->

``` r
#ggsave("2.png",width = 8,height = 5)


#country difference
subset <- groupedVideos2 %>%
  filter(category == c("News & Politics")) %>%
  gather(metric, value, "Dislike Percentage":"Comments-disabled Percentage" )


ggplot(subset, aes(country, value, fill = metric, label=percent(value,accuracy=0.1))) + 
  geom_bar(position="dodge", col = "black", stat = "identity") +
  scale_fill_manual(values=c("#A52A2A","#FCBFA8")) +
  geom_text(position = position_dodge2(width = 0.9),hjust = 0.45,vjust=-0.5, size=3) +
  labs(y = "",
       x = "",
       subtitle="Compared with western countriesm, India seems to have a looser news control environment")+
  theme_classic() 
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-29-3.png)<!-- -->

``` r
#ggsave("3.pdf",width = 8,height = 6)
```

Based on the above pictures, we found that News&Politics is the category
where negative sentiment dwell most and news control environment differ
from each country. Thus, we decided to dive deeper into the category of
News&Politics to explore the different attitudes News&Politics in
different country.

``` r
#define a function which can generate a wordcloud

freqWord = function(df,categ,country,remove) {
docs = Corpus(VectorSource(df[(df$category == categ)&(df$country==country),]$tags))

toSpace <- content_transformer(function (x , pattern ) gsub(pattern, " ", x))
docs <- tm_map(docs, toSpace, "/")
docs <- tm_map(docs, toSpace, "@")
docs <- tm_map(docs, toSpace, "\\|")
# Convert the text to lower case
docs <- tm_map(docs, content_transformer(tolower))
# Remove numbers
docs <- tm_map(docs, removeNumbers)
# Remove english common stopwords
docs <- tm_map(docs, removeWords, stopwords("english"))
docs <- tm_map(docs, removeWords, remove)
# Remove your own stop word
# specify your stopwords as a character vector
# docs <- tm_map(docs, removeWords, c("des", "les", "pas", "de", "est", "plus", "vos")) 
# Remove punctuations
docs <- tm_map(docs, removePunctuation)
# Eliminate extra white spaces
docs <- tm_map(docs, stripWhitespace)
# Text stemming
# docs <- tm_map(docs, stemDocument)
dtm <- TermDocumentMatrix(docs)
m <- as.matrix(dtm)
v <- sort(rowSums(m),decreasing=TRUE)
d <- data.frame(word = names(v),freq=v)

wordcloud(words = d$word, freq = d$freq,min.freq = 20,
          max.words=100, random.order=FALSE, rot.per=0.35, 
          scale=c(3.2,0.4),
          colors=c("#282828","#FF0000"))
#wordcloud2(d,color="#FF0000",
           #backgroundColor = "#282828")
}
```

``` r
negative <- ALL %>%
  filter(dislikePerc > 0.10)
freqWord(negative,'News & Politics','US',c('news','today'))
```

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "/"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "@"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "\\|"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, content_transformer(tolower)):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeNumbers): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, stopwords("english")):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, remove): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removePunctuation): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, stripWhitespace): transformation drops
    ## documents

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

The most controversial topic in the US is about election

``` r
freqWord(negative,'News & Politics','CA',c('news','today'))
```

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "/"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "@"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "\\|"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, content_transformer(tolower)):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeNumbers): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, stopwords("english")):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, remove): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removePunctuation): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, stripWhitespace): transformation drops
    ## documents

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

The result of Canada is between US and GB.

``` r
freqWord(negative,'News & Politics','GB',c('news','today'))
```

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "/"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "@"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "\\|"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, content_transformer(tolower)):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeNumbers): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, stopwords("english")):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, remove): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removePunctuation): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, stripWhitespace): transformation drops
    ## documents

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->

The most controversial topic in GB is about coronavirus.

``` r
freqWord(negative,'News & Politics','IN',c('news','today','live'))
```

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "/"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "@"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "\\|"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, content_transformer(tolower)):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeNumbers): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, stopwords("english")):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, remove): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removePunctuation): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, stripWhitespace): transformation drops
    ## documents

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-34-1.png)<!-- -->

Indian did not care about western politics too much # Key topic 3:
Viewership Habits by Countries After exploring the different attitudes
towards News&Politics in different country. Our next key question is if
there are some common viewing habits existing on different countries. We
try to answer questions by exploring if there are some common videos
appearing on the trending list of different countries and what character
theses videos have.

## Overlapping number of videos

``` r
# Heatmap for Video Overlaps between 4 English Speaking Countries: India, United States, Canada & United Kingdom

countoverlap <- function(country_x, country_y) {
  overlap <- country_x %>%
    inner_join(country_y, by=c("title"="title")) %>%
    distinct(title, .keep_all=TRUE)
  n <- nrow(overlap)
  return(n)
}

countrylst <- list()
countrylst[[1]] <- US
countrylst[[2]] <- CA
countrylst[[3]] <- GB
countrylst[[4]] <- IN

M <- matrix(nrow = 4, ncol = 4)
#count <- df()
for (i in 1:4) {
  for (n in 1:4){
    count <- countoverlap(countrylst[[i]], countrylst[[n]])
    M[i,n] <- count
  }
}
countries <- c("US","CA", "GB", "IN")
count <- data.frame(matrix(ncol=3,nrow=0, dimnames=list(NULL, c("CountryX", "CountryY", "SharedVideos"))))
for (i in 1:4) {
  for (n in 1:4){
    c <- countoverlap(countrylst[[i]], countrylst[[n]])
    count[nrow(count)+1,] = c(countries[i], countries[n], c)
  }
}
M[1,1] = 0
M[2,2] = 0
M[3,3] = 0
M[4,4] = 0
rownames(M) <- c("US", "CA","GB","IN")
colnames(M) <- c("US", "CA","GB","IN")
M.1 <-melt(M)
```

    ## Warning in type.convert.default(X[[i]], ...): 'as.is' should be specified by the
    ## caller; using TRUE

    ## Warning in type.convert.default(X[[i]], ...): 'as.is' should be specified by the
    ## caller; using TRUE

``` r
ggplot(M.1, aes(x = X2, y = X1)) + 
  geom_raster(aes(fill=value)) + 
  theme_bw() + theme(axis.text.x=element_text(size=9, angle=0, vjust=0.3),
                     axis.text.y=element_text(size=9),
                     plot.title=element_text(size=11)) + scale_fill_gradient(low = "white", high = "#FF0000") +
  labs(x = "English Speaking Countries", y = "English Speaking Countries", title = "Videos Overlap") 
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->

Evidently, we are able to observe that US and CA has the most
overlapping videos, followed by GB while IN has the least overlapping
videos, which is quite intuitive due to cultural similarity and
geographic distance.

## Commonality -> category: music, game, entertainment

``` r
#Join data by title to find all videos which exist on the trending list of all 4 countries
shared <- US %>%
  inner_join(CA, by=c("title"="title")) %>%
  inner_join(GB, by=c("title"="title")) %>%
  inner_join(IN, by=c("title"="title")) %>%
  distinct(title, .keep_all=TRUE)  %>%
  select(1,3,4,5,6,7,8,9,10,11,12,13,14) %>%
  dplyr::rename( channelTitle=channelTitle.x,view_count=view_count.x,likes=likes.x, tags=tags.x,
         dislikes=dislikes.x,comment_count=comment_count.x, category=category.x, top200youtuber=top200youtuber.x)
```

``` r
#group overlapping videos by category and compute the percentage
group_cate <- shared %>%
  group_by(category) %>%
  summarize(percentage = n()/nrow(shared))

#plot the above result
ggplot(group_cate, aes(percentage, reorder(category, percentage), fill = category, label=percent(percentage,accuracy=0.1))) +
  geom_bar(col = "black", stat = "identity") +
  scale_fill_manual(values=coul) +
  #geom_line(aes(viewMean, reorder(category, dislikePercMean)), group = 1, lwd = 1, lty = 3, col="grey") +
  labs(y = "Category",
       x = "category percentage in overlapping videos",
       title='percentage=# of videos in the category/total videos',
       subtitle='Music, Gaming and Entertianment are top3') +
  geom_text(hjust = -0.02, size=2.5) +
  theme_classic()
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-37-1.png)<!-- -->

``` r
# group union videos by category and compute the percentage
group_cate <- ALL %>%
  group_by(category) %>%
  summarize(percentage = n()/nrow(ALL))

#plot the above result
ggplot(group_cate, aes(percentage, reorder(category, percentage), fill = category, label=percent(percentage,accuracy=0.1))) +
  geom_bar(col = "black", stat = "identity") +
  scale_fill_manual(values=coul) +
  #geom_line(aes(viewMean, reorder(category, dislikePercMean)), group = 1, lwd = 1, lty = 3, col="grey") +
  labs(y = "Category",
       x = "category percentage in union videos",
       title='percentage=# of videos in the category/total videos',
       subtitle='Music, Gaming and Entertianment are top3') +
  geom_text(hjust = -0.02, size=2.5) +
  theme_classic()
```

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-37-2.png)<!-- -->

From the above two pictures, we can see that Music, Entertainment and
Gaming are always top 3 no matter in overlapping videos or union videos,
but compared with the percentage in union videos, music accounts for a
greater percenate abput 28% in overlapping videos while only accounts
for 13% in union videos. Music is the most shared category across the
world. It might be because that music can arouse resonace more easily.

## wordcould of overlapping videos by category

``` r
freqWord1 = function(categ,remove) {
docs = Corpus(VectorSource(shared[shared$category == categ,]$tags))

toSpace <- content_transformer(function (x , pattern ) gsub(pattern, " ", x))
docs <- tm_map(docs, toSpace, "/")
docs <- tm_map(docs, toSpace, "@")
docs <- tm_map(docs, toSpace, "\\|")
# Convert the text to lower case
docs <- tm_map(docs, content_transformer(tolower))
# Remove numbers
docs <- tm_map(docs, removeNumbers)
# Remove english common stopwords
docs <- tm_map(docs, removeWords, stopwords("english"))
docs <- tm_map(docs, removeWords, remove)
# Remove your own stop word
# specify your stopwords as a character vector
# docs <- tm_map(docs, removeWords, c("des", "les", "pas", "de", "est", "plus", "vos")) 
# Remove punctuations
docs <- tm_map(docs, removePunctuation)
# Eliminate extra white spaces
docs <- tm_map(docs, stripWhitespace)
# Text stemming
# docs <- tm_map(docs, stemDocument)
dtm <- TermDocumentMatrix(docs)
m <- as.matrix(dtm)
v <- sort(rowSums(m),decreasing=TRUE)
d <- data.frame(word = names(v),freq=v)

wordcloud(words = d$word, freq = d$freq,min.freq = 10,
          max.words=100, random.order=FALSE, rot.per=0.35, 
          scale=c(3.2,0.4),
          colors=c("#282828","#FF0000"))
#wordcloud2(d,color="#FF0000",
           #backgroundColor = "#282828")
}
```

``` r
group_chan <- shared %>%
  group_by(channelTitle) %>%
  summarize(count = n()) %>%
  arrange(desc(count))


group_chan %>%
  left_join(shared, by=c("channelTitle"="channelTitle")) %>%
  filter(category=="Music") %>%
  select(channelTitle, count, top200youtuber) %>%
  distinct()
```

    ## # A tibble: 50 × 3
    ##    channelTitle      count top200youtuber
    ##    <chr>             <int> <lgl>         
    ##  1 Dude Perfect         40 TRUE          
    ##  2 BLACKPINK            32 TRUE          
    ##  3 BANGTANTV            26 TRUE          
    ##  4 HYBE LABELS          26 FALSE         
    ##  5 JYP Entertainment    23 FALSE         
    ##  6 SMTOWN               20 TRUE          
    ##  7 TaylorSwiftVEVO      19 TRUE          
    ##  8 Big Hit Labels       12 FALSE         
    ##  9 BillieEilishVEVO     10 FALSE         
    ## 10 JustinBieberVEVO     10 TRUE          
    ## # … with 40 more rows

``` r
#par(family="AppleGothic")
freqWord1("Music",c("official", "music","video","new","album","lyrics"))
```

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "/"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "@"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "\\|"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, content_transformer(tolower)):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeNumbers): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, stopwords("english")):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, remove): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removePunctuation): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, stripWhitespace): transformation drops
    ## documents

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-39-1.png)<!-- -->
We can see English pop music are appearing often on the overlapping
trending list, but surprisingly, there are many korean characters all of
which are about kpop. Kpop explosion breaks cultural barriers and has
become a global phenomenon. Their visual effects involving colourful
costumes and perfect dancing are conquering the world.

``` r
group_chan <- shared %>%
  group_by(channelTitle) %>%
  summarize(count = n()) %>%
  arrange(desc(count))


group_chan %>%
  left_join(shared, by=c("channelTitle"="channelTitle")) %>%
  filter(category=="Entertainment") %>%
  select(channelTitle, count, top200youtuber) %>%
  distinct()
```

    ## # A tibble: 43 × 3
    ##    channelTitle                count top200youtuber
    ##    <chr>                       <int> <lgl>         
    ##  1 MrBeast                        41 TRUE          
    ##  2 Marvel Entertainment           36 FALSE         
    ##  3 ZHC                            30 TRUE          
    ##  4 Warner Bros. Pictures          17 FALSE         
    ##  5 HBO Max                        14 FALSE         
    ##  6 Netflix                        14 TRUE          
    ##  7 Sony Pictures Entertainment     8 FALSE         
    ##  8 Logan Paul                      6 FALSE         
    ##  9 Justin Bieber                   5 TRUE          
    ## 10 OverSimplified                  5 FALSE         
    ## # … with 33 more rows

``` r
#par(family="AppleGothic")
freqWord1("Entertainment",c("official", "none","video","new","album","lyrics"))
```

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "/"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "@"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "\\|"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, content_transformer(tolower)):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeNumbers): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, stopwords("english")):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, remove): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removePunctuation): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, stripWhitespace): transformation drops
    ## documents

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-40-1.png)<!-- -->

There are some entertainment giants appeared in the wordcloud, like
Marvel, Netflix and so on. There are also some media
personalities,individual content creator whose views and subscribers are
comparable to entertainment giants.

``` r
group_chan <- shared %>%
  group_by(channelTitle) %>%
  summarize(count = n()) %>%
  arrange(desc(count))


group_chan %>%
  left_join(shared, by=c("channelTitle"="channelTitle")) %>%
  filter(category=="Entertainment") %>%
  select(channelTitle, count, top200youtuber) %>%
  distinct()
```

    ## # A tibble: 43 × 3
    ##    channelTitle                count top200youtuber
    ##    <chr>                       <int> <lgl>         
    ##  1 MrBeast                        41 TRUE          
    ##  2 Marvel Entertainment           36 FALSE         
    ##  3 ZHC                            30 TRUE          
    ##  4 Warner Bros. Pictures          17 FALSE         
    ##  5 HBO Max                        14 FALSE         
    ##  6 Netflix                        14 TRUE          
    ##  7 Sony Pictures Entertainment     8 FALSE         
    ##  8 Logan Paul                      6 FALSE         
    ##  9 Justin Bieber                   5 TRUE          
    ## 10 OverSimplified                  5 FALSE         
    ## # … with 33 more rows

``` r
#par(family="AppleGothic")
freqWord1("Gaming",c("game"))
```

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "/"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "@"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, toSpace, "\\|"): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, content_transformer(tolower)):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeNumbers): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, stopwords("english")):
    ## transformation drops documents

    ## Warning in tm_map.SimpleCorpus(docs, removeWords, remove): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, removePunctuation): transformation drops
    ## documents

    ## Warning in tm_map.SimpleCorpus(docs, stripWhitespace): transformation drops
    ## documents

![](TopYoutubersandTrendingVideos_Analysis_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->

As for game, it is surprising that it is clash of clans, a mobile game
about kingdom building, other than competitive PC games, bring the most
common video content on youtube.

# Conclusion, Limitation and Next steps

## Conlusions

Key Question 1: Engagement Difference by Youtubers’ Nationalities &
Categories User engagement(like/views, views/followers) is significantly
different grouped by YouTubers’ nationalities and categories.

Key Question 2: Viewers’ Negative Sentiment by their Countries and
Videos Categories Topics related to politics and covid-19 pandemic
exhibit a controversial manner . Western countries share a more common
viewership habit compared with India.

Key Question 3: Viewership Habits by English Speaking Countries Music,
Entertainment, & Gaming are categories enjoyed by all aforementioned
nations. KPop exhibits a rather surprisingly popular fashion in all four
nations.

## Limitations

Our sample only includes the most popular videos without any ordinary
videos so more comprehensive conclusions could not be drawn. We also
ignore the connection between YouTubers and their videos due to want of
more features and difficulty of linking two datasets.

## Next Steps

We would like to include more ordinary videos rather than those only
coming from the trending list We would like to explore relations between
YouTubers and their videos through additional features such as video
quality, shooting method, editing technique and etc. Natural Language
Processing & Deep Learning

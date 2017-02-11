# Following the 19th Korean presidential election with a Data Scientist



## 1. Hypothesis Test

One weekend, as I was reading ['Kang-jin diaries'](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791195823086&orderClick=JAI) by [Sohn Hak-kyu](https://namu.wiki/w/손학규), 
which he wrote when he returned to politics as the executive director of the [East Asia Future Foundation](http://www.future21.or.kr) last year, 
I got the inspiration to code this. This is the outcome of the R coding of the first weekend of 2017.

Is the talent of MLB pitchers inherited? 
Can a son outrun his father? Jumping to the conclusion, 
a son may or may not outrun his father. 
However, the talent of the son shows regression to the mean. 
In other words, the son of an outstanding father tends to be more average than his father, 
and the son of a not-so-talented father tends to outrun his father, as he skews towards average.

<img src="fig/pe-mlb-regression-eng.png" alt="Regression analysis overview" width="77%" />

## 2. Data

I got a list of families that have three consecutive generations of Major League pitchers from Wikipedia, 
and gathered the records of pitchers’ performances from Lahman’s Baseball Database. 
The hypothesis was verified based on the records and ERA.

- [Wikipedia, List of second-generation Major League Baseball players](https://en.wikipedia.org/wiki/List_of_second-generation_Major_League_Baseball_players) : 46 families in total
- [Lahman](http://seanlahman.com/) :Lahman’s Baseball Database: Records of innings, points lost, ERA, etc.

## 3. R Code


### 3.1. Setting it up

One can notice that Lahman’s database is easy to access via R package.


~~~{.r}
# 0. Configuration ----------------------------------------
library(ggplot2)
library(tidyverse)
library(Lahman)
library(stringr)
library(ggthemes)
library(extrafont)
library(gridExtra)
library(ggrepel)
library(DT)
library(rvest)
library(httr)
loadfonts()
~~~

### 3.2. Import Data - Wikipedia

First, get the list of fathers and sons who were baseball players. 
Thankfully, Wikipedia has a neat list, which on can use for analysis. 
Use specifically fathers-and-sons who were both pitchers.


~~~{.r}
url <- "https://en.wikipedia.org/wiki/List_of_second-generation_Major_League_Baseball_players" 

player <- url %>%
  read_html() %>%
  html_nodes(xpath='//*[@id="mw-content-text"]/table[2]') %>%
  html_table(fill = TRUE) %>% 
  .[[1]]

names(player) <- c("father", "f_posi", "son", "son_posi")

player_pitcher <- player %>% dplyr::filter(f_posi =="Pitcher" & son_posi =="Pitcher") %>% 
                     mutate(father = str_replace_all(father, "\\*", ''),
                            son = str_replace_all(son, "\\*", '')) %>% 
                     mutate(father = str_replace_all(father, ",", ''),
                            son = str_replace_all(son, ",", '')) %>% 
                     mutate(father = str_replace_all(father, "†", ''),
                            son = str_replace_all(son, "†", '')) %>% 
                     mutate(family_key = paste0("fam", seq(1, 46,1)))
datatable(player_pitcher)                     
~~~

<!--html_preserve--><div id="htmlwidget-b809f018f0199aa090de" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-b809f018f0199aa090de">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46"],["Mike Bacsik","Jim Bagby Sr.","Floyd Bannister","Steve Bedrosian","Dave Bennett","Pedro Borbón","Joe Coleman","Joe Coleman","Mardie Cornejo","Bill Crouch","Doug Drabek","Dick Ellsworth","Steve Grilli","Ross Grimsley","Clarence Heise","Pat Jacquez","Dave Johnson","Rankin Johnson Sr.","Lew Krausse Sr.","Joe Landrum","Bill Laxton","Thornton Lee","Glenn Liebhardt Sr.","Jack Lively","Willard Mains","Jim McAndrew","Lance McCullers","Willie Mills","René Monteagudo","Julio Navarro","Chet Nichols Sr.","John O'Donoghue","Diomedes Olivo","Jay Pettibone","Herman Pillette","Fred Rath Sr.","Walt Ripley","Bruce Ruffin","Jeff Russell","Mel Stottlemyre","Mel Stottlemyre","Dizzy Trout","Ed Walsh","Hank Webb","Joe Wood","Clyde Wright"],["Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher"],["Mike Bacsik","Jim Bagby Jr.","Brian Bannister","Cam Bedrosian","Erik Bennett","Pedro Borbón Jr.","Joe Coleman","Casey Coleman","Nate Cornejo","Bill Crouch","Kyle Drabek","Steve Ellsworth","Jason Grilli","Ross Grimsley","Jim Heise","Tom Jacquez","Steve Johnson","Rankin Johnson Jr.","Lew Krausse Jr.","Bill Landrum","Brett Laxton","Don Lee","Glenn Liebhardt Jr.","Buddy Lively","Jim Mains","Jamie McAndrew","Lance McCullers Jr.","Art Mills","Aurelio Monteagudo","Jaime Navarro","Chet Nichols Jr.","John O'Donoghue","Gilberto Rondón","Jonathan Pettibone","Duane Pillette","Fred Rath Jr.","Allen Ripley","Chance Ruffin","James Russell","Mel Stottlemyre Jr.","Todd Stottlemyre","Steve Trout","Ed Walsh Jr.","Ryan Webb","Joe Wood","Jaret Wright"],["Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher","Pitcher"],["fam1","fam2","fam3","fam4","fam5","fam6","fam7","fam8","fam9","fam10","fam11","fam12","fam13","fam14","fam15","fam16","fam17","fam18","fam19","fam20","fam21","fam22","fam23","fam24","fam25","fam26","fam27","fam28","fam29","fam30","fam31","fam32","fam33","fam34","fam35","fam36","fam37","fam38","fam39","fam40","fam41","fam42","fam43","fam44","fam45","fam46"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>father\u003c/th>\n      <th>f_posi\u003c/th>\n      <th>son\u003c/th>\n      <th>son_posi\u003c/th>\n      <th>family_key\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"order":[],"autoWidth":false,"orderClasses":false,"columnDefs":[{"orderable":false,"targets":0}]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

To join the data with Lahman’s database, 
use nameLast and nameFirst as the keys. 
Therefore, one needs to separate the first and last names and convert the **wide** data format to **long** data format.



~~~{.r}
player_f_df <- player_pitcher %>% dplyr::select(pitcher_name=father, family_key) %>% 
  mutate(family="father")
player_s_df <- player_pitcher %>% dplyr::select(pitcher_name=son, family_key) %>% 
  mutate(family="son")

player_fs_df <- bind_rows(player_f_df, player_s_df)

player_fs_df <- player_fs_df %>% 
                  tidyr::separate(pitcher_name, into=c("nameFirst", "nameLast"), sep=" ", remove = FALSE)

datatable(player_fs_df)
~~~

<!--html_preserve--><div id="htmlwidget-6fd6117f89ce30eb6221" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-6fd6117f89ce30eb6221">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92"],["Mike Bacsik","Jim Bagby Sr.","Floyd Bannister","Steve Bedrosian","Dave Bennett","Pedro Borbón","Joe Coleman","Joe Coleman","Mardie Cornejo","Bill Crouch","Doug Drabek","Dick Ellsworth","Steve Grilli","Ross Grimsley","Clarence Heise","Pat Jacquez","Dave Johnson","Rankin Johnson Sr.","Lew Krausse Sr.","Joe Landrum","Bill Laxton","Thornton Lee","Glenn Liebhardt Sr.","Jack Lively","Willard Mains","Jim McAndrew","Lance McCullers","Willie Mills","René Monteagudo","Julio Navarro","Chet Nichols Sr.","John O'Donoghue","Diomedes Olivo","Jay Pettibone","Herman Pillette","Fred Rath Sr.","Walt Ripley","Bruce Ruffin","Jeff Russell","Mel Stottlemyre","Mel Stottlemyre","Dizzy Trout","Ed Walsh","Hank Webb","Joe Wood","Clyde Wright","Mike Bacsik","Jim Bagby Jr.","Brian Bannister","Cam Bedrosian","Erik Bennett","Pedro Borbón Jr.","Joe Coleman","Casey Coleman","Nate Cornejo","Bill Crouch","Kyle Drabek","Steve Ellsworth","Jason Grilli","Ross Grimsley","Jim Heise","Tom Jacquez","Steve Johnson","Rankin Johnson Jr.","Lew Krausse Jr.","Bill Landrum","Brett Laxton","Don Lee","Glenn Liebhardt Jr.","Buddy Lively","Jim Mains","Jamie McAndrew","Lance McCullers Jr.","Art Mills","Aurelio Monteagudo","Jaime Navarro","Chet Nichols Jr.","John O'Donoghue","Gilberto Rondón","Jonathan Pettibone","Duane Pillette","Fred Rath Jr.","Allen Ripley","Chance Ruffin","James Russell","Mel Stottlemyre Jr.","Todd Stottlemyre","Steve Trout","Ed Walsh Jr.","Ryan Webb","Joe Wood","Jaret Wright"],["Mike","Jim","Floyd","Steve","Dave","Pedro","Joe","Joe","Mardie","Bill","Doug","Dick","Steve","Ross","Clarence","Pat","Dave","Rankin","Lew","Joe","Bill","Thornton","Glenn","Jack","Willard","Jim","Lance","Willie","René","Julio","Chet","John","Diomedes","Jay","Herman","Fred","Walt","Bruce","Jeff","Mel","Mel","Dizzy","Ed","Hank","Joe","Clyde","Mike","Jim","Brian","Cam","Erik","Pedro","Joe","Casey","Nate","Bill","Kyle","Steve","Jason","Ross","Jim","Tom","Steve","Rankin","Lew","Bill","Brett","Don","Glenn","Buddy","Jim","Jamie","Lance","Art","Aurelio","Jaime","Chet","John","Gilberto","Jonathan","Duane","Fred","Allen","Chance","James","Mel","Todd","Steve","Ed","Ryan","Joe","Jaret"],["Bacsik","Bagby","Bannister","Bedrosian","Bennett","Borbón","Coleman","Coleman","Cornejo","Crouch","Drabek","Ellsworth","Grilli","Grimsley","Heise","Jacquez","Johnson","Johnson","Krausse","Landrum","Laxton","Lee","Liebhardt","Lively","Mains","McAndrew","McCullers","Mills","Monteagudo","Navarro","Nichols","O'Donoghue","Olivo","Pettibone","Pillette","Rath","Ripley","Ruffin","Russell","Stottlemyre","Stottlemyre","Trout","Walsh","Webb","Wood","Wright","Bacsik","Bagby","Bannister","Bedrosian","Bennett","Borbón","Coleman","Coleman","Cornejo","Crouch","Drabek","Ellsworth","Grilli","Grimsley","Heise","Jacquez","Johnson","Johnson","Krausse","Landrum","Laxton","Lee","Liebhardt","Lively","Mains","McAndrew","McCullers","Mills","Monteagudo","Navarro","Nichols","O'Donoghue","Rondón","Pettibone","Pillette","Rath","Ripley","Ruffin","Russell","Stottlemyre","Stottlemyre","Trout","Walsh","Webb","Wood","Wright"],["fam1","fam2","fam3","fam4","fam5","fam6","fam7","fam8","fam9","fam10","fam11","fam12","fam13","fam14","fam15","fam16","fam17","fam18","fam19","fam20","fam21","fam22","fam23","fam24","fam25","fam26","fam27","fam28","fam29","fam30","fam31","fam32","fam33","fam34","fam35","fam36","fam37","fam38","fam39","fam40","fam41","fam42","fam43","fam44","fam45","fam46","fam1","fam2","fam3","fam4","fam5","fam6","fam7","fam8","fam9","fam10","fam11","fam12","fam13","fam14","fam15","fam16","fam17","fam18","fam19","fam20","fam21","fam22","fam23","fam24","fam25","fam26","fam27","fam28","fam29","fam30","fam31","fam32","fam33","fam34","fam35","fam36","fam37","fam38","fam39","fam40","fam41","fam42","fam43","fam44","fam45","fam46"],["father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>pitcher_name\u003c/th>\n      <th>nameFirst\u003c/th>\n      <th>nameLast\u003c/th>\n      <th>family_key\u003c/th>\n      <th>family\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"order":[],"autoWidth":false,"orderClasses":false,"columnDefs":[{"orderable":false,"targets":0}]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### 3.3. Merging the names and Lahman performance records

#### 3.3.1. Appending playerID to the pitcher name list

There were many overlapping values among the first and last names due to them containing special characters, 
fathers and sons sharing the same family names, and often having common first names. 
Through data cleansing works, I gave the overlapping names unique playerIDs. 
In fact, I spent the whole weekend writing this code.


~~~{.r}
player_db <- playerInfo("")
player_m <- left_join(player_fs_df, player_db, by=c("nameFirst", "nameLast"))

player_m_f <- player_m %>% dplyr::filter(family=="father") %>% group_by(family, nameFirst, nameLast, pitcher_name, family_key) %>% 
  summarise(playerID = first(playerID)) %>% 
  mutate(playerID = ifelse(nameLast == "Monteagudo" & family=="father", "montere01", playerID),
         playerID = ifelse(nameFirst == "Pedro" & family=="father", "borbope01", playerID),
         playerID = ifelse(nameFirst == "Pedro" & family=="son", "borbope02", playerID))
         

player_m_s <- player_m %>% dplyr::filter(family=="son") %>% group_by(family, nameFirst, nameLast, pitcher_name, family_key) %>% 
  summarise(playerID = last(playerID)) %>% 
  mutate(playerID = ifelse(nameFirst == "Pedro" & family=="son", "borbope02", playerID),
         playerID = ifelse(nameFirst == "Tom" & family=="son", "jacquto01", playerID),
         playerID = ifelse(nameFirst == "Gilberto" & family=="son", "rondogi01", playerID))

# merge father and son
player_m_list <- bind_rows(player_m_f, player_m_s)

datatable(player_m_list)
~~~

<!--html_preserve--><div id="htmlwidget-d2ffbaa0d855063ed66d" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-d2ffbaa0d855063ed66d">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92"],["father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son"],["Bill","Bill","Bruce","Chet","Clarence","Clyde","Dave","Dave","Dick","Diomedes","Dizzy","Doug","Ed","Floyd","Fred","Glenn","Hank","Herman","Jack","Jay","Jeff","Jim","Jim","Joe","Joe","Joe","Joe","John","Julio","Lance","Lew","Mardie","Mel","Mel","Mike","Pat","Pedro","Rankin","René","Ross","Steve","Steve","Thornton","Walt","Willard","Willie","Allen","Art","Aurelio","Bill","Bill","Brett","Brian","Buddy","Cam","Casey","Chance","Chet","Don","Duane","Ed","Erik","Fred","Gilberto","Glenn","Jaime","James","Jamie","Jaret","Jason","Jim","Jim","Jim","Joe","Joe","John","Jonathan","Kyle","Lance","Lew","Mel","Mike","Nate","Pedro","Rankin","Ross","Ryan","Steve","Steve","Steve","Todd","Tom"],["Crouch","Laxton","Ruffin","Nichols","Heise","Wright","Bennett","Johnson","Ellsworth","Olivo","Trout","Drabek","Walsh","Bannister","Rath","Liebhardt","Webb","Pillette","Lively","Pettibone","Russell","Bagby","McAndrew","Coleman","Coleman","Landrum","Wood","O'Donoghue","Navarro","McCullers","Krausse","Cornejo","Stottlemyre","Stottlemyre","Bacsik","Jacquez","Borbón","Johnson","Monteagudo","Grimsley","Bedrosian","Grilli","Lee","Ripley","Mains","Mills","Ripley","Mills","Monteagudo","Crouch","Landrum","Laxton","Bannister","Lively","Bedrosian","Coleman","Ruffin","Nichols","Lee","Pillette","Walsh","Bennett","Rath","Rondón","Liebhardt","Navarro","Russell","McAndrew","Wright","Grilli","Bagby","Heise","Mains","Coleman","Wood","O'Donoghue","Pettibone","Drabek","McCullers","Krausse","Stottlemyre","Bacsik","Cornejo","Borbón","Johnson","Grimsley","Webb","Ellsworth","Johnson","Trout","Stottlemyre","Jacquez"],["Bill Crouch","Bill Laxton","Bruce Ruffin","Chet Nichols Sr.","Clarence Heise","Clyde Wright","Dave Bennett","Dave Johnson","Dick Ellsworth","Diomedes Olivo","Dizzy Trout","Doug Drabek","Ed Walsh","Floyd Bannister","Fred Rath Sr.","Glenn Liebhardt Sr.","Hank Webb","Herman Pillette","Jack Lively","Jay Pettibone","Jeff Russell","Jim Bagby Sr.","Jim McAndrew","Joe Coleman","Joe Coleman","Joe Landrum","Joe Wood","John O'Donoghue","Julio Navarro","Lance McCullers","Lew Krausse Sr.","Mardie Cornejo","Mel Stottlemyre","Mel Stottlemyre","Mike Bacsik","Pat Jacquez","Pedro Borbón","Rankin Johnson Sr.","René Monteagudo","Ross Grimsley","Steve Bedrosian","Steve Grilli","Thornton Lee","Walt Ripley","Willard Mains","Willie Mills","Allen Ripley","Art Mills","Aurelio Monteagudo","Bill Crouch","Bill Landrum","Brett Laxton","Brian Bannister","Buddy Lively","Cam Bedrosian","Casey Coleman","Chance Ruffin","Chet Nichols Jr.","Don Lee","Duane Pillette","Ed Walsh Jr.","Erik Bennett","Fred Rath Jr.","Gilberto Rondón","Glenn Liebhardt Jr.","Jaime Navarro","James Russell","Jamie McAndrew","Jaret Wright","Jason Grilli","Jim Bagby Jr.","Jim Heise","Jim Mains","Joe Coleman","Joe Wood","John O'Donoghue","Jonathan Pettibone","Kyle Drabek","Lance McCullers Jr.","Lew Krausse Jr.","Mel Stottlemyre Jr.","Mike Bacsik","Nate Cornejo","Pedro Borbón Jr.","Rankin Johnson Jr.","Ross Grimsley","Ryan Webb","Steve Ellsworth","Steve Johnson","Steve Trout","Todd Stottlemyre","Tom Jacquez"],["fam10","fam21","fam38","fam31","fam15","fam46","fam5","fam17","fam12","fam33","fam42","fam11","fam43","fam3","fam36","fam23","fam44","fam35","fam24","fam34","fam39","fam2","fam26","fam7","fam8","fam20","fam45","fam32","fam30","fam27","fam19","fam9","fam40","fam41","fam1","fam16","fam6","fam18","fam29","fam14","fam4","fam13","fam22","fam37","fam25","fam28","fam37","fam28","fam29","fam10","fam20","fam21","fam3","fam24","fam4","fam8","fam38","fam31","fam22","fam35","fam43","fam5","fam36","fam33","fam23","fam30","fam39","fam26","fam46","fam13","fam2","fam15","fam25","fam7","fam45","fam32","fam34","fam11","fam27","fam19","fam40","fam1","fam9","fam6","fam18","fam14","fam44","fam12","fam17","fam42","fam41","fam16"],["croucbi01","laxtobi01","ruffibr01","nichoch01","heisecl01","wrighcl01","benneda01","johnsda03","ellswdi01","olivodi01","troutdi01","drabedo01","walshed01","bannifl01","rathfr01","liebhgl01","webbha01","pillehe01","livelja01","pettija01","russeje01","bagbyji01","mcandji01","colemjo04","colemjo04","landrjo01","woodjo03","odonojo01","navarju01","mcculla01","krausle01","cornema01","stottme01","stottme01","bacsimi01","jacqupa01","borbope01","johnsra01","montere01","grimsro01","bedrost01","grillst01","leeth01","riplewa01","mainswi01","millswi01","ripleal01","millsar01","monteau01","croucbi02","landrbi01","laxtobr01","bannibr01","livelbu01","bedroca01","colemca01","ruffich01","nichoch02","leedo01","pilledu01","walshed02","benneer01","rathfr02","rondogi01","liebhgl02","navarja01","russeja02","mcandja01","wrighja02","grillja01","bagbyji02","heiseji01","mainsji01","colemjo05","woodjo04","odonojo02","pettijo02","drabeky01","mcculla02","krausle02","stottme02","bacsimi02","cornena01","borbope02","johnsra02","grimsro02","webbry01","ellswst01","johnsst02","troutst01","stottto01","jacquto01"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>family\u003c/th>\n      <th>nameFirst\u003c/th>\n      <th>nameLast\u003c/th>\n      <th>pitcher_name\u003c/th>\n      <th>family_key\u003c/th>\n      <th>playerID\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"order":[],"autoWidth":false,"orderClasses":false,"columnDefs":[{"orderable":false,"targets":0}]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

#### 3.3.2. Merging the pitcher names and performance record

The process of combining the father and sons’ names(player_m_list) and 
their records(Pitching) can be summarized in one line; the key is playerID. 
The ERA is the most important indicator for a pitcher’s performance. 
The formula for calculating the ERA for each pitchers’ entire career is as follows:

$$\frac{\text{ER}}{\frac{\text{IPouts}}{3}} \times 9$$

- ERA(Earned Run Average): The average of the points the pitcher lost per game
- ER(Earned Run): A run that scores against a pitcher without an error or passed ball
- IPouts(Outs Pitched): Innings pitched \(\times\) 3.


~~~{.r}
player_pitch <- left_join(player_m_list, Pitching, by="playerID")

player_pitch_father <- player_pitch %>% group_by(playerID, nameFirst, nameLast, family, family_key) %>% 
  summarise(ERA = sum(ER, na.rm=TRUE)/(sum(IPouts, na.rm=TRUE)/3)*9) %>% 
  dplyr::filter(family =="father")

player_pitch_son <- player_pitch %>% group_by(playerID, nameFirst, nameLast, family, family_key) %>% 
  summarise(ERA = sum(ER, na.rm=TRUE)/(sum(IPouts, na.rm=TRUE)/3)*9) %>% 
  dplyr::filter(family =="son")

player_pitch_m <- bind_rows(player_pitch_father, player_pitch_son)

datatable(player_pitch_m) %>% 
  formatRound("ERA", digits = 2)
~~~

<!--html_preserve--><div id="htmlwidget-014bb501c44c8e776ef9" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-014bb501c44c8e776ef9">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92"],["bacsimi01","bagbyji01","bannifl01","bedrost01","benneda01","borbope01","colemjo04","colemjo04","cornema01","croucbi01","drabedo01","ellswdi01","grillst01","grimsro01","heisecl01","jacqupa01","johnsda03","johnsra01","krausle01","landrjo01","laxtobi01","leeth01","liebhgl01","livelja01","mainswi01","mcandji01","mcculla01","millswi01","montere01","navarju01","nichoch01","odonojo01","olivodi01","pettija01","pillehe01","rathfr01","riplewa01","ruffibr01","russeje01","stottme01","stottme01","troutdi01","walshed01","webbha01","woodjo03","wrighcl01","bacsimi02","bagbyji02","bannibr01","bedroca01","benneer01","borbope02","colemca01","colemjo05","cornena01","croucbi02","drabeky01","ellswst01","grillja01","grimsro02","heiseji01","jacquto01","johnsra02","johnsst02","krausle02","landrbi01","laxtobr01","leedo01","liebhgl02","livelbu01","mainsji01","mcandja01","mcculla02","millsar01","monteau01","navarja01","nichoch02","odonojo02","pettijo02","pilledu01","rathfr02","ripleal01","rondogi01","ruffich01","russeja02","stottme02","stottto01","troutst01","walshed02","webbry01","woodjo04","wrighja02"],["Mike","Jim","Floyd","Steve","Dave","Pedro","Joe","Joe","Mardie","Bill","Doug","Dick","Steve","Ross","Clarence","Pat","Dave","Rankin","Lew","Joe","Bill","Thornton","Glenn","Jack","Willard","Jim","Lance","Willie","René","Julio","Chet","John","Diomedes","Jay","Herman","Fred","Walt","Bruce","Jeff","Mel","Mel","Dizzy","Ed","Hank","Joe","Clyde","Mike","Jim","Brian","Cam","Erik","Pedro","Casey","Joe","Nate","Bill","Kyle","Steve","Jason","Ross","Jim","Tom","Rankin","Steve","Lew","Bill","Brett","Don","Glenn","Buddy","Jim","Jamie","Lance","Art","Aurelio","Jaime","Chet","John","Jonathan","Duane","Fred","Allen","Gilberto","Chance","James","Mel","Todd","Steve","Ed","Ryan","Joe","Jaret"],["Bacsik","Bagby","Bannister","Bedrosian","Bennett","Borbón","Coleman","Coleman","Cornejo","Crouch","Drabek","Ellsworth","Grilli","Grimsley","Heise","Jacquez","Johnson","Johnson","Krausse","Landrum","Laxton","Lee","Liebhardt","Lively","Mains","McAndrew","McCullers","Mills","Monteagudo","Navarro","Nichols","O'Donoghue","Olivo","Pettibone","Pillette","Rath","Ripley","Ruffin","Russell","Stottlemyre","Stottlemyre","Trout","Walsh","Webb","Wood","Wright","Bacsik","Bagby","Bannister","Bedrosian","Bennett","Borbón","Coleman","Coleman","Cornejo","Crouch","Drabek","Ellsworth","Grilli","Grimsley","Heise","Jacquez","Johnson","Johnson","Krausse","Landrum","Laxton","Lee","Liebhardt","Lively","Mains","McAndrew","McCullers","Mills","Monteagudo","Navarro","Nichols","O'Donoghue","Pettibone","Pillette","Rath","Ripley","Rondón","Ruffin","Russell","Stottlemyre","Stottlemyre","Trout","Walsh","Webb","Wood","Wright"],["father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","father","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son","son"],["fam1","fam2","fam3","fam4","fam5","fam6","fam7","fam8","fam9","fam10","fam11","fam12","fam13","fam14","fam15","fam16","fam17","fam18","fam19","fam20","fam21","fam22","fam23","fam24","fam25","fam26","fam27","fam28","fam29","fam30","fam31","fam32","fam33","fam34","fam35","fam36","fam37","fam38","fam39","fam40","fam41","fam42","fam43","fam44","fam45","fam46","fam1","fam2","fam3","fam4","fam5","fam6","fam8","fam7","fam9","fam10","fam11","fam12","fam13","fam14","fam15","fam16","fam18","fam17","fam19","fam20","fam21","fam22","fam23","fam24","fam25","fam26","fam27","fam28","fam29","fam30","fam31","fam32","fam34","fam35","fam36","fam37","fam33","fam38","fam39","fam40","fam41","fam42","fam43","fam44","fam45","fam46"],[4.43050193050193,3.10759377859103,4.06281407035176,3.37783375314861,9,3.51525974025974,4.38095238095238,4.38095238095238,2.45454545454546,3.375,3.73491124260355,3.71578784598732,4.51015801354402,3.85714285714286,4.5,4.5,4.63803680981595,2.92,4.5,5.64179104477612,4.73424657534246,3.55547612239062,2.17410228509249,4.59237536656891,3.53051058530511,3.65211754537597,3.24889170360988,8.4375,6.41584158415842,3.64521193092622,7.19021739130435,4.0745672436751,3.10248447204969,5.33333333333333,3.4488468361916,4.69565217391304,9,4.19479495268139,3.74840860866929,2.96918837675351,2.96918837675351,3.23260364436835,1.81558529180254,4.31360946745562,null,3.49865021210953,5.45833333333333,3.95899179835967,5.08441558441558,5.81012658227848,7.80722891566265,4.68265682656827,5.72420262664165,3.69551115723923,5.40575079872205,3.46895074946467,5.26829268292683,6.75,4.08764940239044,3.80859758090879,8.05263157894737,11.0454545454545,3.6,4.24719101123596,4.00337574655933,3.38745387453875,7.86075949367089,3.60724346076459,8.95544554455446,4.1566265060241,5.625,5.98203592814371,3.22281167108753,5.36029411764706,5.04545454545454,4.72040220564385,3.63977900552486,4.57627118644068,4.44512195121951,4.40044247787611,1.6875,4.50323508267434,5.4,5.59756097560976,3.89606458123108,4.88297872340426,4.27893536121673,4.17828596802842,5.57401812688822,3.35106382978723,6.51724137931035,5.08910212474297]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>playerID\u003c/th>\n      <th>nameFirst\u003c/th>\n      <th>nameLast\u003c/th>\n      <th>family\u003c/th>\n      <th>family_key\u003c/th>\n      <th>ERA\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":6},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"rowCallback":"function(row, data) {\nDTWidget.formatRound(this, row, data, 6, 2);\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->

### 3.4. Visualize the father and sons’ ERA

#### 3.4.1. Sons who outperformed their fathers

If the father’s ERA is higher than the son’s, 
one can say the son has outperformed his father. 
If the father’s ERA is higher than average, 
the son’s ERA tends to be better, due to the data regressing to the mean.


~~~{.r}
player_era_mean <- player_pitch_m[,-c(1:3)] %>% spread(key=family, value=ERA) %>% 
  dplyr::rename(father_era=father, son_era=son)

player_viz_df <- player_era_mean %>% left_join(player_pitcher, by="family_key")

# the son’s ERA tends to be better father
ggplot(player_viz_df, aes(father_era, son_era)) +
  geom_abline(intercept = 0, slope = 1, size=1, color="lightgreen") +
  geom_point() +
  geom_smooth(method="lm", se=FALSE) +
  labs(
    x="Father ERA",
    y="Son ERA") +
  theme_economist(base_size = 14, base_family="NanumMyeongjo") +
  scale_y_continuous(breaks = seq(3, 12, 3),
                     labels = c('3.0', '6.0', '9.0', '12'), 
                     limits = c(2, 12)) +
  scale_x_continuous(breaks = seq(3, 12, 3),
                     labels = c('3.0', '6.0', '9.0', '12'), 
                     limits = c(2, 10)) +
  geom_text_repel(aes(label=ifelse(son_era < father_era, son,'')))
~~~

<img src="fig/pe-baseball-son-win-1.png" style="display: block; margin: auto;" />

~~~{.r}
player_viz_df %>% dplyr::filter(son_era<father_era) %>% arrange(son_era) %>% 
  dplyr::select(family_key, father, father_era, son, son_era) %>% 
  datatable() %>% 
  formatRound("father_era", digits = 2) %>% 
  formatRound("son_era", digits = 2)
~~~

<!--html_preserve--><div id="htmlwidget-e80ac8e77281c442bcf1" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-e80ac8e77281c442bcf1">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16"],["fam36","fam27","fam44","fam20","fam31","fam7","fam14","fam19","fam13","fam24","fam17","fam34","fam37","fam29","fam28","fam5"],["Fred Rath Sr.","Lance McCullers","Hank Webb","Joe Landrum","Chet Nichols Sr.","Joe Coleman","Ross Grimsley","Lew Krausse Sr.","Steve Grilli","Jack Lively","Dave Johnson","Jay Pettibone","Walt Ripley","René Monteagudo","Willie Mills","Dave Bennett"],[4.69565217391304,3.24889170360988,4.31360946745562,5.64179104477612,7.19021739130435,4.38095238095238,3.85714285714286,4.5,4.51015801354402,4.59237536656891,4.63803680981595,5.33333333333333,9,6.41584158415842,8.4375,9],["Fred Rath Jr.","Lance McCullers Jr.","Ryan Webb","Bill Landrum","Chet Nichols Jr.","Joe Coleman","Ross Grimsley","Lew Krausse Jr.","Jason Grilli","Buddy Lively","Steve Johnson","Jonathan Pettibone","Allen Ripley","Aurelio Monteagudo","Art Mills","Erik Bennett"],[1.6875,3.22281167108753,3.35106382978723,3.38745387453875,3.63977900552486,3.69551115723923,3.80859758090879,4.00337574655933,4.08764940239044,4.1566265060241,4.24719101123596,4.44512195121951,4.50323508267434,5.04545454545454,5.36029411764706,7.80722891566265]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>family_key\u003c/th>\n      <th>father\u003c/th>\n      <th>father_era\u003c/th>\n      <th>son\u003c/th>\n      <th>son_era\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":[3,5]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"rowCallback":"function(row, data) {\nDTWidget.formatRound(this, row, data, 5, 2);\nDTWidget.formatRound(this, row, data, 3, 2);\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->

#### 3.4.2. Sons who performed worse than their fathers

If the father’s ERA is lower than the son’s, 
one can say than the son didn’t perform as well as his father. 
If the father’s ERA is low, due to the data regressing towards the mean, 
the son’s ERA tends to be higher than the father’s, 
and shows that the sons didn’t perform as well as their fathers.


~~~{.r}
# Sons who performed worse than their fathers
ggplot(player_viz_df, aes(father_era, son_era)) +
  geom_abline(intercept = 0, slope = 1, size=1, color="lightgreen") +
  geom_point() +
  geom_smooth(method="lm") +
  labs(
    x="Father ERA",
    y="Son ERA") +
  theme_economist(base_size = 14, base_family="NanumMyeongjo") +
  scale_y_continuous(breaks = seq(3, 12, 3),
                     labels = c('3.0', '6.0', '9.0', '12'), 
                     limits = c(2, 12)) +
  scale_x_continuous(breaks = seq(3, 12, 3),
                     labels = c('3.0', '6.0', '9.0', '12'), 
                     limits = c(2, 9)) +
  geom_text_repel(aes(label=ifelse(son_era > father_era, son,'')))
~~~

<img src="fig/pe-baseball-father-win-1.png" style="display: block; margin: auto;" />

~~~{.r}
player_viz_df %>% dplyr::filter(son_era>father_era) %>% arrange(father_era) %>% 
  dplyr::select(family_key, father, father_era, son, son_era) %>% 
  datatable() %>% 
  formatRound("father_era", digits = 2) %>% 
  formatRound("son_era", digits = 2)
~~~

<!--html_preserve--><div id="htmlwidget-7d36d20135a2a80d712d" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-7d36d20135a2a80d712d">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29"],["fam43","fam23","fam9","fam18","fam40","fam41","fam33","fam2","fam42","fam10","fam4","fam35","fam46","fam6","fam25","fam22","fam30","fam26","fam12","fam11","fam39","fam3","fam32","fam38","fam8","fam1","fam15","fam16","fam21"],["Ed Walsh","Glenn Liebhardt Sr.","Mardie Cornejo","Rankin Johnson Sr.","Mel Stottlemyre","Mel Stottlemyre","Diomedes Olivo","Jim Bagby Sr.","Dizzy Trout","Bill Crouch","Steve Bedrosian","Herman Pillette","Clyde Wright","Pedro Borbón","Willard Mains","Thornton Lee","Julio Navarro","Jim McAndrew","Dick Ellsworth","Doug Drabek","Jeff Russell","Floyd Bannister","John O'Donoghue","Bruce Ruffin","Joe Coleman","Mike Bacsik","Clarence Heise","Pat Jacquez","Bill Laxton"],[1.81558529180254,2.17410228509249,2.45454545454546,2.92,2.96918837675351,2.96918837675351,3.10248447204969,3.10759377859103,3.23260364436835,3.375,3.37783375314861,3.4488468361916,3.49865021210953,3.51525974025974,3.53051058530511,3.55547612239062,3.64521193092622,3.65211754537597,3.71578784598732,3.73491124260355,3.74840860866929,4.06281407035176,4.0745672436751,4.19479495268139,4.38095238095238,4.43050193050193,4.5,4.5,4.73424657534246],["Ed Walsh Jr.","Glenn Liebhardt Jr.","Nate Cornejo","Rankin Johnson Jr.","Mel Stottlemyre Jr.","Todd Stottlemyre","Gilberto Rondón","Jim Bagby Jr.","Steve Trout","Bill Crouch","Cam Bedrosian","Duane Pillette","Jaret Wright","Pedro Borbón Jr.","Jim Mains","Don Lee","Jaime Navarro","Jamie McAndrew","Steve Ellsworth","Kyle Drabek","James Russell","Brian Bannister","John O'Donoghue","Chance Ruffin","Casey Coleman","Mike Bacsik","Jim Heise","Tom Jacquez","Brett Laxton"],[5.57401812688822,8.95544554455446,5.40575079872205,3.6,4.88297872340426,4.27893536121673,5.4,3.95899179835967,4.17828596802842,3.46895074946467,5.81012658227848,4.40044247787611,5.08910212474297,4.68265682656827,5.625,3.60724346076459,4.72040220564385,5.98203592814371,6.75,5.26829268292683,3.89606458123108,5.08441558441558,4.57627118644068,5.59756097560976,5.72420262664165,5.45833333333333,8.05263157894737,11.0454545454545,7.86075949367089]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>family_key\u003c/th>\n      <th>father\u003c/th>\n      <th>father_era\u003c/th>\n      <th>son\u003c/th>\n      <th>son_era\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":[3,5]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"rowCallback":"function(row, data) {\nDTWidget.formatRound(this, row, data, 5, 2);\nDTWidget.formatRound(this, row, data, 3, 2);\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->









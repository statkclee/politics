# 데이터 과학자와 함께 하는 제19대 대통령 선거




## 페이스북 페이지 팬수 추세

민주당 문재인 후보와 국민의당 안철수 후보가 양강 체계를 구축하고 치열한 각축전을 벌이고 있다.
심상정 후보는 페이스북을 통해 지지세를 넓혀가고 있고, 홍준표와 유승민 후보는 페북이 유의미한 숫자를 만들어 내고 있지는 않다. 페이스북 팬수 추세를 시각화 한다.

## TV 토론이 지지율에 미치는 효과

TV 토론이 지지율에 미치는 효과를 검증하기 위한 도구로 페이스북 페이지 팬수 증가분을 통계지표로 삼는다.

- **대선 후보 TV 토론 일정** 
    - ~~KBS 비법정 토론: 4월 19일~~
    - ~~1차 TV 토론 (정치): 4월 23일~~
    - ~~JTBC 비법정 토론 (양극화와 안보): 4월 25일~~
    - ~~2차 TV 토론 (경제): 4월 28일~~
    - ~~3차 TV 토론 (사회): 5월 2일~~

심상정 후보는 JTBC TV토론을 비롯한 5차례 TV토론 효과를 가장 많이 본 것으로 보이며 안철수 후보는 TV토론에서 초반에 열세를 보였지만, 후반에는 신속한 학습능력을 통해 만회를 한 것으로 보인다.

### 페이스북 페이지 팬수 비교 


~~~{.r}
# 0. 참고문헌 ---------------------------------------------------------------------
library(Rfacebook)
library(RCurl)
library(tidyverse)
library(lubridate)
library(ggplot2)
library(stringr)
library(DT)
library(gridExtra)
library(ggthemes)
library(extrafont)
loadfonts()
fb_keys <- "EAACEdEose0cBAFkMShc5ip4LjKRCZCblqzPPZAPXFOZCao5vCMEaNumbWsVug53JvL6UOxZBrfZC6haf4hfRfmCdGfySXEqsU8gpvbZAp7UunaRkgiW2TEZCM9Na2vZCdeznpdaxfdckx7yasF8Cbe1HT3G3RFVjZB6J03VEDOrBCIG9Kii9olNBi"

fb_crawl_date <- "2017-05-02"

# 1. 데이터 불러오기 ---------------------------------------------------------------------

## 1.1. 페북 데이터 데이터---------------------------------

get_fb_likes <- function(fb_pages, sdate, edate, fb_keys){
    url         <- paste0("https://graph.facebook.com/v2.8/", fb_pages, "/insights/page_fans_country/lifetime?&since=", sdate, "&until=", edate, "&access_token=", fb_keys)
    url_json    <- getURL(url)
    url_list    <- fromJSON(url_json)
    url_dat_likes  <- lapply(url_list$data[[1]]$values, function(x) {x$value})
    library(gtools)
    url_dat_df <- do.call(gtools::smartbind, url_dat_likes)
    url_dat_df[is.na(url_dat_df)] <- 0
    url_date <- lapply(url_list$data[[1]]$values, function(x) {x$end_time})
    url_date_df <- do.call(rbind, url_date)
    
    url_df <- cbind(url_date_df, url_dat_df)
    url_df <- url_df %>%
        dplyr::mutate(url_date_df = lubridate::ymd(stringr::str_sub(url_date_df, 1, 10))) %>% 
        dplyr::rename(fdate=url_date_df)
    return(url_df)
}

# 2. 데이터 가져오기 ---------------------------------------------------------------

## 2.1. 안철수
ahn_fan_1703 <- get_fb_likes("ahncs111", "2017-01-01", "2017-03-30", fb_keys)
ahn_fan_1704 <- get_fb_likes("ahncs111", "2017-03-30", fb_crawl_date, fb_keys)

ahn_fan <- bind_rows(ahn_fan_1703, ahn_fan_1704)

ahn_fan_lng <- ahn_fan %>% 
    gather(`iso-a2`, fans, -fdate) %>% 
    mutate(fans = ifelse(is.na(fans), 0, fans))

ahn_fan_lng <- ahn_fan_lng %>% 
    group_by(fdate) %>% 
    dplyr::summarise(ahn_fans = sum(fans))

write_csv(ahn_fan_lng, "data/fb_ahn.csv")

## 2.2. 문재인
moon_fan_1703 <- get_fb_likes("moonbyun1", "2017-01-01", "2017-03-30", fb_keys)
moon_fan_1704 <- get_fb_likes("moonbyun1", "2017-03-30", fb_crawl_date, fb_keys)

moon_fan <- bind_rows(moon_fan_1703, moon_fan_1704)

moon_fan_lng <- moon_fan %>% 
    gather(`iso-a2`, fans, -fdate) %>% 
    mutate(fans = ifelse(is.na(fans), 0, fans))

moon_fan_lng <- moon_fan_lng %>% 
    group_by(fdate) %>% 
    dplyr::summarise(moon_fans = sum(fans))

write_csv(moon_fan_lng, "data/fb_moon.csv")

## 2.3. 심상정
sim_fan_1703 <- get_fb_likes("simsangjung", "2017-01-01", "2017-03-30", fb_keys)
sim_fan_1704 <- get_fb_likes("simsangjung", "2017-03-30", fb_crawl_date, fb_keys)

sim_fan <- bind_rows(sim_fan_1703, sim_fan_1704)

sim_fan_lng <- sim_fan %>% 
    gather(`iso-a2`, fans, -fdate) %>% 
    mutate(fans = ifelse(is.na(fans), 0, fans))

sim_fan_lng <- sim_fan_lng %>% 
    group_by(fdate) %>% 
    dplyr::summarise(sim_fans = sum(fans))

write_csv(sim_fan_lng, "data/fb_sim.csv")
~~~


~~~{.r}
## 2.4. 데이터 정제 ---------------------------------------------------------------

moon_df <- read_csv("data/fb_moon.csv")
ahn_df <- read_csv("data/fb_ahn.csv")
sim_df <- read_csv("data/fb_sim.csv")

fb_fans_df <- moon_df %>% 
    left_join(ahn_df, by="fdate") %>% 
    left_join(sim_df, by="fdate")

fb_fans_long_df <- fb_fans_df %>% 
    gather(candidate, fans, -fdate) %>% 
    mutate(candidate = factor(candidate, levels=c("moon_fans", "sim_fans", "ahn_fans")))


DT::datatable(fb_fans_df) %>% 
  DT::formatCurrency(c(2,3,4), '', mark=",", digits=0)
~~~

<!--html_preserve--><div id="htmlwidget-a6f16072b1df846cf477" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-a6f16072b1df846cf477">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92","93","94","95","96","97","98","99","100","101","102","103","104","105","106","107","108","109","110","111","112","113","114","115","116","117","118","119","120","121"],["2017-01-01","2017-01-02","2017-01-03","2017-01-04","2017-01-05","2017-01-06","2017-01-07","2017-01-08","2017-01-09","2017-01-10","2017-01-11","2017-01-12","2017-01-13","2017-01-14","2017-01-15","2017-01-16","2017-01-17","2017-01-18","2017-01-19","2017-01-20","2017-01-21","2017-01-22","2017-01-23","2017-01-24","2017-01-25","2017-01-26","2017-01-27","2017-01-28","2017-01-29","2017-01-30","2017-01-31","2017-02-01","2017-02-02","2017-02-03","2017-02-04","2017-02-05","2017-02-06","2017-02-07","2017-02-08","2017-02-09","2017-02-10","2017-02-11","2017-02-12","2017-02-13","2017-02-14","2017-02-15","2017-02-16","2017-02-17","2017-02-18","2017-02-19","2017-02-20","2017-02-21","2017-02-22","2017-02-23","2017-02-24","2017-02-25","2017-02-26","2017-02-27","2017-02-28","2017-03-01","2017-03-02","2017-03-03","2017-03-04","2017-03-05","2017-03-06","2017-03-07","2017-03-08","2017-03-09","2017-03-10","2017-03-11","2017-03-12","2017-03-13","2017-03-14","2017-03-15","2017-03-16","2017-03-17","2017-03-18","2017-03-19","2017-03-20","2017-03-21","2017-03-22","2017-03-23","2017-03-24","2017-03-25","2017-03-26","2017-03-27","2017-03-28","2017-03-29","2017-03-30","2017-03-31","2017-04-01","2017-04-02","2017-04-03","2017-04-04","2017-04-05","2017-04-06","2017-04-07","2017-04-08","2017-04-09","2017-04-10","2017-04-11","2017-04-12","2017-04-13","2017-04-14","2017-04-15","2017-04-16","2017-04-17","2017-04-18","2017-04-19","2017-04-20","2017-04-21","2017-04-22","2017-04-23","2017-04-24","2017-04-25","2017-04-26","2017-04-27","2017-04-28","2017-04-29","2017-04-30","2017-05-01"],[462633,463046,463443,463908,464289,464800,465445,465795,466059,466295,466555,466974,467544,467900,468272,468578,468964,469534,469904,470318,470525,470903,471593,472151,472524,472976,473414,473715,473982,474329,474521,474758,475405,475959,476518,477068,477535,477998,478327,478469,478765,478909,479072,479179,479293,479373,479623,479737,479899,479960,480000,480072,480095,480112,480125,480137,480189,480264,480393,480405,480466,480538,480594,480879,481063,481066,481057,481071,481804,482126,482371,482494,482592,482665,482943,483064,483140,483115,483071,483159,483614,483871,484059,484225,484302,484347,485811,486506,486808,486923,487072,487176,487231,487391,487542,487683,487822,488180,488668,489189,489818,490215,490420,490723,490930,491471,492415,493725,494457,495550,496430,497242,498172,499127,499743,500107,500469,501402,501929,502611,503102],[94422,94426,94457,94490,94780,94920,94981,95015,95053,95093,95117,95162,95192,95221,95238,95264,95281,95304,95370,95415,95437,95474,95526,95547,95627,95674,95725,95802,95871,96451,96514,96546,96690,96733,96839,96904,96990,97102,97165,97234,97326,97364,97378,97393,97456,97484,97736,97851,97925,97963,98009,98070,98127,98198,98643,98942,99089,99176,99267,99299,99349,99386,99475,99539,99594,99645,99683,99717,99900,100057,100198,100323,100428,100583,100761,100896,101031,101214,101397,101541,101715,101896,102049,102184,102424,102650,102891,103081,103256,103514,103797,104049,104288,104718,105514,106258,106839,107348,107757,108348,108792,109125,109314,109547,109785,109997,110317,110953,111469,112098,112582,112967,113643,114112,114401,114668,115010,115394,115639,115988,116179],[182787,182954,183088,183233,183366,183510,183666,183759,183876,183964,184139,184275,184422,184580,184715,184816,185045,185766,186332,186873,187214,187431,187670,187972,188212,188433,188633,188867,188958,189064,189150,189290,189971,190303,190634,190873,191244,191758,192064,192329,192452,192503,192628,192656,192722,192784,192878,193012,193118,193211,193268,193300,193372,193434,193497,193543,193710,194348,194640,194931,195150,195334,195491,195727,196015,196149,196228,196341,197130,197619,198920,199999,200656,201095,201509,201834,202155,202403,202962,203491,203885,204413,204936,205390,205864,206066,206565,206815,206982,207178,207470,207899,208368,208771,209204,209568,210204,210654,210939,211319,211714,212285,212682,213752,214727,215482,216153,216959,218400,219590,220615,221565,222461,224409,226438,232102,240054,243644,249468,254119,256451]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>fdate<\/th>\n      <th>moon_fans<\/th>\n      <th>ahn_fans<\/th>\n      <th>sim_fans<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[2,3,4]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"rowCallback":"function(row, data) {\nDTWidget.formatCurrency(this, row, data, 2, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 3, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 4, '', 0, 3, ',', '.', true);\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->

### 페이스북 페이지 팬수 비교 

`ggsave()` 함수로 그래프 시각화 산출물을 저장할 경우 가장 마지막 그래프만 자동으로 저장한다.
[Saving grid.arrange() plot to file](http://stackoverflow.com/questions/17059099/saving-grid-arrange-plot-to-file)을 참조하면 그런 문제를 해결할 수 있다. 즉 `grid.arrange` 대신 `arrangeGrob` 함수를 사용하면 원하는 방식으로 그래프를 저장할 수 있다.


~~~{.r}
# 3. 데이터 시각화 ---------------------------------------------------
## 3.1. 전체 후보 ----------------------------------------------------
fans_p <- fb_fans_long_df %>% 
    ggplot() +
    aes(x=fdate, y=fans, fill=candidate, color=candidate) +
    geom_line(size=1.7) +
    scale_x_date(date_labels = "%m-%d") +
    scale_y_continuous(labels=scales::comma) +
    # theme_minimal(base_family="NanumGothic") +
    labs(x="",y="",title="Facebook Fans Trend",
         caption="\n Source: Facebook for Developers, Graph API Explorer") +
    theme(legend.position="top") +
    scale_colour_manual(name="",
                        values=c(moon_fans="#065ce8", 
                                 ahn_fans="#07f91f",
                                 sim_fans="#f2ee09")) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-05-02")), linetype=4, color="red", size=0.9)

grid.arrange(fans_p, nrow=1)
~~~

<img src="fig/facebook-page-moon-ahn-viz-1.png" style="display: block; margin: auto;" />

~~~{.r}
## 3.2. 후보 별로 ----------------------------------------------------

ahn_p1 <- ahn_df %>% 
    ggplot(aes(x= fdate, y=ahn_fans/1000)) +
    geom_line(colour="#07f91f") +
    geom_point(size=1.3, colour="#07f91f") +
    scale_x_date(date_labels = "%m-%d") +
    scale_y_continuous(labels=scales::comma) +
    # theme_minimal(base_family="NanumGothic") +
    labs(x="",y="",title="Ahn Facebook Fans Trend",
         caption="\n Source: Facebook(ahncs111)") +
    theme(legend.position="none") +
    geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-05-02")), linetype=4, color="red", size=0.9)

moon_p1 <- moon_df %>% 
    ggplot(aes(x= fdate, y=moon_fans/1000)) +
    geom_line(colour="#065ce8") +
    geom_point(size=1.3, colour="#065ce8") +
    scale_x_date(date_labels = "%m-%d") +
    scale_y_continuous(labels=scales::comma) +
    # theme_minimal(base_family="NanumGothic") +
    labs(x="",y="",title="Moon Facebook Fans Trend",
         caption="\n Source: Facebook(moonbyun1)") +
    theme(legend.position="none") +
    geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-05-02")), linetype=4, color="red", size=0.9)

sim_p1 <- sim_df %>% 
    ggplot(aes(x= fdate, y=sim_fans/1000)) +
    geom_line(colour="#f2ee09") +
    geom_point(size=1.3, colour="#f2ee09") +
    scale_x_date(date_labels = "%m-%d") +
    scale_y_continuous(labels=scales::comma) +
    # theme_minimal(base_family="NanumGothic") +
    labs(x="",y="",title="Sim Facebook Fans Trend",
         caption="\n Source: Facebook(simsangjung)") +
    theme(legend.position="none") +
    geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="red", size=0.9) +
    geom_vline(xintercept=as.numeric(ymd("2017-05-02")), linetype=4, color="red", size=0.9)

grid.arrange(moon_p1, sim_p1, ahn_p1, nrow=1)
~~~

<img src="fig/facebook-page-moon-ahn-viz-2.png" style="display: block; margin: auto;" />

~~~{.r}
## 3.3. 후보 별로 확대 ----------------------------------------------------

ahn_p2 <- ahn_df %>% dplyr::filter(fdate >="2017-04-01") %>% 
    ggplot(aes(x= fdate, y=ahn_fans)) +
    geom_line(colour="#07f91f") +
    geom_point(size=1.7, colour="#07f91f") +
    scale_x_date(date_labels = "%m-%d") +
    scale_y_continuous(labels=scales::comma) +
    # theme_minimal(base_family="NanumGothic") +
    labs(x="",y="",title="Ahn Facebook Fans Trend",
         caption="\n Source: Facebook(ahncs111)") +
    theme(legend.position="none") +
    geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-05-02")), linetype=4, color="#ab3ae8", size=0.3) +
    annotate(geom="text",x=as.Date("2017-04-19"), y=105000,label="KBS",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-23"), y=116000,label="1차",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-25"), y=105000,label="JTBC",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-28"), y=116000,label="2차",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-05-02"), y=105000,label="3차",fontface="bold", size=3)    

moon_p2 <- moon_df %>% dplyr::filter(fdate >="2017-04-01") %>% 
    ggplot(aes(x= fdate, y=moon_fans)) +
    geom_line(colour="#065ce8") +
    geom_point(size=1.7, colour="#065ce8") +
    scale_x_date(date_labels = "%m-%d") +
    scale_y_continuous(labels=scales::comma) +
    # theme_minimal(base_family="NanumGothic") +
    labs(x="",y="",title="Moon Facebook Fans Trend",
         caption="\n Source: Facebook(moonbyun1)") +
    theme(legend.position="none") +
    geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-05-02")), linetype=4, color="#ab3ae8", size=0.3) +
    annotate(geom="text",x=as.Date("2017-04-19"), y=492000,label="KBS",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-23"), y=502000,label="1차",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-25"), y=492000,label="JTBC",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-28"), y=502000,label="2차",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-05-02"), y=492000,label="3차",fontface="bold", size=3)

sim_p2 <- sim_df %>% dplyr::filter(fdate >="2017-04-01") %>% 
    ggplot(aes(x= fdate, y=sim_fans)) +
    geom_line(colour="#f2ee09") +
    geom_point(size=1.7, colour="#f2ee09") +
    scale_x_date(date_labels = "%m-%d") +
    scale_y_continuous(labels=scales::comma) +
    # theme_minimal(base_family="NanumGothic") +
    labs(x="",y="",title="Sim Facebook Fans Trend",
         caption="\n Source: Facebook(simsangjung)") +
    theme(legend.position="none") +
    geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="#ab3ae8", size=0.3) +
    geom_vline(xintercept=as.numeric(ymd("2017-05-02")), linetype=4, color="#ab3ae8", size=0.3) +
    annotate(geom="text",x=as.Date("2017-04-19"), y=245000,label="KBS",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-23"), y=230000,label="1차",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-25"), y=245000,label="JTBC",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-04-28"), y=230000,label="2차",fontface="bold", size=3) +
    annotate(geom="text",x=as.Date("2017-05-02"), y=245000,label="3차",fontface="bold", size=3)

grid.arrange(moon_p2, sim_p2, ahn_p2, nrow=1)
~~~

<img src="fig/facebook-page-moon-ahn-viz-3.png" style="display: block; margin: auto;" />

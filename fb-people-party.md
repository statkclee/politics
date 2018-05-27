---
layout: page
title: 데이터 과학자와 함께 하는 제19대 대통령 선거
subtitle: 페이스북 페이지 - 국민의당 경선
output:
  html_document: 
    keep_md: yes
  pdf_document:
    latex_engine: xelatex
mainfont: NanumGothic
---



## 페이스북 페이지 국민의당 경선

국민의당 내에 크게 대권을 향한 두명의 주자가 열심히 달려나가고 있다.
두 후보 중 한명은 국민의당 후보가 되어 민주당, 바른정당 등 후보와 대선을 치르게 된다.
여론지지율에서는 손학규 후보가 안철수후보와 비교하여 데이터 분석하고 있는 현 시점에서 3%와 10%대로 3배 정도 차이가 나고 있다.


### 손학규 페북 페이지 

먼저 [손학규 페북 페이지](https://www.facebook.com/koreasohn)에서 데이터를 긁어와서 `ggplot2`에 넣고 이를 `plotly`에 넣어
동적 그래프로 시각화하여 페북 활동성 추세를 살펴본다.


~~~{.r}
fb_keys <- "EAACEdEose0cBAJZCHBaDcekwSMcnQwqHHAR2qEJAO24MVKor0K1pEEWvazfWGMzeis6FCnmdl0cZAaQkEtn2BUbVau3ZBsxVhZB8XbvVQOn9OZA7xQhrMBpw9PniYFrTZC0Cc6WR5j18O4sK5A89OzaT1Luh89tyzaHHzTiCi3z07fO5gzS4HVZAg1IINLfRUYZD"

# 페북 포스트글 긁어오기: 일별 100개
get_fb_posts <- function(page_name, start_date, end_date, fb_keys) {
  # 긁어올 날짜 생성
  scraping_days <- seq(from=as.Date(start_date), to=as.Date(end_date), by='days')
  posts  <- data.frame()
  for(scrape_day in scraping_days){
    daily_post  <- data.frame()
    tryCatch({
      daily_post <- getPage(page=page_name, token=fb_keys,
                            since=as.Date(scrape_day, origin='1970-01-01'),
                            until=as.Date(scrape_day, origin='1970-01-01')+1)},
      error = function(e){})
    posts <- rbind(posts, daily_post)
  }
  return(posts)
}

start_date <- "2017-01-01"
end_date   <- "2017-03-08"

sohn <- get_fb_posts("koreasohn", start_date, end_date, fb_keys)
ahn  <- get_fb_posts("ahncs111", start_date, end_date, fb_keys)

# 1. 데이터 불러오기 ---------------------------------------------------------------------
## 1.1. 손학규 
fb_page_df <- sohn %>% 
  mutate(fb_date = ymd(substr(created_time, 1,10))) %>% 
  dplyr::select(fb_date, likes_count, comments_count, shares_count) %>% 
  group_by(fb_date) %>% 
  dplyr::summarise(
            like_mean    = mean(likes_count),
            comment_mean = mean(comments_count),
            share_mean   = mean(shares_count) )
~~~



~~~{.output}
Error in mutate_impl(.data, dots): Evaluation error: 객체 'created_time'를 찾을 수 없습니다.

~~~



~~~{.r}
fb_page_df_lng <- fb_page_df %>% ungroup %>% dplyr::select(fb_date, like_mean, share_mean, comment_mean) %>% 
  gather(type, activity_avg, -fb_date) %>% 
  mutate(type=factor(type, levels=c("like_mean", "share_mean", "comment_mean")))
~~~



~~~{.output}
Error in eval(lhs, parent, parent): 객체 'fb_page_df'를 찾을 수 없습니다

~~~



~~~{.r}
## 1.2. 안철수
ahn_fb_page_df <- ahn %>% 
  mutate(fb_date = ymd(substr(created_time, 1,10))) %>% 
  dplyr::select(fb_date, likes_count, comments_count, shares_count) %>% 
  group_by(fb_date) %>% 
  dplyr::summarise(
    like_mean    = mean(likes_count),
    comment_mean = mean(comments_count),
    share_mean   = mean(shares_count) )
~~~



~~~{.output}
Error in mutate_impl(.data, dots): Evaluation error: 객체 'created_time'를 찾을 수 없습니다.

~~~



~~~{.r}
ahn_fb_page_df_lng <- ahn_fb_page_df %>% ungroup %>% dplyr::select(fb_date, like_mean, share_mean, comment_mean) %>% 
  gather(type, activity_avg, -fb_date) %>% 
  mutate(type=factor(type, levels=c("like_mean", "share_mean", "comment_mean")))
~~~



~~~{.output}
Error in eval(lhs, parent, parent): 객체 'ahn_fb_page_df'를 찾을 수 없습니다

~~~

### 안철수 페북 페이지 

먼저 [안철수 페북 페이지](https://www.facebook.com/ahncs111)에서 데이터를 긁어와서 `ggplot2`에 넣고 이를 `plotly`에 넣어
동일한 방법으로 동적 그래프로 시각화하여 페북 활동성 추세를 살펴본다.


~~~{.r}
## 2. 페북 시각화 ---------------------------------

## 2.1. 손학규 페이지 한장 찍기
dlist <- unique(fb_page_df_lng$fb_date)
~~~



~~~{.output}
Error in unique(fb_page_df_lng$fb_date): 객체 'fb_page_df_lng'를 찾을 수 없습니다

~~~



~~~{.r}
g <- ggplot(data=fb_page_df_lng, aes(x=fb_date, y=activity_avg, color=type)) +
  geom_point(aes(colour=type), alpha=0.3) +
  geom_line() +
  theme_tufte() +
  theme(legend.position="top", 
        legend.title=element_blank(),
        plot.caption=element_text(hjust=0,size=8),
        plot.subtitle=element_text(face="italic"),
        axis.text=element_text(size=7.5))+
  labs(x="",y="",title="공식 손학규 페이스북 페이지") +
  theme(text=element_text(family="NanumGothic")) +
  scale_x_date(date_labels="%m-%d", breaks=seq(dlist[1], tail(dlist,1), "7 day"))
~~~



~~~{.output}
Error in ggplot(data = fb_page_df_lng, aes(x = fb_date, y = activity_avg, : 객체 'fb_page_df_lng'를 찾을 수 없습니다

~~~



~~~{.r}
ggplotly(g)
~~~



~~~{.output}
Error in ggplotly(g): 객체 'g'를 찾을 수 없습니다

~~~



~~~{.r}
## 2.2. 손학규 페이지 격자그림에 시각화

g_facet <- ggplot(data=fb_page_df_lng, aes(x=fb_date, y=activity_avg, color=type)) +
  geom_point(aes(colour=type), alpha=0.3) +
  geom_line() +
  theme_tufte() +
  theme(legend.position="bottom", 
        legend.title=element_blank(),
        plot.caption=element_text(hjust=0,size=8),
        plot.subtitle=element_text(face="italic"),
        axis.text=element_text(size=7.5))+
  labs(x="",y="",title="공식 손학규 페이스북 페이지") +
  theme(text=element_text(family="NanumGothic")) +
  scale_x_date(date_labels="%m-%d", breaks=seq(dlist[1], tail(dlist,1), "7 day")) +
  facet_wrap(~type, nrow=3)
~~~



~~~{.output}
Error in ggplot(data = fb_page_df_lng, aes(x = fb_date, y = activity_avg, : 객체 'fb_page_df_lng'를 찾을 수 없습니다

~~~



~~~{.r}
ggplotly(g_facet)
~~~



~~~{.output}
Error in ggplotly(g_facet): 객체 'g_facet'를 찾을 수 없습니다

~~~


~~~{.r}
## 2.2. 안철수 페이지 한장 찍기
dlist <- unique(ahn_fb_page_df_lng$fb_date)
~~~



~~~{.output}
Error in unique(ahn_fb_page_df_lng$fb_date): 객체 'ahn_fb_page_df_lng'를 찾을 수 없습니다

~~~



~~~{.r}
g <- ggplot(data=ahn_fb_page_df_lng, aes(x=fb_date, y=activity_avg, color=type)) +
  geom_point(aes(colour=type), alpha=0.3) +
  geom_line() +
  theme_tufte() +
  theme(legend.position="top", 
        legend.title=element_blank(),
        plot.caption=element_text(hjust=0,size=8),
        plot.subtitle=element_text(face="italic"),
        axis.text=element_text(size=7.5))+
  labs(x="",y="",title="공식 안철수 페이스북 페이지") +
  theme(text=element_text(family="NanumGothic")) +
  scale_x_date(date_labels="%m-%d", breaks=seq(dlist[1], tail(dlist,1), "7 day"))
~~~



~~~{.output}
Error in ggplot(data = ahn_fb_page_df_lng, aes(x = fb_date, y = activity_avg, : 객체 'ahn_fb_page_df_lng'를 찾을 수 없습니다

~~~



~~~{.r}
ggplotly(g)
~~~



~~~{.output}
Error in ggplotly(g): 객체 'g'를 찾을 수 없습니다

~~~



~~~{.r}
## 2.2. 안철수 페이지 격자그림에 시각화

g_facet <- ggplot(data=ahn_fb_page_df_lng, aes(x=fb_date, y=activity_avg, color=type)) +
  geom_point(aes(colour=type), alpha=0.3) +
  geom_line() +
  geom_smooth(se=FALSE) +
  theme_tufte() +
  theme(legend.position="bottom", 
        legend.title=element_blank(),
        plot.caption=element_text(hjust=0,size=8),
        plot.subtitle=element_text(face="italic"),
        axis.text=element_text(size=7.5))+
  labs(x="",y="",title="공식 안철수 페이스북 페이지") +
  theme(text=element_text(family="NanumGothic")) +
  scale_x_date(date_labels="%m-%d", breaks=seq(dlist[1], tail(dlist,1), "7 day")) +
  facet_wrap(~type, nrow=3)
~~~



~~~{.output}
Error in ggplot(data = ahn_fb_page_df_lng, aes(x = fb_date, y = activity_avg, : 객체 'ahn_fb_page_df_lng'를 찾을 수 없습니다

~~~



~~~{.r}
ggplotly(g_facet)
~~~



~~~{.output}
Error in ggplotly(g_facet): 객체 'g_facet'를 찾을 수 없습니다

~~~

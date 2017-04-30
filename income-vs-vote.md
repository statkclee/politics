# 데이터 과학자가 바라본 20대 총선



## 정치 경제

정치 경제라고하여 정치와 경제는 밀접한 관계가 있다. 
정치의 중요한 척도인 대선 득표율과 경제를 대표한다고 볼 수 있는 1인당 소득에 대한 관계를 살펴보자.
2012년 문재인 후보와 박근혜 후보가 보수와 진보를 대표하여 맞짱을 뜬 대선 득표율과 
각 시도별 1인당 소득의 상관관계를 살펴보자.

## 데이터

[통계청 KOSIS](http://kosis.kr/)에서 1인당 개인소득 자료를 중앙선거관리위원회에서 18대선 투표구별 개표결과 데이터를 가공하여 
정치 경제 관계를 살펴본다.

- [행정구역(시도)별 1인당 지역내총생산, 지역총소득, 개인소득](http://kosis.kr/statHtml/statHtml.do?orgId=101&tblId=DT_1C65#)
- 중앙선거관리위원회 &rarr; 분야별 정보 &rarr; 선거정보 &rarr; 하단 자료실
    - [제18대 대통령선거 투표구별 개표결과 다운로드](http://www.nec.go.kr/portal/cmm/fms/FileDown.do?atchFileId=FILE_000000000061613&fileSn=1&bbsId=)

## 환경설정

엑셀 파일을 불러읽어오기 위해 `readxl` 팩키지와 시각화에 필요한 팩키지를 불러온다.


~~~{.r}
library(tidyverse)
library(readxl)
library(ggrepel)
library(ggthemes)
library(extrafont)
loadfonts()
~~~
## 데이터 가져오기

2012년 대선 득표율 데이터와 2012년 시도별 소득데이터를 불러와서 각 후보별 득표율을 계산한다.


~~~{.r}
# 1. 데이터 가져오기 ---------------------------------------------
## 1.1. 2012년 대선 득표율 ---------------------------------------
vote_df <- read_excel("data/제18대 대선 투표구별 개표자료.xls", skip=3)
# http://stackoverflow.com/questions/28549045/dplyr-select-error-found-duplicated-column-name
valid_column_names <- make.names(names=names(vote_df), unique=TRUE, allow_ = TRUE)
names(vote_df) <- valid_column_names

names(vote_df) <- c("시도명", "구시군명", "읍면동명", "투표구명", "선거인수", "투표수", 
  "박근혜", "문재인", "X.1", "X.2", "X.3", "X.4", "X.5", "무효.투표수", "기권수")

votes_df <- vote_df %>% 
  dplyr::filter(`구시군명` =="합계" & `시도명` !="전국" & `시도명` !="세종특별자치시") %>% 
  dplyr::select(`시도명`, `투표수`, `박근혜`, `문재인`) %>% 
  mutate_each(funs(as.numeric), matches("^[박|문|투]")) %>% 
  mutate(`박득표율` = `박근혜`/`투표수`*100, 
         `문득표율` = `문재인`/`투표수`*100)

## 1.2. 2012년 1인당 소득 ---------------------------------------

income_df <- read_excel("data/행정구역_시도_별_1인당_지역내총생산__지역총소득__개인소득_20170430200327.xlsx")
names(income_df) <- c("시도명", "총소득", "개인소득", "민간소비")

incomes_df <- income_df %>% 
  dplyr::filter(`시도명` !="행정구역(시도)별" & `시도명` !="세종특별자치시") %>% 
  dplyr::select(`시도명`, `개인소득`) %>% 
  mutate_each(funs(as.numeric), matches("^[개]"))

## 1.3. 데이터 결합 --------------------------------------------

df <- votes_df %>% left_join(incomes_df, by="시도명")

# 2. 데이터 정제 ---------------------------------------------

ratio_df <- df %>% 
  dplyr::select(시도명, 박득표율, 문득표율, 개인소득) %>% 
  gather(후보, 득표율, -시도명, -개인소득)
~~~

## 데이터 시각화

2012년 개인소득(경제)과 대선 후보별득표율(정치) 간의 관계를 산점도를 그려보고 `lowess` 적합을 통해 추세를 시각화한다.


~~~{.r}
# 3. 데이터 시각화 ---------------------------------------------

ggplot(ratio_df, aes(y=`개인소득`, x=`득표율`/100, label=`시도명`, fill=`후보`)) +
  geom_point(aes(y=`개인소득`, x=`득표율`/100, color=`후보`), size=2) +
  geom_text_repel(aes(y=`개인소득`, x=`득표율`/100, label=`시도명`)) +
  geom_smooth(aes(group = 1), se=TRUE, level=0.67, size=1, color="green", alpha=0.1) +
  theme_wsj(base_family='NanumGothic') +
  scale_x_continuous(labels=scales::percent) +
  scale_y_continuous(labels=scales::comma) +
  theme(legend.position="top", 
        plot.caption=element_text(hjust=0,size=8),
        plot.subtitle=element_text(face="italic"),
        axis.text=element_text(size=10.5),
        text=element_text(family="NanumGothic")) +
  labs(x="1인당 소득 (단위:천원)",y="",title="시도별 득표수와 1인당 소득",
       subtitle="X축: 후보별 지지율(%), Y축: 1인당 소득(천원)",
       caption="\n 자료출처: 선관위 18대 대선 득표수와 통계청 시도별 소득자료")
~~~



~~~{.output}
`geom_smooth()` using method = 'loess'

~~~

<img src="fig/political-economy-viz-1.png" style="display: block; margin: auto;" />

~~~{.r}
# 4. 데이터 표 ---------------------------------------------

DT::datatable(votes_df %>% mutate(`박득표율` = `박득표율`/100, `문득표율`=`문득표율`/100)) %>% 
  DT::formatCurrency(c(2,3,4), currency="", interval=3, mark=",", digits=0) %>% 
  DT::formatPercentage(c('박득표율','문득표율'), digits=1)
~~~

<!--html_preserve--><div id="htmlwidget-55a6427ac415e056d658" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-55a6427ac415e056d658">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16"],["서울특별시","부산광역시","대구광역시","인천광역시","광주광역시","대전광역시","울산광역시","경기도","강원도","충청북도","충청남도","전라북도","전라남도","경상북도","경상남도","제주특별자치도"],[6307869,2219699,1585806,1657821,898416,904367,694938,7018577,911988,925778,1168095,1142133,1171210,1710122,2008683,330967],[3024572,1324159,1267789,852600,69574,450576,413977,3528915,562876,518442,658928,150315,116296,1375164,1259174,166184],[3227639,882511,309034,794213,823737,448310,275451,3442084,340870,398907,497630,980322,1038347,316659,724896,161235],[0.479491885452916,0.596548901450152,0.799460337519217,0.514289540306221,0.0774407401471034,0.498222513647667,0.595703501607338,0.502796364562218,0.617196717500669,0.560006826690632,0.564104803119609,0.131609015762613,0.099295600276637,0.804132102855819,0.626865463589825,0.502116525212483],[0.511684532446695,0.397581383782216,0.194875035155624,0.479070418338289,0.91687703691831,0.495716893694706,0.396367733524429,0.490424768439528,0.373765882884424,0.430888398730581,0.426018431720023,0.858325606562458,0.886559199460387,0.185167490974328,0.360881234122059,0.487163372783389]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>시도명<\/th>\n      <th>투표수<\/th>\n      <th>박근혜<\/th>\n      <th>문재인<\/th>\n      <th>박득표율<\/th>\n      <th>문득표율<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[2,3,4,5,6]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"rowCallback":"function(row, data) {\nDTWidget.formatPercentage(this, row, data, 5, 1);\nDTWidget.formatPercentage(this, row, data, 6, 1);\nDTWidget.formatCurrency(this, row, data, 2, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 3, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 4, '', 0, 3, ',', '.', true);\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->

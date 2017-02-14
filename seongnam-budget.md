# 데이터 과학자와 함께 하는 제19대 대통령 선거



## 지방자치단체 세입 세출 데이터

지방자치단체 세입 세출 데이터를 바탕으로 지방자치단체 세입원을 식별하고 가장 많은 세출이 집행되는 부분을 시각화한다.
4차 산업혁명의 전초기지로 평가받는 판교테크노밸리가 소재하고 있는 성남시 세입 세출 데이터를 활용해 본다.

## 생키그림 시각화

### 환경설정

세입 세출 데이터를 바탕으로 생키그림을 통해 세입과 세출의 흐름을 시각화하기 위해서 환경을 설정한다.


~~~{.r}
# 0. 환경설정 --------------------------------------------------------------
library(tidyverse)
library(stringr)
library(googleVis)
library(readxl)
library(scales)
library(stringr)
~~~

### 세입세출 데이터 전처리

엑셀에서 데이터를 가져와서 `network3D` 팩키지 `sankeyNetwork` 시각화를 위해 데이터를 준비한다.
R과 달리 자바스크립트는 0부터 인덱스가 시작되어 이를 맞춰주는 작업을 한다.

`sankeyNetwork` 함수에 데이터를 넣을 때 가장 먼저 링크와 노드를 별도 데이터프레임으로 준비하고 이를 인자에 맞춰 넣어준다.


~~~{.r}
# 1. 데이터 가져오기 --------------------------------------------------------------
rm(list=ls())

outflow <- read_excel("data/Seongnam_Sankey_2017_original.xlsx", sheet="세출") %>% 
  mutate(from = str_replace_all(from, "\\s", ""),
         to  = str_replace_all(to, "\\s", ""))
inflow <- read_excel("data/Seongnam_Sankey_2017_original.xlsx", sheet="세입") %>% 
  mutate(from = str_replace_all(from, "\\s", ""),
         to  = str_replace_all(to, "\\s", ""))

inflow_detail <- read_excel("data/Seongnam_Sankey_2017_original.xlsx", sheet="지방세") %>% 
  mutate(from = str_replace_all(from, "\\s", ""),
         to  = str_replace_all(to, "\\s", ""))


tax_flow <- bind_rows(inflow_detail, inflow, outflow)

tax_node <- unique(c(tax_flow$from, tax_flow$to))
tax_node <- str_replace_all(tax_node, "\\s", "")

tax_node_df <- data.frame(id=c(0:(length(tax_node)-1)), name=tax_node)
lut <- c("보통세" = 0, 
"보통세외" = 1, 
"주민세" = 2, 
"재산세" = 3, 
"자동차세" = 4, 
"담배소비세" = 5, 
"지방소득세" = 6, 
"지방세" = 7, 
"세외수입" = 8, 
"지방교부세" = 9, 
"조정교부금" = 10, 
"국가,도비보조금" = 11, 
"지방채" = 12, 
"보존수입등및내부거래" = 13, 
"세입세출" = 14, 
"일반공공행정" = 15, 
"공공질서및안전" = 16, 
"교육" = 17, 
"문화및관광" = 18, 
"환경보호" = 19, 
"사회복지" = 20, 
"보건" = 21, 
"농림해양수산" = 22, 
"산업,중소기업" = 23, 
"수송및교통" = 24, 
"국토및지역개발" = 25, 
"예비비" = 26, 
"기타" = 27)

tax_flow$from <-  lut[tax_flow$from]
tax_flow$to <-  lut[tax_flow$to]

tax_node_df <- data.frame(name =tax_node_df[,2])
~~~

### 세입세출 데이터 전처리

`sankeyNetwork` 함수에 한글 글꼴, 노드 폭, 글꼴 크기를 설정하여 시각화를 완성한다.


~~~{.r}
# 2. 생키그림 시각화 --------------------------------------------------------------

sankeyNetwork(Links = tax_flow, Nodes = tax_node_df, Source = 'from',
              Target = 'to', Value = 'amt', NodeID = 'name',
              width = 700, fontSize = 12, nodeWidth = 30, fontFamily="NanumGothic")
~~~



~~~{.output}
Links is a tbl_df. Converting to a plain data frame.

~~~

<!--html_preserve--><div id="htmlwidget-744c710f372733784cb6" style="width:700px;height:480px;" class="sankeyNetwork html-widget"></div>
<script type="application/json" data-for="htmlwidget-744c710f372733784cb6">{"x":{"links":{"source":[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,14,14,14,14,14,14,14,14,14,14,14,14],"target":[7,7,0,0,0,0,0,14,14,14,14,14,14,14,15,16,17,18,19,20,21,22,23,24,25,26,27],"value":[8332.31,130,428.575,2529.37,1194.43,501,3678.94,8462.31,1563.24,122.84,2132.81,3782.38,190,840,1063.5,49.67,626.48,1432.1,990.92,6315.59,1283.24,91.41,396.69,1563.22,946.86,94.55,2269.35]},"nodes":{"name":["보통세","보통세외","주민세","재산세","자동차세","담배소비세","지방소득세","지방세","세외수입","지방교부세","조정교부금","국가,도비보조금","지방채","보존수입등및내부거래","세입세출","일반공공행정","공공질서및안전","교육","문화및관광","환경보호","사회복지","보건","농림해양수산","산업,중소기업","수송및교통","국토및지역개발","예비비","기타"],"group":["보통세","보통세외","주민세","재산세","자동차세","담배소비세","지방소득세","지방세","세외수입","지방교부세","조정교부금","국가,도비보조금","지방채","보존수입등및내부거래","세입세출","일반공공행정","공공질서및안전","교육","문화및관광","환경보호","사회복지","보건","농림해양수산","산업,중소기업","수송및교통","국토및지역개발","예비비","기타"]},"options":{"NodeID":"name","NodeGroup":"name","LinkGroup":null,"colourScale":"d3.scale.category20()","fontSize":12,"fontFamily":"NanumGothic","nodeWidth":30,"nodePadding":10,"units":"","margin":{"top":null,"right":null,"bottom":null,"left":null},"iterations":32}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

# 데이터 과학자와 함께 하는 제19대 대통령 선거



## 여론조사기관 PDF 파일에서 지지율 흐름도까지

[리얼미터](http://www.realmeter.net/)에서 발간하는 PDF 파일을 파싱하여 대선후보이 어디에서 왔는지를 생키플롯을 통해 시각화한다.
[리얼미터 주간 여론조사](http://www.realmeter.net/category/pdf/) 웹사이트에서 PDF 파일을 다운로드 받아 디렉토리에 저장하고,
지지율 흐름도를 시각화할 데이터를 추출한다. 

특히, 지역, 정당, 직업, 후보, 연령별로 지지율 데이터를 추출하여 생키그림(SankeyPlot)으로 지지율 흐름을 시각화한다.


~~~{.r}
# 0. 환경설정 -----------------------------------------------
library(networkD3)
library(pdftools)
library(tidyverse)
library(stringr)
library(lubridate)
library(testthat)
library(ggthemes)
library(extrafont)
loadfonts()
~~~

### 생키그림으로 시각화할 데이터

지역-정당, 정당-직업, 직업-후보, 후보-연령 정보가 연결되어 있는 리얼미터 여론조사 PDF파일을 파싱하여 
`csv`로 사전 작업하여 정리한다.


~~~{.r}
# 0. 환경설정 -----------------------------------------------

# 1. 데이터 가져오기 -----------------------------------------------
## 1.1. 지역-정당 데이터 가져오기 ----------------------------------
region2party <- read_csv("data/sankey_region_to_party.csv")
~~~



~~~{.output}
Parsed with column specification:
cols(
  지역 = col_character(),
  민주당 = col_double(),
  새누리당 = col_double(),
  국민의당 = col_double(),
  바른정당 = col_double(),
  정의당 = col_double(),
  기타정당 = col_double()
)

~~~



~~~{.r}
region2party <- region2party %>% gather(party, support, -지역) %>% dplyr::select(from=지역, to= party, support)

## 1.2. 정당-직업 데이터 가져오기 ----------------------------------
party2job <- read_csv("data/sankey_party_to_job.csv")  
~~~



~~~{.output}
Parsed with column specification:
cols(
  직업 = col_character(),
  민주당 = col_double(),
  새누리당 = col_double(),
  국민의당 = col_double(),
  바른정당 = col_double(),
  정의당 = col_double(),
  기타정당 = col_double()
)

~~~



~~~{.r}
party2job <- party2job %>% gather(party, support, -직업) %>% dplyr::select(from=party, to=직업, support)

## 1.3. 직업-후보 데이터 가져오기 ----------------------------------
job2person <- read_csv("data/sankey_job_to_person.csv")  
~~~



~~~{.output}
Parsed with column specification:
cols(
  직업 = col_character(),
  문재인 = col_double(),
  손학규 = col_double(),
  안철수 = col_double(),
  안희정 = col_double(),
  유승민 = col_double(),
  이재명 = col_double(),
  황교안 = col_double(),
  기타후보 = col_double()
)

~~~



~~~{.r}
job2person <- job2person %>% gather(person, support, -직업) %>% dplyr::select(from=직업, to= person, support)

## 1.4. 후보-연령 데이터 가져오기 ----------------------------------
person2age <- read_csv("data/sankey_person_to_age.csv")  
~~~



~~~{.output}
Parsed with column specification:
cols(
  연령 = col_character(),
  문재인 = col_double(),
  손학규 = col_double(),
  안철수 = col_double(),
  안희정 = col_double(),
  유승민 = col_double(),
  이재명 = col_double(),
  황교안 = col_double(),
  기타후보 = col_double()
)

~~~



~~~{.r}
person2age <- person2age %>% gather(person, support, -연령) %>% dplyr::select(from=person, to= 연령, support)
~~~

### 노드와 링크 형태 네트워크 데이터 변환

`networkD3` 팩키지 `sankeyNetwork` 함수를 활용하여 생키그림을 통해 지지율 흐름을 시각화하는데 적당한 
형태 데이터로 가공한다.


~~~{.r}
# 2. 생키그림 데이터  -----------------------------------------------
## 2.1. 노드 데이터 -------------------------------------------------
rm_node <- unique(c(region2party$from, region2party$to, party2job$from, party2job$to,
                    job2person$from, job2person$to, person2age$from, person2age$to))

rm_node_df <- data.frame(id=c(0:(length(rm_node)-1)), name=rm_node)

lut <- c("서울" = 0, 
         "경기/인천" = 1, 
         "대전/충청/세종" = 2, 
         "강원" = 3, 
         "부산/경남/울산" = 4, 
         "대구/경북" = 5, 
         "광주/전라" = 6, 
         "제주" = 7, 
         "민주당" = 8, 
         "새누리당" = 9, 
         "국민의당" = 10, 
         "바른정당" = 11, 
         "정의당" = 12, 
         "기타정당" = 13, 
         "사무직" = 14, 
         "노동직" = 15, 
         "가정주부" = 16, 
         "자영업" = 17, 
         "학생" = 18, 
         "농림어업" = 19, 
         "무직" = 20, 
         "기타직업" = 21, 
         "문재인" = 22, 
         "손학규" = 23, 
         "안철수" = 24, 
         "안희정" = 25, 
         "유승민" = 26, 
         "이재명" = 27, 
         "황교안" = 28, 
         "기타후보" = 29, 
         "19~29세" = 30, 
         "30대" = 31, 
         "40대" = 32, 
         "50대" = 33, 
         "60세이상" = 34)

region2party$from <-  lut[region2party$from]
region2party$to <-  lut[region2party$to]

party2job$from <-  lut[party2job$from]
party2job$to <-  lut[party2job$to]

job2person$from <-  lut[job2person$from]
job2person$to <-  lut[job2person$to]

person2age$from <-  lut[person2age$from]
person2age$to <-  lut[person2age$to]


rm_node_df <- data.frame(name =rm_node_df[,2])

rm_link_df <- bind_rows(region2party, party2job) %>% bind_rows(job2person) %>% 
  bind_rows(person2age)
~~~

### 지지율 흐름 시각화

`networkD3` 팩키지 `sankeyNetwork` 함수에 데이터를 넣어 지지율 흐름을 시각화한다.


~~~{.r}
# 2. 생키그림 시각화 --------------------------------------------------------------

sankeyNetwork(Links = rm_link_df, Nodes = rm_node_df, Source = 'from',
              Target = 'to', Value = 'support', NodeID = 'name', units = '%', 
              width = 640, height=480, fontSize = 12, nodeWidth = 30, fontFamily="NanumGothic")
~~~



~~~{.output}
Links is a tbl_df. Converting to a plain data frame.

~~~

<!--html_preserve--><div id="htmlwidget-963af0aec6a666261017" style="width:640px;height:480px;" class="sankeyNetwork html-widget"></div>
<script type="application/json" data-for="htmlwidget-963af0aec6a666261017">{"x":{"links":{"source":[0,1,2,3,4,5,6,7,0,1,2,3,4,5,6,7,0,1,2,3,4,5,6,7,0,1,2,3,4,5,6,7,0,1,2,3,4,5,6,7,0,1,2,3,4,5,6,7,8,8,8,8,8,8,8,8,9,9,9,9,9,9,9,9,10,10,10,10,10,10,10,10,11,11,11,11,11,11,11,11,12,12,12,12,12,12,12,12,13,13,13,13,13,13,13,13,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,22,22,22,22,22,23,23,23,23,23,24,24,24,24,24,25,25,25,25,25,26,26,26,26,26,27,27,27,27,27,28,28,28,28,28,29,29,29,29,29],"target":[8,8,8,8,8,8,8,8,9,9,9,9,9,9,9,9,10,10,10,10,10,10,10,10,11,11,11,11,11,11,11,11,12,12,12,12,12,12,12,12,13,13,13,13,13,13,13,13,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,14,15,16,17,18,19,20,21,22,22,22,22,22,22,22,22,23,23,23,23,23,23,23,23,24,24,24,24,24,24,24,24,25,25,25,25,25,25,25,25,26,26,26,26,26,26,26,26,27,27,27,27,27,27,27,27,28,28,28,28,28,28,28,28,29,29,29,29,29,29,29,29,30,31,32,33,34,30,31,32,33,34,30,31,32,33,34,30,31,32,33,34,30,31,32,33,34,30,31,32,33,34,30,31,32,33,34,30,31,32,33,34],"value":[45.9,46.8,43.1,38.1,41.1,35.8,45.8,45.8,13.4,13.3,16.9,16.2,16.1,24.6,4.3,14.1,12.1,9.4,13.2,10.6,7.7,5.4,28.7,11.4,6.4,5.8,2.8,6.9,6.6,6.4,2.8,13.2,8.5,8.1,3.1,3.8,7.2,6.8,4.5,2.8,25.5,31.4,40.5,43.8,39.2,38.1,24.5,21.5,59.7,45.7,40.8,35.3,48.9,26.7,21.8,36.6,7.6,14.3,16.7,22.5,10.1,19.3,23.4,10.6,9.8,9.7,12.1,16.2,11.2,17.6,9.8,8.8,4.6,10.1,5.1,5.6,4.2,8.7,4.7,5.5,7.6,4.5,5.5,3.8,15,0.6,6.6,9.8,20,29.7,36.7,30.2,20.4,49,63.1,53.5,46.9,34,30.2,22.2,35.2,17.5,19.7,30.8,2,6.3,3.6,3.3,0.5,1.3,2.8,6.5,7.6,7.7,11.7,9.8,11.2,10.1,11.5,8.1,15.8,14,14.3,21.3,20.9,24.2,12.9,13.1,3.4,3.3,4.2,5.4,2.5,6.8,2.5,4.4,8.9,7.8,5.2,7,12.2,8.4,4.5,8.4,7.8,15.3,20.2,21.5,4.6,21.7,25,15.9,6.8,11,11.7,11.8,9,6.6,23.3,9.5,37.3,52,41.5,26.7,13.1,1.4,3.4,2.5,4.4,4.1,9,5.7,10.3,10.4,11.2,17.3,14,14.8,18.7,18.1,2.7,2.6,4.3,4.7,5,11.6,8.5,9.4,7.2,3.4,7.8,8.9,7.7,18.5,29.3,12.5,4.5,9.8,9,15.2]},"nodes":{"name":["서울","경기/인천","대전/충청/세종","강원","부산/경남/울산","대구/경북","광주/전라","제주","민주당","새누리당","국민의당","바른정당","정의당","기타정당","사무직","노동직","가정주부","자영업","학생","농림어업","무직","기타직업","문재인","손학규","안철수","안희정","유승민","이재명","황교안","기타후보","19~29세","30대","40대","50대","60세이상"],"group":["서울","경기/인천","대전/충청/세종","강원","부산/경남/울산","대구/경북","광주/전라","제주","민주당","새누리당","국민의당","바른정당","정의당","기타정당","사무직","노동직","가정주부","자영업","학생","농림어업","무직","기타직업","문재인","손학규","안철수","안희정","유승민","이재명","황교안","기타후보","19~29세","30대","40대","50대","60세이상"]},"options":{"NodeID":"name","NodeGroup":"name","LinkGroup":null,"colourScale":"d3.scale.category20()","fontSize":12,"fontFamily":"NanumGothic","nodeWidth":30,"nodePadding":10,"units":"%","margin":{"top":null,"right":null,"bottom":null,"left":null},"iterations":32}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

# 데이터 과학자가 바라본 20대 총선




## 선거구

선거구 획정 관련 이슈

## 제20대 선거구 

제20대 선거구 데이터는 팀포퐁 [GitHub](https://github.com/southkorea/southkorea-maps)에서 가져온다. GeoJSON과 
Topo JSON 형식을 모두 지원한다.
`ogrListLayers` 함수를 통해서 사용할 `layer=`를 확인한다.


~~~{.r}
library(rgeos)
~~~



~~~{.output}
rgeos version: 0.3-17, (SVN revision 520)
 GEOS runtime version: 3.5.0-CAPI-1.9.0 r4084 
 Linking to sp version: 1.2-2 
 Polygon checking: TRUE 

~~~



~~~{.r}
library(rgdal) # gdal > 1.11.0
~~~



~~~{.output}
Loading required package: sp

~~~



~~~{.output}
rgdal: version: 1.1-3, (SVN revision 594)
 Geospatial Data Abstraction Library extensions to R successfully loaded
 Loaded GDAL runtime: GDAL 2.0.1, released 2015/09/15
 Path to GDAL shared files: C:/Users/KwangChun/Documents/R/win-library/3.2/rgdal/gdal
 GDAL does not use iconv for recoding strings.
 Loaded PROJ.4 runtime: Rel. 4.9.1, 04 March 2015, [PJ_VERSION: 491]
 Path to PROJ.4 shared files: C:/Users/KwangChun/Documents/R/win-library/3.2/rgdal/proj
 Linking to sp version: 1.2-1 

~~~



~~~{.r}
library(ggplot2)

getwd()
~~~

[1] "C:/Users/KwangChun/docs/politics"


~~~{.r}
ogrListLayers("../southkorea-maps/popong/precinct/assembly-precinct-20-geo.json")
~~~

[1] "precincts"
attr(,"driver")
[1] "GeoJSON"
attr(,"nlayers")
[1] 1


~~~{.r}
# precincts
 
ogrInfo("../southkorea-maps/popong/precinct/assembly-precinct-20-geo.json", "precincts")
~~~

Source: "../southkorea-maps/popong/precinct/assembly-precinct-20-geo.json", layer: "precincts"
Driver: GeoJSON; number of rows: 253 
Feature type: wkbPolygon with 2 dimensions
Extent: (124.6112 33.10857) - (131.8744 38.61427)
Number of fields: 4 
           name type length typeName
1            id    4      0   String
2 precinct_name    4      0   String
3      province    4      0   String
4   precinct_no    4      0   String


~~~{.r}
map <-  readOGR(dsn="../southkorea-maps/popong/precinct/assembly-precinct-20-geo.json", layer="precincts")
~~~

OGR data source with driver: GeoJSON 
Source: "../southkorea-maps/popong/precinct/assembly-precinct-20-geo.json", layer: "precincts"
with 253 features
It has 4 fields


~~~{.r}
plot(map)
~~~

<img src="fig/korea_precinct-1.png" title="" alt="" style="display: block; margin: auto;" />




#### 사전투표율

<img src="fig/pre-voting-survey.png" alt="사전 투표율" width="70%" />

|  구 분  |  제19대   | 제20대(예측) |  증감율 |
|---------|---------|-----------|--------|
| 전체    |  65.6%  |70.8%      |  5.2%  |
| 19-29세  |  48.2%  |63.3%      |  15.1% |
| 30대   |  64.2%  |70.5%      |  6.3%  |
| 40대   |  69.7%  |70.6%      |  0.9%  |
| 50대   |  71.9%  |73.5%      |  1.6%  |
| 60세이상  |  72.1% |74.4%      |  2.3%  |

### 선거구 현황

|  구 분  |  제19대   | 제20대(예측) |제20대(실제)|  증감율 |
|---------|---------|-----------|----------|-------|
| 전체    |       |         |          | 5.2%  |
| 19-29세  |       |         |          | 15.1% |
| 30대   |       |         |          | 6.3%  |
| 40대   |       |         |          | 0.9%  |
| 50대   |       |         |          | 1.6%  |
| 60세이상  |      |         |          | 2.3%  |

[제20대 국회의원선거 투표구별 개표결과 자료](http://www.nec.go.kr/portal/bbs/list/B0000338.do?menuNo=200061)




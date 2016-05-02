---
layout: page
title: 데이터 과학자가 바라본 20대 총선
subtitle: 인구 통계(demography)
output:
  html_document: 
    keep_md: yes
  pdf_document:
    latex_engine: xelatex
mainfont: NanumGothic
---




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
rgeos version: 0.3-11, (SVN revision 479)
 GEOS runtime version: 3.4.2-CAPI-1.8.2 r3921 
 Linking to sp version: 1.1-0 
 Polygon checking: TRUE 

~~~



~~~{.r}
library(rgdal) # gdal > 1.11.0
~~~



~~~{.output}
Loading required package: methods

~~~



~~~{.output}
Loading required package: sp

~~~



~~~{.output}
rgdal: version: 1.0-4, (SVN revision 548)
 Geospatial Data Abstraction Library extensions to R successfully loaded
 Loaded GDAL runtime: GDAL 1.11.2, released 2015/02/10
 Path to GDAL shared files: /Library/Frameworks/R.framework/Versions/3.2/Resources/library/rgdal/gdal
 Loaded PROJ.4 runtime: Rel. 4.9.1, 04 March 2015, [PJ_VERSION: 491]
 Path to PROJ.4 shared files: /Library/Frameworks/R.framework/Versions/3.2/Resources/library/rgdal/proj
 Linking to sp version: 1.1-1 

~~~



~~~{.r}
library(ggplot2)

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
Number of fields: 2 
  name type length typeName
1   id    4      0   String
2 name    4      0   String


~~~{.r}
map.20 <-  readOGR(dsn="../southkorea-maps/popong/precinct/assembly-precinct-20-geo.json", layer="precincts")
~~~

OGR data source with driver: GeoJSON 
Source: "../southkorea-maps/popong/precinct/assembly-precinct-20-geo.json", layer: "precincts"
with 253 features
It has 2 fields


~~~{.r}
map.19 <-  readOGR(dsn="../southkorea-maps/popong/precinct/assembly-precinct-19-geo.json", layer="precincts")
~~~

OGR data source with driver: GeoJSON 
Source: "../southkorea-maps/popong/precinct/assembly-precinct-19-geo.json", layer: "precincts"
with 246 features
It has 2 fields


~~~{.r}
par(mfrow = c(1, 2))  # 1 rows and 2 columns
plot(map.19)
plot(map.20)
~~~

<img src="fig/korea_precinct-1.png" title="plot of chunk korea_precinct" alt="plot of chunk korea_precinct" style="display: block; margin: auto;" />

출처: [Overcoming D3 Cartographic Envy With R + ggplot](https://rud.is/b/2014/09/26/overcoming-d3-cartographic-envy-with-r-ggplot/)


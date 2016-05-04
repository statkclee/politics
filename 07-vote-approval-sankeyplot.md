---
layout: page
title: 데이터 과학자가 바라본 20대 총선
subtitle: 여론조사 지지율 흐름 (Sankey diagram)
output:
  html_document: 
    keep_md: yes
  pdf_document:
    latex_engine: xelatex
mainfont: NanumGothic
---




## 생키 도해 

[생키 도해(Sankey diagram)](https://en.wikipedia.org/wiki/Sankey_diagram)는 흐름도(flow diagram)의 일종으로 처음에 증기엔진의 에너지 효율성을 도식화하는데 사용되었고, 화살표의 너비로 흐름의 양을 비율적으로 보여준다. 이를 처음 발명한 [Matthew Henry Phineas Riall Sankey](https://en.wikipedia.org/wiki/Matthew_Henry_Phineas_Riall_Sankey)를 따라 생키 도해(Sankey Diagram)로 부른다. 가장 유명한 사례로 나폴레옹의 러시아 공격을 도식화한 [Charles Joseph Minard](https://en.wikipedia.org/wiki/Charles_Joseph_Minard) 생키도해가 있다. 


<img src="fig/sankey-diagram-Minard.png" alt="생키 도해 사례 - 미나드" width="50%" />


## 여론조사 지지율 흐름 

구글 드라이브에 구글 스프레드쉬트에 데이터를 생성한다. 엑셀같은 설치형 소프트웨어가 갖는 한계를 클라우드 환경에서 
데이터 작업을 할 경우 여러가지로 장점을 갖는다. 
일단 데이터를 다음과 같은 형태로 작성하여 유권자를 투표참여층과 미투표층으로 나누고,
투표참여층과 투표미참여층에 대한 정당 지지도를 생키 도해를 통해 시각화한다.

| origin |  to    |   polls |
|--------|---------|--------|
| 유권자   |  투표참여  |  75.2% |
| 유권자   |  미투표   |  23.9% |
| 투표참여  | 새누리    |  76.1% |
| 투표참여  | 더민주    |  77.9% |
| 투표참여  | 국민의당   |  72.0% |
| 투표참여  | 지지정당없음 | 74.7% |
| 미투표   | 새누리    |  23.3% |
| 미투표   | 더민주    |  21.4% |
| 미투표   | 국민의당   |  23.9% |
| 미투표   | 지지정당없음 | 25.3% |

### 1. 데이터 가져오기 및 준비

최근에 발표된 `googlesheets` 팩키지를 설치하고 인증과정을 거치면 마치 로컬 컴퓨터에 
스프레드쉬트 파일이 있는 것처럼 가져다가 바로 사용할 수 있다.

`gs_ls()`를 호출하면 구글 인증과정을 거치고 나서, 구글 스프레드쉬트 파일을 가져오고,
여기에 포함된 워크쉬트를 데이터프레임으로 불러오는 개념을 이해하면 쉽다.


~~~{.r}
#================================================================
# 1. 데이터 가져오기
#================================================================
library(googlesheets)
glist <- gs_ls()
~~~



~~~{.output}
Auto-refreshing stale OAuth token.

~~~



~~~{.r}
# glist %>% glimpse()

t.dat <- gs_title("sankeyplot-data")
~~~



~~~{.output}
Sheet successfully identified: "sankeyplot-data"

~~~



~~~{.r}
v.dat <- gs_read(t.dat, ws='sample')
~~~



~~~{.output}
Accessing worksheet titled 'sample'.

~~~



~~~{.output}
No encoding supplied: defaulting to UTF-8.

~~~

### 2. 생키 도해 시각화

`googleVis`에 포함된 생키 도해 함수 `gvisSankey`를 사용하여 시각화한다.


~~~{.r}
library(googleVis)

plot(
  gvisSankey(v.dat, from="origin", 
             to="to", weight="polls",
             options=list(
               height=250,
               sankey="{link:{color:{fill:'lightblue'}},
               node:{
               color:{fill:'#a61d4c'},
               label: { fontName: 'nanum',
               fontSize: 9,
               color: '#871b47',
               bold: true,
               italic: true }
               },
               
               }"
             ))
)
~~~

<!-- Sankey generated in R 3.2.4 by googleVis 0.5.10 package -->
<!-- Wed May  4 13:47:34 2016 -->


<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataSankeyID4333f628b4a () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
 "유권자",
"투표참여",
75.2 
],
[
 "유권자",
"미투표",
23.9 
],
[
 "투표참여",
"새누리",
76.1 
],
[
 "투표참여",
"더민주",
77.9 
],
[
 "투표참여",
"국민의당",
72 
],
[
 "투표참여",
"지지정당없음",
74.7 
],
[
 "미투표",
"새누리",
23.3 
],
[
 "미투표",
"더민주",
21.4 
],
[
 "미투표",
"국민의당",
23.9 
],
[
 "미투표",
"지지정당없음",
25.3 
] 
];
data.addColumn('string','origin');
data.addColumn('string','to');
data.addColumn('number','polls');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartSankeyID4333f628b4a() {
var data = gvisDataSankeyID4333f628b4a();
var options = {};
options["width"] =    400;
options["height"] =    250;
options["sankey"] = {link:{color:{fill:'lightblue'}},
               node:{
               color:{fill:'#a61d4c'},
               label: { fontName: 'nanum',
               fontSize: 9,
               color: '#871b47',
               bold: true,
               italic: true }
               },
               
               };

    var chart = new google.visualization.Sankey(
    document.getElementById('SankeyID4333f628b4a')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "sankey";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartSankeyID4333f628b4a);
})();
function displayChartSankeyID4333f628b4a() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartSankeyID4333f628b4a"></script>
 
<!-- divChart -->
  
<div id="SankeyID4333f628b4a" 
  style="width: 400; height: 250;">
</div>

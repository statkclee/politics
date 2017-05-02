# 데이터 과학자가 바라본 20대 총선



## 네이버, 구글 뉴스 추세

제19대 대통령선거가 치뤄지고 있는 2017년 현재 기존 PC 패러다임의 강자 네이버와 안드로이드를 품고 있는 
모바일 강자 구글이 각자 분야에서 양강을 이루는 것은 확실하다. 즉, 이동할 때는 모바일 구글, 이동하지 않고 
자리를 잡고 있을 때는 PC 네이버가 확실한 경쟁우위를 갖추고 있다.

[구글 추세(Google Trends)](https://trends.google.com/) 서비스를 통해 사용자들이 가장 많이 검색하는 검색어를 통해 추세를 대세(?)를 살펴볼 수 있다. 
특히, 안드로이드, iOS 탑재된 스마트폰이 널리 보급되고 검색 인터페이스가 기존 입력방식에서 음성언어를 폭넓게 지원하고, 
인식률도 현저히 높아짐에 따라 사람들의 검색량도 증가하고 구글 추세 분석 결과도 신빙성을 높혀가고 있다.

그럼 구글 추세를 네이버에 적용이 가능할까? 

## 네이버 뉴스 추세(nTrend)

[네이버 뉴스 수집을 위한 도구](https://github.com/forkonlp/N2H4)를 활용하여 원데이터를 가져오고 나서,
해당기간 최대 뉴스에 대한 값을 최대값으로 정하고 이에 대한 상대적인 비율을 적용하여 추세정보를 생성한다.

### 최근 한달간 추세

17년 4월 1일부터 최근까지 한달간 추세는 다음과 같다.

<!--html_preserve--><div id="htmlwidget-16dcd0344ade487ebc56" style="width:1152px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-16dcd0344ade487ebc56">{"x":{"data":[{"x":[1491004800000,1491091200000,1491177600000,1491264000000,1491350400000,1491436800000,1491523200000,1491609600000,1491696000000,1491782400000,1491868800000,1491955200000,1492041600000,1492128000000,1492214400000,1492300800000,1492387200000,1492473600000,1492560000000,1492646400000,1492732800000,1492819200000,1492905600000,1492992000000,1493078400000,1493164800000,1493251200000,1493337600000,1493424000000,1493510400000,1493596800000],"y":[13.0942452043369,19.7873227689741,67.743953294412,69.5371142618849,46.1426188490409,56.1092577147623,54.7748123436197,19.7456213511259,26.8348623853211,67.4520433694746,63.4695579649708,65.8673894912427,68.6822351959967,59.1117597998332,23.1442869057548,46.0383653044203,88.3861551292744,84.5287739783153,80.6713928273561,100,65.5754795663052,40.4295246038365,53.6488740617181,86.3636363636364,73.2276897414512,91.3678065054212,91.8056713928274,67.535446205171,36.7389491242702,49.9582985821518,68.4945788156797],"text":["date: 2017-04-01<br>ntrend: 13.09<br>후보: 문재인","date: 2017-04-02<br>ntrend: 19.79<br>후보: 문재인","date: 2017-04-03<br>ntrend: 67.74<br>후보: 문재인","date: 2017-04-04<br>ntrend: 69.54<br>후보: 문재인","date: 2017-04-05<br>ntrend: 46.14<br>후보: 문재인","date: 2017-04-06<br>ntrend: 56.11<br>후보: 문재인","date: 2017-04-07<br>ntrend: 54.77<br>후보: 문재인","date: 2017-04-08<br>ntrend: 19.75<br>후보: 문재인","date: 2017-04-09<br>ntrend: 26.83<br>후보: 문재인","date: 2017-04-10<br>ntrend: 67.45<br>후보: 문재인","date: 2017-04-11<br>ntrend: 63.47<br>후보: 문재인","date: 2017-04-12<br>ntrend: 65.87<br>후보: 문재인","date: 2017-04-13<br>ntrend: 68.68<br>후보: 문재인","date: 2017-04-14<br>ntrend: 59.11<br>후보: 문재인","date: 2017-04-15<br>ntrend: 23.14<br>후보: 문재인","date: 2017-04-16<br>ntrend: 46.04<br>후보: 문재인","date: 2017-04-17<br>ntrend: 88.39<br>후보: 문재인","date: 2017-04-18<br>ntrend: 84.53<br>후보: 문재인","date: 2017-04-19<br>ntrend: 80.67<br>후보: 문재인","date: 2017-04-20<br>ntrend: 100<br>후보: 문재인","date: 2017-04-21<br>ntrend: 65.58<br>후보: 문재인","date: 2017-04-22<br>ntrend: 40.43<br>후보: 문재인","date: 2017-04-23<br>ntrend: 53.65<br>후보: 문재인","date: 2017-04-24<br>ntrend: 86.36<br>후보: 문재인","date: 2017-04-25<br>ntrend: 73.23<br>후보: 문재인","date: 2017-04-26<br>ntrend: 91.37<br>후보: 문재인","date: 2017-04-27<br>ntrend: 91.81<br>후보: 문재인","date: 2017-04-28<br>ntrend: 67.54<br>후보: 문재인","date: 2017-04-29<br>ntrend: 36.74<br>후보: 문재인","date: 2017-04-30<br>ntrend: 49.96<br>후보: 문재인","date: 2017-05-01<br>ntrend: 68.49<br>후보: 문재인"],"key":null,"type":"scatter","mode":"lines","name":"문재인","line":{"width":1.88976377952756,"color":"rgba(6,92,232,1)","dash":"solid"},"hoveron":"points","legendgroup":"문재인","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1491004800000,1491091200000,1491177600000,1491264000000,1491350400000,1491436800000,1491523200000,1491609600000,1491696000000,1491782400000,1491868800000,1491955200000,1492041600000,1492128000000,1492214400000,1492300800000,1492387200000,1492473600000,1492560000000,1492646400000,1492732800000,1492819200000,1492905600000,1492992000000,1493078400000,1493164800000,1493251200000,1493337600000,1493424000000,1493510400000,1493596800000],"y":[10.884070058382,12.8648874061718,31.9432860717264,27.5437864887406,31.7556296914095,26.3761467889908,26.1259382819016,10.9466221851543,16.0550458715596,36.0508757297748,27.1684737281068,30.045871559633,43.4528773978315,32.6313594662219,16.9516263552961,20.6839032527106,40.929941618015,34.674728940784,44.8498748957465,52.8356964136781,41.4512093411176,20.0166805671393,32.6105087572977,47.2060050041701,58.8198498748957,62.2810675562969,42.9107589658048,54.628857381151,22.8106755629691,34.1743119266055,55.2126772310259],"text":["date: 2017-04-01<br>ntrend: 10.88<br>후보: 홍준표","date: 2017-04-02<br>ntrend: 12.86<br>후보: 홍준표","date: 2017-04-03<br>ntrend: 31.94<br>후보: 홍준표","date: 2017-04-04<br>ntrend: 27.54<br>후보: 홍준표","date: 2017-04-05<br>ntrend: 31.76<br>후보: 홍준표","date: 2017-04-06<br>ntrend: 26.38<br>후보: 홍준표","date: 2017-04-07<br>ntrend: 26.13<br>후보: 홍준표","date: 2017-04-08<br>ntrend: 10.95<br>후보: 홍준표","date: 2017-04-09<br>ntrend: 16.06<br>후보: 홍준표","date: 2017-04-10<br>ntrend: 36.05<br>후보: 홍준표","date: 2017-04-11<br>ntrend: 27.17<br>후보: 홍준표","date: 2017-04-12<br>ntrend: 30.05<br>후보: 홍준표","date: 2017-04-13<br>ntrend: 43.45<br>후보: 홍준표","date: 2017-04-14<br>ntrend: 32.63<br>후보: 홍준표","date: 2017-04-15<br>ntrend: 16.95<br>후보: 홍준표","date: 2017-04-16<br>ntrend: 20.68<br>후보: 홍준표","date: 2017-04-17<br>ntrend: 40.93<br>후보: 홍준표","date: 2017-04-18<br>ntrend: 34.67<br>후보: 홍준표","date: 2017-04-19<br>ntrend: 44.85<br>후보: 홍준표","date: 2017-04-20<br>ntrend: 52.84<br>후보: 홍준표","date: 2017-04-21<br>ntrend: 41.45<br>후보: 홍준표","date: 2017-04-22<br>ntrend: 20.02<br>후보: 홍준표","date: 2017-04-23<br>ntrend: 32.61<br>후보: 홍준표","date: 2017-04-24<br>ntrend: 47.21<br>후보: 홍준표","date: 2017-04-25<br>ntrend: 58.82<br>후보: 홍준표","date: 2017-04-26<br>ntrend: 62.28<br>후보: 홍준표","date: 2017-04-27<br>ntrend: 42.91<br>후보: 홍준표","date: 2017-04-28<br>ntrend: 54.63<br>후보: 홍준표","date: 2017-04-29<br>ntrend: 22.81<br>후보: 홍준표","date: 2017-04-30<br>ntrend: 34.17<br>후보: 홍준표","date: 2017-05-01<br>ntrend: 55.21<br>후보: 홍준표"],"key":null,"type":"scatter","mode":"lines","name":"홍준표","line":{"width":1.88976377952756,"color":"rgba(255,0,0,1)","dash":"solid"},"hoveron":"points","legendgroup":"홍준표","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1491004800000,1491091200000,1491177600000,1491264000000,1491350400000,1491436800000,1491523200000,1491609600000,1491696000000,1491782400000,1491868800000,1491955200000,1492041600000,1492128000000,1492214400000,1492300800000,1492387200000,1492473600000,1492560000000,1492646400000,1492732800000,1492819200000,1492905600000,1492992000000,1493078400000,1493164800000,1493251200000,1493337600000,1493424000000,1493510400000,1493596800000],"y":[12.7189324437031,18.6613844870726,29.4620517097581,52.1684737281068,60.0500417014178,51.4595496246872,48.4153461217681,14.0325271059216,27.4395329441201,54.4620517097581,54.628857381151,63.5946622185154,65.4712260216847,52.9399499582986,18.95329441201,38.6155129274395,81.0884070058382,69.6205170975813,65.4920767306088,81.9641367806505,58.4236864053378,28.8573811509591,48.0400333611343,72.1017514595496,67.743953294412,76.1884904086739,65.6380316930776,63.1567973311093,28.9824854045038,51.7931609674729,55.2752293577982],"text":["date: 2017-04-01<br>ntrend: 12.72<br>후보: 안철수","date: 2017-04-02<br>ntrend: 18.66<br>후보: 안철수","date: 2017-04-03<br>ntrend: 29.46<br>후보: 안철수","date: 2017-04-04<br>ntrend: 52.17<br>후보: 안철수","date: 2017-04-05<br>ntrend: 60.05<br>후보: 안철수","date: 2017-04-06<br>ntrend: 51.46<br>후보: 안철수","date: 2017-04-07<br>ntrend: 48.42<br>후보: 안철수","date: 2017-04-08<br>ntrend: 14.03<br>후보: 안철수","date: 2017-04-09<br>ntrend: 27.44<br>후보: 안철수","date: 2017-04-10<br>ntrend: 54.46<br>후보: 안철수","date: 2017-04-11<br>ntrend: 54.63<br>후보: 안철수","date: 2017-04-12<br>ntrend: 63.59<br>후보: 안철수","date: 2017-04-13<br>ntrend: 65.47<br>후보: 안철수","date: 2017-04-14<br>ntrend: 52.94<br>후보: 안철수","date: 2017-04-15<br>ntrend: 18.95<br>후보: 안철수","date: 2017-04-16<br>ntrend: 38.62<br>후보: 안철수","date: 2017-04-17<br>ntrend: 81.09<br>후보: 안철수","date: 2017-04-18<br>ntrend: 69.62<br>후보: 안철수","date: 2017-04-19<br>ntrend: 65.49<br>후보: 안철수","date: 2017-04-20<br>ntrend: 81.96<br>후보: 안철수","date: 2017-04-21<br>ntrend: 58.42<br>후보: 안철수","date: 2017-04-22<br>ntrend: 28.86<br>후보: 안철수","date: 2017-04-23<br>ntrend: 48.04<br>후보: 안철수","date: 2017-04-24<br>ntrend: 72.1<br>후보: 안철수","date: 2017-04-25<br>ntrend: 67.74<br>후보: 안철수","date: 2017-04-26<br>ntrend: 76.19<br>후보: 안철수","date: 2017-04-27<br>ntrend: 65.64<br>후보: 안철수","date: 2017-04-28<br>ntrend: 63.16<br>후보: 안철수","date: 2017-04-29<br>ntrend: 28.98<br>후보: 안철수","date: 2017-04-30<br>ntrend: 51.79<br>후보: 안철수","date: 2017-05-01<br>ntrend: 55.28<br>후보: 안철수"],"key":null,"type":"scatter","mode":"lines","name":"안철수","line":{"width":1.88976377952756,"color":"rgba(7,249,31,1)","dash":"solid"},"hoveron":"points","legendgroup":"안철수","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1491004800000,1491091200000,1491177600000,1491264000000,1491350400000,1491436800000,1491523200000,1491609600000,1491696000000,1491782400000,1491868800000,1491955200000,1492041600000,1492128000000,1492214400000,1492300800000,1492387200000,1492473600000,1492560000000,1492646400000,1492732800000,1492819200000,1492905600000,1492992000000,1493078400000,1493164800000,1493251200000,1493337600000,1493424000000,1493510400000,1493596800000],"y":[7.36030025020851,9.88323603002502,21.5596330275229,21.2677231025855,23.6655546288574,19.1201000834028,19.557964970809,8.42368640533778,15.1376146788991,21.2885738115096,16.4929107589658,16.8682235195997,33.5904920767306,32.3811509591326,14.4495412844037,26.3552960800667,35.4462051709758,25.6672226855713,36.1342785654712,50.4378648874062,32.7356130108424,13.4695579649708,30.1918265221017,43.5779816513761,52.5437864887406,47.5396163469558,29.3994995829858,43.0150125104254,15.116763969975,18.6405337781485,28.6488740617181],"text":["date: 2017-04-01<br>ntrend: 7.36<br>후보: 유승민","date: 2017-04-02<br>ntrend: 9.88<br>후보: 유승민","date: 2017-04-03<br>ntrend: 21.56<br>후보: 유승민","date: 2017-04-04<br>ntrend: 21.27<br>후보: 유승민","date: 2017-04-05<br>ntrend: 23.67<br>후보: 유승민","date: 2017-04-06<br>ntrend: 19.12<br>후보: 유승민","date: 2017-04-07<br>ntrend: 19.56<br>후보: 유승민","date: 2017-04-08<br>ntrend: 8.42<br>후보: 유승민","date: 2017-04-09<br>ntrend: 15.14<br>후보: 유승민","date: 2017-04-10<br>ntrend: 21.29<br>후보: 유승민","date: 2017-04-11<br>ntrend: 16.49<br>후보: 유승민","date: 2017-04-12<br>ntrend: 16.87<br>후보: 유승민","date: 2017-04-13<br>ntrend: 33.59<br>후보: 유승민","date: 2017-04-14<br>ntrend: 32.38<br>후보: 유승민","date: 2017-04-15<br>ntrend: 14.45<br>후보: 유승민","date: 2017-04-16<br>ntrend: 26.36<br>후보: 유승민","date: 2017-04-17<br>ntrend: 35.45<br>후보: 유승민","date: 2017-04-18<br>ntrend: 25.67<br>후보: 유승민","date: 2017-04-19<br>ntrend: 36.13<br>후보: 유승민","date: 2017-04-20<br>ntrend: 50.44<br>후보: 유승민","date: 2017-04-21<br>ntrend: 32.74<br>후보: 유승민","date: 2017-04-22<br>ntrend: 13.47<br>후보: 유승민","date: 2017-04-23<br>ntrend: 30.19<br>후보: 유승민","date: 2017-04-24<br>ntrend: 43.58<br>후보: 유승민","date: 2017-04-25<br>ntrend: 52.54<br>후보: 유승민","date: 2017-04-26<br>ntrend: 47.54<br>후보: 유승민","date: 2017-04-27<br>ntrend: 29.4<br>후보: 유승민","date: 2017-04-28<br>ntrend: 43.02<br>후보: 유승민","date: 2017-04-29<br>ntrend: 15.12<br>후보: 유승민","date: 2017-04-30<br>ntrend: 18.64<br>후보: 유승민","date: 2017-05-01<br>ntrend: 28.65<br>후보: 유승민"],"key":null,"type":"scatter","mode":"lines","name":"유승민","line":{"width":1.88976377952756,"color":"rgba(0,250,255,1)","dash":"solid"},"hoveron":"points","legendgroup":"유승민","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1491004800000,1491091200000,1491177600000,1491264000000,1491350400000,1491436800000,1491523200000,1491609600000,1491696000000,1491782400000,1491868800000,1491955200000,1492041600000,1492128000000,1492214400000,1492300800000,1492387200000,1492473600000,1492560000000,1492646400000,1492732800000,1492819200000,1492905600000,1492992000000,1493078400000,1493164800000,1493251200000,1493337600000,1493424000000,1493510400000,1493596800000],"y":[3.56547122602168,5.23352793994996,10.5713094245204,8.61134278565471,9.09090909090909,9.46622185154295,10.1751459549625,3.16930775646372,7.27689741451209,12.0308590492077,10.5296080066722,21.2051709758132,31.6722268557131,22.6021684737281,9.38281901584654,20.8090075062552,29.5663052543787,19.76647206005,32.1726438698916,35.4670558798999,20.0166805671393,6.63052543786489,21.7472894078399,31.1926605504587,36.4470391993328,35.070892410342,26.4804003336113,37.5729774812344,11.3219349457882,14.6997497914929,27.5020850708924],"text":["date: 2017-04-01<br>ntrend: 3.57<br>후보: 심상정","date: 2017-04-02<br>ntrend: 5.23<br>후보: 심상정","date: 2017-04-03<br>ntrend: 10.57<br>후보: 심상정","date: 2017-04-04<br>ntrend: 8.61<br>후보: 심상정","date: 2017-04-05<br>ntrend: 9.09<br>후보: 심상정","date: 2017-04-06<br>ntrend: 9.47<br>후보: 심상정","date: 2017-04-07<br>ntrend: 10.18<br>후보: 심상정","date: 2017-04-08<br>ntrend: 3.17<br>후보: 심상정","date: 2017-04-09<br>ntrend: 7.28<br>후보: 심상정","date: 2017-04-10<br>ntrend: 12.03<br>후보: 심상정","date: 2017-04-11<br>ntrend: 10.53<br>후보: 심상정","date: 2017-04-12<br>ntrend: 21.21<br>후보: 심상정","date: 2017-04-13<br>ntrend: 31.67<br>후보: 심상정","date: 2017-04-14<br>ntrend: 22.6<br>후보: 심상정","date: 2017-04-15<br>ntrend: 9.38<br>후보: 심상정","date: 2017-04-16<br>ntrend: 20.81<br>후보: 심상정","date: 2017-04-17<br>ntrend: 29.57<br>후보: 심상정","date: 2017-04-18<br>ntrend: 19.77<br>후보: 심상정","date: 2017-04-19<br>ntrend: 32.17<br>후보: 심상정","date: 2017-04-20<br>ntrend: 35.47<br>후보: 심상정","date: 2017-04-21<br>ntrend: 20.02<br>후보: 심상정","date: 2017-04-22<br>ntrend: 6.63<br>후보: 심상정","date: 2017-04-23<br>ntrend: 21.75<br>후보: 심상정","date: 2017-04-24<br>ntrend: 31.19<br>후보: 심상정","date: 2017-04-25<br>ntrend: 36.45<br>후보: 심상정","date: 2017-04-26<br>ntrend: 35.07<br>후보: 심상정","date: 2017-04-27<br>ntrend: 26.48<br>후보: 심상정","date: 2017-04-28<br>ntrend: 37.57<br>후보: 심상정","date: 2017-04-29<br>ntrend: 11.32<br>후보: 심상정","date: 2017-04-30<br>ntrend: 14.7<br>후보: 심상정","date: 2017-05-01<br>ntrend: 27.5<br>후보: 심상정"],"key":null,"type":"scatter","mode":"lines","name":"심상정","line":{"width":1.88976377952756,"color":"rgba(242,238,9,1)","dash":"solid"},"hoveron":"points","legendgroup":"심상정","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1492560000000,1492560000000],"y":[-1.85,104.85],"text":"xintercept: 17275","key":null,"type":"scatter","mode":"lines","name":"","line":{"width":1.88976377952756,"color":"rgba(169,169,169,1)","dash":"dashdot"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1492905600000,1492905600000],"y":[-1.85,104.85],"text":"xintercept: 17279","key":null,"type":"scatter","mode":"lines","name":"","line":{"width":1.88976377952756,"color":"rgba(169,169,169,1)","dash":"dashdot"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1493078400000,1493078400000],"y":[-1.85,104.85],"text":"xintercept: 17281","key":null,"type":"scatter","mode":"lines","name":"","line":{"width":1.88976377952756,"color":"rgba(169,169,169,1)","dash":"dashdot"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1493337600000,1493337600000],"y":[-1.85,104.85],"text":"xintercept: 17284","key":null,"type":"scatter","mode":"lines","name":"","line":{"width":1.88976377952756,"color":"rgba(169,169,169,1)","dash":"dashdot"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1492560000000],"y":[3],"text":"KBS","key":null,"textfont":{"size":11.3385826771654,"color":"rgba(0,0,0,1)"},"type":"scatter","mode":"text","hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","name":""},{"x":[1492905600000],"y":[3],"text":"1차","key":null,"textfont":{"size":11.3385826771654,"color":"rgba(0,0,0,1)"},"type":"scatter","mode":"text","hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","name":""},{"x":[1493078400000],"y":[3],"text":"JTBC","key":null,"textfont":{"size":11.3385826771654,"color":"rgba(0,0,0,1)"},"type":"scatter","mode":"text","hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","name":""},{"x":[1493337600000],"y":[3],"text":"2차","key":null,"textfont":{"size":11.3385826771654,"color":"rgba(0,0,0,1)"},"type":"scatter","mode":"text","hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","name":""}],"layout":{"margin":{"t":51.0684931506849,"r":7.30593607305936,"b":31.1498547114985,"l":25.9028642590286},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":"후보별 네이버 뉴스검색 추세","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"xaxis":{"domain":[0,1],"type":"linear","autorange":false,"tickmode":"array","range":[1490875200000,1493726400000],"ticktext":["04-03","04-10","04-17","04-24","05-01"],"tickvals":[1491177600000,1491782400000,1492387200000,1492992000000,1493596800000],"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":9.9626400996264},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":"","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"type":"linear","autorange":false,"tickmode":"array","range":[-1.85,104.85],"ticktext":["0","25","50","75","100"],"tickvals":[2.22044604925031e-016,25,50,75,100],"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":9.9626400996264},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":"","titlefont":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"hovermode":"closest"},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"modeBarButtonsToRemove":["sendDataToCloud"]},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve--><img src="fig/ntrend-recent-2.png" style="display: block; margin: auto;" /><!--html_preserve--><div id="htmlwidget-8cd2ef22413e6e332485" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-8cd2ef22413e6e332485">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31"],["2017-04-01","2017-04-02","2017-04-03","2017-04-04","2017-04-05","2017-04-06","2017-04-07","2017-04-08","2017-04-09","2017-04-10","2017-04-11","2017-04-12","2017-04-13","2017-04-14","2017-04-15","2017-04-16","2017-04-17","2017-04-18","2017-04-19","2017-04-20","2017-04-21","2017-04-22","2017-04-23","2017-04-24","2017-04-25","2017-04-26","2017-04-27","2017-04-28","2017-04-29","2017-04-30","2017-05-01"],[628,949,3249,3335,2213,2691,2627,947,1287,3235,3044,3159,3294,2835,1110,2208,4239,4054,3869,4796,3145,1939,2573,4142,3512,4382,4403,3239,1762,2396,3285],[522,617,1532,1321,1523,1265,1253,525,770,1729,1303,1441,2084,1565,813,992,1963,1663,2151,2534,1988,960,1564,2264,2821,2987,2058,2620,1094,1639,2648],[610,895,1413,2502,2880,2468,2322,673,1316,2612,2620,3050,3140,2539,909,1852,3889,3339,3141,3931,2802,1384,2304,3458,3249,3654,3148,3029,1390,2484,2651],[353,474,1034,1020,1135,917,938,404,726,1021,791,809,1611,1553,693,1264,1700,1231,1733,2419,1570,646,1448,2090,2520,2280,1410,2063,725,894,1374],[171,251,507,413,436,454,488,152,349,577,505,1017,1519,1084,450,998,1418,948,1543,1701,960,318,1043,1496,1748,1682,1270,1802,543,705,1319]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>date<\/th>\n      <th>문재인<\/th>\n      <th>홍준표<\/th>\n      <th>안철수<\/th>\n      <th>유승민<\/th>\n      <th>심상정<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[2,3,4,5,6]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"rowCallback":"function(row, data) {\nDTWidget.formatCurrency(this, row, data, 2, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 3, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 4, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 5, '', 0, 3, ',', '.', true);\nDTWidget.formatCurrency(this, row, data, 6, '', 0, 3, ',', '.', true);\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->




~~~{.r}
# 0. 환경설정 ---------------------------------------------------------------
# if (!require("devtools")) install.packages("devtools")
# devtools::install_github("forkonlp/N2H4")
library(N2H4)
library(ggplot2)
library(tidyverse)
library(plotly)
library(lubridate)

# 1. 데이터 긁어오기 --------------------------------------------------------

start_date<-"2017-04-01"
end_date<-"2017-05-01"

candidates <- c("문재인", "홍준표", "안철수", "유승민", "심상정")

trend_df <- c()

for(candidate in candidates){
  print(candidate)
  tmp <- getNewsTrend(candidate, start_date, end_date)
  trend_df <- rbind(trend_df, cbind(data.frame(`후보`=candidate,tmp)))
}

# 2. 데이터 정제 --------------------------------------------------------

trends_df <- trend_df %>% mutate(global_max = max(cnt)) %>% 
  group_by(date) %>% 
  mutate(ntrend = cnt / global_max *100)

trends_hist_df <- trend_df %>% group_by(후보) %>% 
  summarise(tlt_cnt = sum(cnt)) %>% 
  mutate(cnt_pcnt = tlt_cnt/sum(tlt_cnt)) %>% 
  mutate(후보=factor(후보, levels=c("문재인","홍준표","안철수","유승민","심상정")))

# 3. 데이터 시각화 --------------------------------------------------------

ntrend_g <- ggplot(trends_df,aes(x=date, y=ntrend, color=후보)) +
  geom_line() +
  scale_x_date(date_labels="%m-%d") +
  theme_minimal() +
  theme(legend.position="none", 
        plot.caption=element_text(hjust=0,size=8),
        plot.subtitle=element_text(face="italic"),
        axis.text=element_text(size=7.5))+
  labs(x="",y="",title="후보별 네이버 뉴스검색 추세",
       caption="\n 데이터출처: 네이버, 네이버 뉴스 수집 도구(https://github.com/forkonlp/N2H4/)",
       subtitle="") +
  scale_colour_manual(name="",
                      values=c(문재인="#065ce8", 
                               홍준표="#ff0000",
                               안철수="#07f91f",
                               유승민="#00faff",
                               심상정="#f2ee09")) +
  geom_vline(xintercept=as.numeric(ymd("2017-04-19")), linetype=4, color="darkgrey", size=0.5) +
  geom_vline(xintercept=as.numeric(ymd("2017-04-23")), linetype=4, color="darkgrey", size=0.5) +
  geom_vline(xintercept=as.numeric(ymd("2017-04-25")), linetype=4, color="darkgrey", size=0.5) +
  geom_vline(xintercept=as.numeric(ymd("2017-04-28")), linetype=4, color="darkgrey", size=0.5) +
  annotate(geom="text",x=as.Date("2017-04-19"), y=3,label="KBS",fontface="bold", size=3) +
  annotate(geom="text",x=as.Date("2017-04-23"), y=3,label="1차",fontface="bold", size=3) +
  annotate(geom="text",x=as.Date("2017-04-25"), y=3,label="JTBC",fontface="bold", size=3) +
  annotate(geom="text",x=as.Date("2017-04-28"), y=3,label="2차",fontface="bold", size=3)

ntrend_hist_g <- ggplot(trends_hist_df, aes(x=후보, y=tlt_cnt)) +
  geom_bar(stat="identity", data=subset(trends_hist_df, 후보=="문재인"), fill="#065ce8") +
  geom_bar(stat="identity", data=subset(trends_hist_df, 후보=="홍준표"), fill="#ff0000") +
  geom_bar(stat="identity", data=subset(trends_hist_df, 후보=="안철수"), fill="#07f91f") +
  geom_bar(stat="identity", data=subset(trends_hist_df, 후보=="유승민"), fill="#00faff") +
  geom_bar(stat="identity", data=subset(trends_hist_df, 후보=="심상정"), fill="#f2ee09") +
  theme_minimal() +
  theme(legend.position="none", 
        plot.caption=element_text(hjust=0,size=8),
        plot.subtitle=element_text(face="italic"),
        axis.text=element_text(size=7.5))+
  labs(x="",y="",title="후보별 네이버 뉴스검색 추세",
       caption="\n 데이터출처: 네이버, 네이버 뉴스 수집 도구(https://github.com/forkonlp/N2H4/)",
       subtitle="") +
  geom_text(data=trends_hist_df,aes(x=후보,y=tlt_cnt, label=scales::percent(cnt_pcnt)),vjust=0)

# 4. 표 --------------------------------------------------------------

DT::datatable(trends_df %>% dplyr::select(후보, date, cnt) %>% 
                spread(후보, cnt)) %>% 
  DT::formatCurrency(c(2:6), "", interval=3, mark=',', digits=0)
~~~


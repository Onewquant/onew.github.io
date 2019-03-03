---
layout: single
title: "시가 갭과 당일 주가 변동 방향과의 상관관계 (Correlation between opening gap and the direction of price movement in Korean stock market)"
author_profile: false
tag: 
- korean stock market
- correlation analysis
- heat map analysis
categories: 
- Basic Research
---
  
한국 개별 주식 종목 몇 개를 선정해 당일 종가에 매수, 다음날 시가에 매도하는 오버나잇 갭 전략을 만들어보는 중에 한 가지 특이한 패턴을 발견했다. 일단, 전략을 만들기 위해 접근한 방식은 전략에서 수익이 어떻게 나게 되는지 이해하고, 그 부분에 영향을 주는 요소들을 분석 하였다. 전략의 수익은 두 가지 요소로 결정된다.   
 
(1) 진입할 종목을 어떻게 선정해야 승률이 높은가? (승률)  
(2) Trading Fee(거래세, 수수료, 슬리피지 등의 합)를 고려하더라도 시가갭의 크기가 충분히 큰가? (손익비)  

(2) 번은 일단 제쳐두고, (1)번을 풀어보기 위해 여러가지 요인들(Input Variables)과 다음날 시가갭이 어떤 관계가 있는지 살펴보았다.   
 
![heat_map_2017](https://user-images.githubusercontent.com/34860302/53695354-d3be6000-3dfd-11e9-93fb-841ebb6afe03.png)  
 
##### (Figure 1) Heat map of various factors #####   
  
2017년 01월 01일부터 2019년 02월 28일 까지의 상장 종목(KOSPI & KOSDAQ) 가격데이터로 상관행렬(Correlation matrix)을 구해 Heat map diagram을 그려보았는데, 보다시피 시가갭을 미리 예측할 수 있게 해주는 요인은 보이지 않았다. 그런데, 한 가지 특이한 점은 시가 갭과 그 갭이 있던 날의 주가움직임 (종가 - 시가)이 약한 음의 상관성을 보인다는 것이다.  
  
즉 시가 갭이 (+)인 날은 대체로 당일 종가가 시가보다 낮고(장중 주가 하락), 반대로 시가 갭이 (-)인 날은 대체로 종가가 시가보다 높았다(장중 주가 상승). 물론 강한 상관성은 아니라 ‘대체로’라는 말이 어울리지 않을지도 모르지만…  
  
![next_opening_gap_net_ret vs next_day_fluctuation](https://user-images.githubusercontent.com/34860302/53695474-83e09880-3dff-11e9-8898-255ab15f68b3.png)  
 
##### (Figure 2) Distribution diagram for 2 years (2017.01.01 ~ 2019.02.28) #####  
 
상관계수 : -0.184590  
  
![next_opening_gap_net_ret vs next_day_fluctuation 20y](https://user-images.githubusercontent.com/34860302/53695562-7bd52880-3e00-11e9-89a4-c9b9f5f596d0.png)   
  
##### (Figure 3) Distribution diagram for 20 years (2000.01.01 ~ 2019.02.28) #####   
 
상관계수 : -0.316227   

원래 풀려고 했던 문제와 관련된 성질은 아니지만 새로운 전략을 만드는데 충분히 응용해볼 수 있는 패턴으로 보인다. 거래자들의 평균회귀적 성향이 나타나는게 아닐까?  
 
## Reference ##    
(Programming Language) Python 3.66   
(Data Source) Curvelib / KOR Stock Price Data (2000.01.01 ~ 2019.01.22) 한국 주식 전종목   
(Figure 1,2,3) 자체 제작   
 
## 데이터 처리 특이사항 ##  
(1) 수정주가를 사용하지 않음  
(2) Opening gap 이 -30% 미만, 혹은 30% 초과인 데이터는 제거함  
(3) 주가가 2500원 미만인 주식 종목은 제외함  
(4) next_opening_gap_net_ret : 100 * (Next_Day_Open - Today_Close) / Today_Close - 0.5
(5) next_day_fluctuation : 100 * (Next_Day_Close - Next_Day_Open) / Next_Day_Open
  

---
layout: single
title: "한국 주식시장에서의 멱함수 분포 (Power law distribution in Korean stock market)"
author_profile: false
tag: 
- complex system
- network theory
- korean stock market
- power law
- linear regression
categories: 
- Basic Research
---

최근 복잡계 네트워크 이론에 관한 책을 계속 읽고 있다. 지진, 산불, 전쟁의 규모, 새의 비행시간, 사람이 편지를 보내는 시간간격 등 상상을 초월할 정도로 다양한 곳에 멱함수 법칙이 존재함을 알게 되었다. 그리고 궁금해졌다.  

> 요즘 내가 다루고 있는 한국 주식시장 데이터에서도 정말 멱함수 법칙이 나타날까?  
 
바로 확인에 들어갔다.  
  
결과는?  
 
![kor_stocks_power_law](https://user-images.githubusercontent.com/34860302/51611300-4405d600-1f62-11e9-9e09-e86d57d2e2c4.png)  
 
##### (Figure 1) Power law distribution of Korean Stocks #####  
 
완벽할 정도로 멱함수 법칙을 따르고 있었다.  
 
그래프 보는 방법을 짧게 설명하자면,  
 
x축은 어제 종가에 비교했을때 오늘 종가가 얼마나 변했는지를 나타내는 수익률 값(%)이다. 예를 들어, 어제 종가가 1000원인 주식 A가 있다고 하자. A의 오늘 종가가 1100원이면 수익률 값은 100*(1100-1000)/1000 = 10% 이다. 따라서 A는 붉은색 막대들이 있는 Positive Returns그래프에서 x = 10 의 막대에 하나를 보탰을 것이다. 이런식으로 2000년 1월 1일부터 2019년 1월 22일까지 한국 주식시장에 있는 모든 개별종목의 일간 수익률을 1% 단위로 분류하고 각각 몇 개가 관찰되는지 세어 보았다. (내가 직접 센 것은 아니고 컴퓨터가…)  
그랬더니 이런 아름다운 멱함수 분포가 나타났다.  
  
그런데 16부터는 막대가 안보인다. 이는 숫자크기가 작아서 그렇다. 이들도 확실히 보고싶어 발생 횟수(y축)에 로그값을 취해보았다.  
 
![kor_stocks_power_law_logarithm](https://user-images.githubusercontent.com/34860302/51611312-4a944d80-1f62-11e9-982b-80d3b5c020b9.png)  
 
##### (Figure 2) Power law distribution of Korean Stocks in logarithm function #####  
 
거의 선형에 가까운 그래프를 얻을 수 있었다. 정말 멱함수 맞구나 !  
산점도(Scatter graph)로 바꾸고 선형회귀분석을 통해 회귀선도 구할 수 있다. <sup>content 1</sup>  
 
![linear_regression_pos](https://user-images.githubusercontent.com/34860302/51611848-90054a80-1f63-11e9-8e66-8b89900b1f4e.png)  
![linear_regression_neg](https://user-images.githubusercontent.com/34860302/51611851-9267a480-1f63-11e9-869d-1b13dcb7125c.png)  
 
##### (Figure 3,4) Power law distribution in scattering graph #####  
 
> Positive Returns : Y = -0.3377X + 13.8954  
> Negative Returns : Y = 0.3646X + 13.9624  
 
기울기가 +, - 로 부호가 다른 것 빼고는 식이 거의 같다고 볼 수 있다. 결국 수익률의 절대값(변동성)이 멱함수 법칙을 따른다고 볼 수 있겠다.  
 
한편, 더 자세히 들여다보면 재미있는 현상이 몇 가지 있다.  
먼저, +15% 혹은 -15%에서 그래프의 기울기가 한 번 변하는 것 같다. 산점도(Scatter graph)에서 회귀선을 빼보면 이 현상이 더 뚜렷하게 보인다.  
 
![kor_stocks_power_low_logarithm_scatter_neg](https://user-images.githubusercontent.com/34860302/51611679-2be28680-1f63-11e9-829e-fe3251e489b1.png)  
![kor_stocks_power_low_logarithm_scatter_pos](https://user-images.githubusercontent.com/34860302/51611685-30a73a80-1f63-11e9-85f7-e2ec8ec09c40.png)  
 
##### (Figure 5,6) Power law distribution without linear regression line #####  

물론 그냥 뭉뚱그려 생각해도 큰 무리는 없어보이지만, 가운데 쯤이 뭔가 변화가 있어보이지 않는가? Positive Return과 Negative Return그래프 둘다에서 볼 수 있듯, 수익률 절대값이 15% 이하인 데이터의 그래프 기울기가 나머지 데이터 그래프 기울기보다 더 가파르다. 또한 15% 근처에서는 마치 방울을 달아놓은 것 같은 모양의 분포가 있다. 서로 다른 두 속성의 그래프를 떡하니 구분해주는 표시같다. 왜 이런 분포들이 생기는 것일까? 15%라는 숫자에 특별한 의미가 있는 것인가?  
 
또 한 가지 특이한 점은 상한가와 하한가부근 즉 +30%, -30% 수익률을 보이는 횟수가 그 직전 횟수들보다 상대적으로 훨씬 큰 편이라는 것이다. 즉, 27.XX % 혹은 28.XX %의 수익률보다 29.XX % 수익률이 훨씬 더 자주 나타났다. 이건 무슨 의미일까? 상한가와 하한가라는 상징적인 의미가 실제 분포에도 반영되는 것일까? 상한가와 하한가를 넘어 있어야 할 분포 숫자들의 합일까? 아니면, 또 다른 속성을 가진 분포일까?  
 
흥미로운 현상들을 보며 궁금증만 더욱 쌓여간다.   
 
왜 그런지도 차차 알게 되겠지?  
 
## Reference ##  
(Programming Language) Python 3.66  
(Data Source) Curvelib / KOR Stock Price Data (2000.01.01 ~ 2019.01.22) 한국 주식 전종목  
(content 1) scipy 라이브러리의 stats.linregress 사용  
(Figure 1,2,3,4,5,6) 자체 제작  






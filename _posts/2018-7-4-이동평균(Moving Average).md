---
title: 이동평균(Moving Average) - SMA, EMA, LWMA···
categories: MISC
tags: moving average, SMA, EMA, LWMA
---

http://blog.naver.com/PostView.nhn?blogId=swatpjs&logNo=220256081171

http://www.analog.com/media/en/technical-documentation/dsp-book/dsp_book_Ch15.pdf

하드웨어 신호처리중 이동평균과 관련해서 몇가지를 정리한 것이다.

이동평균(Moving Average)이란 2개 이상의 연속된 데이터 값의 평균을 연속적으로 계산해내는 평균화 방법이다.

![](http://ktword.co.kr/img_data/3665_1.JPG)

일반적으로 처음부터 끝까지, 전 구간을 대상으로 평균을 내기에는 정확도가 떨어지므로, 평균을 산출할 데이터의 개수(지난 2시간의 데이터, 최신 데이터 200개···)를 정한 후, 그 데이터로만 평균을 낸다.


### SMA(Simple Moving Average) - 단순이동평균
단순히 주어진 구간에서의 평균을 산출하는 계산방법이다.


### LWMA(Linear Weighted Moving Average) - 선형가중이동평균
LWMA와 WMA의 차이는 조금 더 봐야한다. 정확하지 않다.

SMA의 둔한 반응속도를 보완할 수 있는 계산방법으로, 오래된 값이라도 완전히 무시하지는 않고 반영시키나 가중치를 낮추는 계산방식을 사용하며, 실제로는 거의 사용하지 않는다

예를 들어 10개의 데이터를 대상으로 EMA를 구한다면, 가장 최근의 데이터에는 10을 곱해주고, 그 다음 데이터에는 9를 곱해주는 식으로 합을 구해서 데이터의 개수로 나눠준다.


### EMA(Exponential Moving Average) - 지수이동평균
최근의 데이터에 더 높은 가중치를 부여하여 오래된 값의 가중치를 낮추는 방법으로, LWMA보다 더 반응속도가 빠르다.

EMA의 공식은 아래와 같다.

$$
{EMA}_1 = data_1
$$

$$
{EMA}_t = \alpha \times data_t + (1-\alpha) \times {EMA}_{t-1}
$$

data_t는 t 시점에서의 입력 데이터값이다.
α는 (0,1) 에서 정의된 상수로, 가중치, 소멸계수 또는 감소인자라고 부른다. 
α의 값이 높을수록 과거의 데이터를 적게 반영해 더 민감하게 반응하며, α의 값이 낮을떄는 그 반대이다.

$$
\alpha = \frac{2}{n+1}
$$

value는 데이터값, i는 시점, n은 이동평균기간을 의미한다.

또한 EMA를 2번 돌린 경우 DEMA(Double EMA)가 되며, DEMA에서 EMA를 한번 더 돌릴 경우 TEMA(Triple EMA)가 된다.

$$
DEMA = 2 \times EMA - EMA(EMA)
$$

![](https://www.norwegiancreations.com/wp-content/uploads/2016/08/dema3-1140x641.png)
청색이 본래 신호, 황색이 EMA, 적색이 DEMA 그래프이다.

아래는 DEMA를 코드로 구현한 것이다.
```
int EMA_function(float alpha, int latest, int stored);
 
int sensor_pin = 0;
 
int ema_a = 0.06;
int ema_ema = 0;
int ema = 0;
 
void setup() {
}
 
void loop() {
  int sensor_value = analogRead(sensor_pin);
   
  ema = EMA_function(ema_a, sensor_value, ema);
  ema_ema = EMA_function(ema_a, ema, ema_ema);
   
  int DEMA = 2*ema - ema_ema;
 
  delay(1);
}

int EMA_function(float alpha, int latest, int stored){
  return round(alpha*latest) + round((1-alpha)*stored);
}
```


### LRMA(Linear Regression Moving Average)



### ARMA( Auto-Regressive Moving Average) - 자기회귀이동평균

$$
X_t = c + \sum_{i=1}^{p} \varphi_i X_{t-i} + \epsilon_t
$$

## 



## 



정보통신기술용어해설
http://ktword.co.kr/abbr_view.php?nav=&m_temp1=3665&id=133

The Scientist and Engineer's Guide to Digital Signal Processing Moving Average Filters
http://www.analog.com/media/en/technical-documentation/dsp-book/dsp_book_Ch15.pdf

ALGIB - Moving average filters (SMA, EMA, LRMA)
http://www.alglib.net/time-series/moving-average-filter.php

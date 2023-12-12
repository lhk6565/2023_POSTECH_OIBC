# 2023_POSTECH_OIBC

<p align='center'>
    <img src='poster.jpeg' width='250' height='350'>
</p>

[2023 POSTECH OIBC CHALLENGE 제5회 태양광 발전량 예측 경진대회](https://o.solarkim.com/cmpt2023)에서 우수상:trophy: (2등 / 120팀)을 받은 성균관대학교 仁義禮智信팀의 발표 자료입니다.
- [이형권](https://github.com/lhk6565) (성균관대학교 산업공학과)
- [최승준](https://github.com/swanseacity) (성균관대학교 수학과, 시스템경영공학과)
- [김찬호](https://github.com/kimchanhooo) (성균관대학교 시스템경영공학과)
- [변유정](https://github.com/HBHBYJYJ) (성균관대학교 시스템경영공학과)
- [부도현](https://github.com/DohyunBu) (성균관대학교 시스템경영공학과, 소프트웨어학과)


## 대회 소개
- 기술로 전력 시장을 개방하는 **H Energy**와 **POSTECH 오픈이노베이션 빅데이터 센터(OIBC) 및 미래도시 연구센터(FOIC)**, **산업 인공지능 프로그램**이 함께 미래 전력 시장을 이끌어갈 인재를 발굴, 지원하기 위해 대학 및 대학원생 대상으로 제5회 OIBC CHALLENGE를 주최
- 다종 태양광 발전 예측 모형의 앙상블 활용 방안 개발

## 대회 기간
- 2023년 10월 22일 ~ 2023년 11월 11일: 사전 대회 기간
- 2023년 11월 13일 ~ 2023년 11월 17일: 경진 대회 기간
- 2023년 11월 28일: 온라인 발표 평가
- 2023년 12월 01일: 시상식

## 대회 미션
- "기 개발된 5종의 태양광 발전량 예측 모형들은 그 특성들이 달라 시간대별 예측 성능에 장단점이 존재함. 이러한 다종 예측 모형들의 특성을 파악 및 활용하여 예측 성능 향상이 가능한 앙상블 기법을 개발하고자 함."

## 대회 규칙
1. 대회 기간 동안의 태양광 발전소의 시간 별 발전량을 전날 10시와 17시 각각 총 두 번 API를 통해 매일 입력
2. 예측된 발전량을 입력
3. 입력한 태양광 발전량 양은 변경할 수 있으며, 최종 입력된 발전량을 기준으로 평가
4. 전날 10시, 17시 예측된 발전량의 평균과 실제 발전량의 차이를 비교하여 시간대별 예측오차율을 아래와 같이 평가
$$\frac{\left|\frac{\hat{G}_h^{10}+\hat{G}_h^{17}}{2}-G_h\right|}{C}\times 100$$
$C$ : 태양광 발전소 용량 (kw) <br>
$G_h$ : 태양광 발전소 $h$ 시간대 실제 발전량 <br>
$\hat{G}_h^n$ : 전날 $n$시에 입력한 $h$ 시간대 발전량 예측값 <br>
5. 예측 인센티브 (정산금) 산정 <br>
예측 인센티브는 시간대별 예측오차율에 따라 아래와 같이 산정 <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\- 시간대별 예측오차율 6% 이하 : $I_n$ = 4원/kWh <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\- 시간대별 예측오차율 6% 초과 ~ 8% 이하 : $I_n$ = 3원/kWh <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\- 시간대별 예측오차율 8% 이상 : $I_n$ = 0원/kWh <br>
예측 일자의 정산금은 아래와 같이 (발전소의 시간대별 발전량 $\times$ 인센티브)의 합으로 계산
$$\sum_{h=1}^{24}G_h \times I_n$$
$G_h$ : 태양광 발전소 $h$ 시간대 실제 발전량 <br>
$I_h$ : $h$ 시간대의 예측 인센티브 <br>
단, 설비 이용률 10% 미만 시 오차율 산정에서 제외되어 인센티브는 지급되지 않음 <br>
(설비 이용률 : 태양광 발전소 용량 중 실제 발전량 비율) <br>
6. 대회 기간 동안의 총 예측 인센티브 (정산금) 합이 높을수록 우수한 것으로 평가

<span style="color:red"><주의></span> <br>
\- 발전이 없는 시간대에도 예측값을 입력해야 함 <br>
\- 해당 시간까지 한차례도 예측값을 입력하지 않으면 모든 시간대에 대해서 0kWh로 입력한 것으로 간주

## 제안 방법
### Ensemble Architure
- 날씨 예측값과 모델별 예측값을 활용하여 $Adaptive$ $weight$ $\bf\hat{w}=f(x_F,x)$ 추정
### Train & Validation Method
$$L:Loss,　I:Incentive,　\alpha:hyperparameter(0<\alpha<1)$$
- 정량적 손실 $L_{quan}=I_{max}-I_{present}$
- 정성적 손실 $L_{qual}=|\bf \hat{w} \cdot x-y|^{\alpha}$
- 성능 지표 $S=(1-\frac{\sum{I_{present}}}{\sum{I_{max}}})\times100\%$
- 자세한 내용은 [발표 자료](https://github.com/lhk6565/2023_POSTECH_OIBC/blob/main/Presentation_SKKU.pdf) 참고 바람

## 코드 실행
- 디렉토리 구성은 다음과 같습니다 :
~~~
root
│
├── README.md                                    # 디렉토리 안내 파일
│
├── Presentation_SKKU.pdf                        # 발표 자료
│
└── notebooks/                                   # ipynb 파일 폴더
    ├── Validation_Dataset.ipynb                 # 
    ├── Train.ipynb                              #     
    ├── Weight_Inference.ipynb                   # 
    └── Inference.ipynb                          # 
~~~

## 느낀점
주어진 데이터가 날씨 실측값, 날씨 예측값, 모델별 발전량 예측값, 실제 발전량으로 데이터 전처리 및 특성 공학 기반의 접근이 어려운 환경이었다.<br>
이에, 기존 논문을 기반으로 일사량과 같은 특성 추가, 날씨 실측값과 날씨 예측값을 활용한 날씨 예측값 보정 모델, 날씨 및 모델 예측값의 시계열성을 반영한 구조 등의 아이디어가 나왔으나, 도메인 지식 부족으로 최종 결과물에 반영되지 못했다. 또한, 입력에 따라 적응적으로 변화하는 가중치를 도출하였으나, 시간적인 제약으로 인해 이를 해석하여 새로운 인사이트를 도출하지 못한 점이 아쉽다.<br>
그럼에도 불구하고, 데이터를 기반으로 추상적인 인사이트 도출 및 도식화를 통해 해당 시스템을 분석하고, 시스템에 대한 이해와 직관 기반의 해결방안을 수학적으로 표현하여 손실함수 및 성능지표를 제안하여 성과를 낸 점은 글로 전부 표현할 수 없는 짜릿하고 새로운 경험이었다.<br>
문제를 세분화하여 정의하고, 새로운 방법으로 문제해결을 시도하는... 대학원... 가야겠지?<br>


## 문의
코드 및 발표자료에 대해 궁금한 점이 있으실 경우 lhk6565@naver.com으로 문의해주시면 답변드리겠습니다.:blush:<br>
데이터에 대한 저작권은 대회 주관사인 [POSTECH 오픈이노베이션 빅데이터센터 (OIBC)](http://oibc.postech.ac.kr/)에 있으므로 해당 주관사로 문의 부탁드립니다.

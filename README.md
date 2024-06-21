# 2024 수학과 프로그래밍 Final Project<br>: 닫힌 집단 내에서 글의 스타일은 시간이 흐를수록 닮아가는가?
<div align="right">국어국문학과 2019110023 고혁진</div>

## 1. 들어가며
&nbsp;&nbsp;&nbsp;&nbsp;닫힌 집단 안에서 집단을 위하여 글을 쓸 때, 글의 양식은 점점 비슷해져가는 양상을 보인다. 예를 들어, 조선왕조실록, 대법원이나 헌법재판소 등의 판례, 수능시험 등을 살펴보면 초기에는 상대적으로 자유롭게 쓰여진 글들이, 시간이 지날수록 서로를 모방하고, 양식이 구체화되고, 글의 모범에 대한 암묵적 합의가 생기면서, 점점 내용과 형식 측면에서 비슷해지는 현상을 관찰할 수 있다. 그런데 이 사실이 그저 나의 과도한 의미부여인지, 실제로 있는 현상인지, 만약 실제로 있는 현상이라면 정량적으로 분석할 수 있는지 확인하고 싶었다. <br>&nbsp;&nbsp;&nbsp;&nbsp;  글의 다양도를 측정하는 방법에는 여러 가지가 있다. 대표적인 것이 타입 토큰 비율(Type-Token Ratio, TTR)을 측정하는 방법이다. 단어의 총 개수와, 단어의 종류의 개수를 비교해 얼마나 다양한 어휘가 사용되었는지를 비교하는 방법이다. 그러나 이 방법은 어휘의 다양도를 측정할 뿐, 글 전체적으로 형식이나 내용이 얼마나 비슷해져가는지를 측정하는데에는 적합하지 못하다. 그래서 본 연구에서는, 정보엔트로피 개념을 이용하여 "글의 수렴도"를 측정하고자 했다. 정보엔트로피 개념을 이용하여 글의 다양도를 분석하려는 아이디어는, 가와노 히로시의 『예술 · 정보 · 기호』 4장~6장에서 소개된 문학 작품과 음악 작품의 원소들의 출현 빈도를 조사하여 정보엔트로피를 이용해 예술성을 측정한 선행 연구들에서 아이디어를 얻었다. <br>&nbsp;&nbsp;&nbsp;&nbsp;  한국어는 교착어로서, 각 문장성분의 격틀이 조사에 의해 결정되는 언어이다. 따라서 각 주격, 서술격, 목적격 조사 앞에 있는 어휘들의 엔트로피를 조사하면, 각 주어, 서술어, 목적어 자리에 나오는 단어들의 균질성을 파악할 수 있지 않을까 하는 생각이 들었다. 즉, 글이 시간이 지남에 따라 서로 비슷해져서 글의 형식적, 내용적 다양성이 감소된다면, 각 문장의 자리에는 한정된 어휘만 오게 될 것이고, 주요 문장 성분인 주격, 서술격, 목적격에 오는 어휘들의 "뻔함"의 정도는 더욱 높아질 것이고, 반대로 "놀라움"의 정도라고 할 수 있는 정보 엔트로피는 감소할 것이다. 본 연구에서는 앞선 아이디어로부터, 1960년부터 2010년대까지의 대법원 판결문 중, 민사재판의 "판결이유" 문장을 시대별로 카테고리화하여, 각 문장성분 자리의 단어의 균질성을 파악하여, 시대가 지남에 따라 얼마나 문체의 다양성이 적어지는지를 측정하고자 한다. <br><br>

## 2. 데이터 수집 및 전처리 방법
2-(1). 정보 엔트로피는 다음과 같이 정의된다. $H(x) = -\sum\limits_{i=1}^n P(x_i) log P(x_i)$ 이 식에 따르면, 나올 확률이 적은 문자가 나온다면 엔트로피의 값은 증가하고, 반대로 확률이 높은 문자가 나온다면 엔트로피의 양은 감소한다. 판례문 전체의 텍스트의 어휘와 빈도를 조사하고, 특정 조사 앞에 나오는 어휘를 따로 정리하여 주격, 서술격, 목적격 자리에 나오는 어휘들의 확률을 매칭한 후, 그 확률에 따른 정보 엔트로피를 구했다. 이를 이제부터 "통사적 엔트로피"로 부르도록 하겠다. <br><br>2-(2). 판례문 데이터는 국가법령정보의 Open API를 이용했고, 분석 양을 줄이기 위해 랜덤 추출을 진행하였다. 선고일자의 년 월 일을 합한 숫자가 5의 배수인 것만 남기고, 사건번호를 3으로 나눈 나머지가 1인 것만을 남겨서, 총 524건의 판례데이터를 추출하였고, 그에 따라 총 1,075,686자의 텍스트가 만들어졌다. 이 과정은 'Pretreatment.ipynb'에 담겨있다. <img src="Image\OPEN API.png"><br> &nbsp;&nbsp;&nbsp;&nbsp;그런데 여기서 정보엔트로피에 공식에 따르면, 특별한 경우가 아니라면, 전체 글에 포함된 단어의 개수가 많을수록 엔트로피는 자연스럽게 증가한다. 따라서 이를 동질하게 하기 위해 각 년도별로 7만자만 남기고 뒷자리를 삭제했다. 판례들은 선고 일자에 따라 정리되어 있으므로, 이렇게 자르더라도 데이터의 성질이 변질되지 않는다. <br><br>2-(3). 다음으로 년도별 판례문을 KoNLPy의 Mecab분석기를 활용해 형태소별로 정제하였고, 특정 조사 앞에 있는 어휘를 추출하는 코드를 작성하여 이 빈도를 조사해서 확률로 만들었다. 그리고 년도별로 특정 조사 앞에 나오는 어휘들의 사전 확률을 앞선 엔트로피 공식에 넣어서 계산하는 작업을 수행했다. 이 과정은 'Final Project.ipynb'에 담겨있다.<br><br>

## 3. 엔트로피 분석
분석 수행 결과는 다음과 같다.<br>
<img src="Image\문장길이분포.png">
&nbsp;&nbsp;&nbsp;&nbsp;우선 문장길이들이 분석에 영향을 줄 정도로 비슷한지 아닌지를 검사했다. 먼저 시대별로 문장 길이 분포가 정규성을 가지는지 검정하였다. 모든 년도에 대해 Shapiro-Wilk Test를 수행한 결과, 문장 길이의의 분포의 P-Value는 0.04 이하이므로, 모든 년도가 정규성을 만족하므로 Bartlett's Test를 수행하였고, Bartlett's Test Statistic: 4.538258343067401, Bartlett's Test P-Value: 0.03314529950209816로 문장 길이에 대해 분산 차이가 유의미하게 있었다. 따라서 히스토그램으로 표시해서 직접 데이터를 확인하였고, 몇가지 문장 추출이 제대로 이루어지지 않은 것으로 보이는 특이값(12,000자를 넘는 문장은 상식적이지 않다)을 제외하면 문장 길이 분포가 결론에 영향을 줄 정도로 크게 차이가 나지 않았고, 분석 작업을 계속 진행했다.<br><br>
&nbsp;&nbsp;&nbsp;&nbsp;각 격틀별로 어휘를 추출해 출현빈도와 그에 따른 확률을 계산했고, 각 자리에 따른 빈도 상위 10순위를 정리한 표는 다음과 같다.<br>
<img src="Image\빈도정보이미지.png"><br><br>
&nbsp;&nbsp;&nbsp;&nbsp;각 격틀을 추출한 기준은 다음과 같다. 먼저 주격 성분의 경우, 주제를 나타내는 보조사 은/는(품사 태깅은 JX) 앞에 나오는 단어들을 추출했다. 실제 주격 조사가 아닌 보조사 앞에 나타난 어휘를 추출한 이유는, 본 연구자가 한국어는 주어가 없고 대신 주제(Topic)가 있을 뿐이라는 주제어라는 주제어설을 지지하기 때문이며, 이에 따라 주제 성분임을 표지하는 보조사를 기준으로 주제성분을 추출했다. 그래서 분석과정에서 '주어'가 아닌 '주제'라는 용어를 사용했다. 목적격 성분의 경우, 목적어를 나타내는 보조사 을/를(품사 태깅은 JKO) 앞에 나오는 단어들을 추출했다. 마지막으로 서술격 성분의 경우, 종결어미로 끝남을 나타내는 태그들, EF, VV+EF, VA+EF, VX+EF의 앞 성분을 추출하였고, 서술격 조사 '이다'와, 의존명사 '것'과 함께 출현하는 '것이다'를 처리하기 위해, VCP+EF 태그들 앞에 나타나는 성분과, NNB+VCP+EF 태그들 앞에 나타나는 성분을 추출했다.

&nbsp;&nbsp;&nbsp;&nbsp;위에서 추출한 확률을 바탕으로 각 시대별로 각 자리의 통사적 엔트로피를 계산하였고, 그 추이는 다음과 같다. 이때 확률을 계산하는 과정에서 분모 자리에는 시대 전체의 빈도수를 사용하였고 분자자리에 역시 각 격틀의 시대 전체 빈도수를 사용하였다. 그리고 이렇게 고정된 확률을 이용하여 시대별로 엔트로피를 계산하였다. 본 연구의 목적에 맞게 계산하려면 그렇게 설정해야 한다고 판단했기 때문이다.<br>
<img src="Image\엔트로피변화추이.png">

&nbsp;&nbsp;&nbsp;&nbsp;각 자리의 통사적 엔트로피를 평균한 값을 전체 문장 전체 엔트로피로 나눠 상대적 엔트로피를 계산하였고, 그 추이는 다음과 같다.<br>
<img src="Image\상대엔트로피변화추이.png"><br><br>

## 4. 나가며
&nbsp;&nbsp;&nbsp;&nbsp;통사적 엔트로피의 시대적 추이를 확인한 결과, 연구를 시작하면서 했던 가정인 "만약 문체가 서로 닮아갔다면, 주요 문장성분에 나오는 어휘들이 균질해질 것이고 따라서 이들의 정보엔트로피를 합한 통사적 엔트로피는 시간이 지날수록 점점 감소할 것이다."라는 주장과 부합하는 결과가 나왔다. 그러나 몇 가지 문제가 있다. (1) 전체 엔트로피가 시대에 따라 감소한 점이다. 연구 전에는 전체 엔트로피는 증가할 것으로 예상했다. 왜냐하면 시대가 지나면서 새로운 어휘와 개념이 등장해 사용되는 어휘 종류가 더 다양해질 것이라고 생각했기 때문이다. 그러나 전체 엔트로피 또한 시대가 지날수록 감소되었다. 이에 대해서 방법론 자체의 부적절성인지, 단순 계산 실수인지를 검증할 필요가 있다. (2) 한국어 형태소 분석기는 완벽하지 않다. 일례로, 본 연구에서 "피고" 라는 명사 단어를 "피"+"고"라는 동사 굴절 형태로 파악한 경우가 몇 개 발견되었다. 이에 대해 더 확실하게 점검을 했어야 했는데 시간의 부족으로 그러지 못하였다. (3) 민사 재판 판례가 아닌 형사 재판 등 다른 판례나, 아니면 신문, 소설 등의 다른 텍스트와 비교해서 이 가정과 결과가 올바른지 판단했어야 했는데 그러지 못했다. 추후 연구를 한다면 이 점에 대해서 보완하고 싶다. 또 기회가 된다면 정보 엔트로피를 이용하는 방법이 아닌, 더 적절한 방법론으로 문체가 닮아가는 방법을 검증해보고 싶다.<br><br>

## 5. 참고문헌
* 川野洋, 진중권 역.(1990) 『예술 · 정보 · 기호』.도서출판중원문화.<br>
* 박은정, 조성준. (2014).“KoNLPy: 쉽고 간결한 한국어 정보처리 파이썬 패키지”. 제 26회 한글 및 한국어 정보처리 학술대회 논문집.<br>
* hyunwoongko/kss - GitHub<br>





---
layout: post
title: "주요 뉴스 큐레이션 사이트 개발을 위한 조사"
date: 2022-01-29
excerpt: "주요한 뉴스만 모아 제공하는 사이트 개발을 위해 관련된 자료들을 조사하며 요약 정리하는 문서"
tags: [study]
category: [SUMM]
comments: false
---
## 기존 사이트
### 한눈에 보는 오늘-네이트 뉴스
* 10분 단위로 주요 뉴스를 확인
	* 가장 많이 언급된 키워드 10개를 제공
	* 해당 키워드를 클릭하면 같은 키워드의 비슷한 기사들도 확인 가능
* 사용자 검색 기능을 제공하지 않는다.
### 뉴스퀘어
* 주요 뉴스 요약 사이트
* 알고리즘이 아닌 편집자가 직접 기사를 요약하는 뉴스 큐레이션 서비스
* 참고 링크를 통해 원본 기사 링크를 제공
### 섬리
* 관심있는 분야와 언론매체를 설정하면 자동으로 뉴스를 선별하여 보여주는 뉴스 요약 서비스
* 사용자 맞춤 큐레이션
* 인공지능 기술을 바탕으로 주요 언론사 뉴스를 검색하여 자연어 처리 방식으로 뉴스 내용을 요약해 제공
### 와비 
* 사용자가 관심있는 분야에 대한 뉴스를 선별해 제공
* 자연어 처리 기술을 바탕으로 뉴스의 핵심 내용을 요약
	* 스팸링크가 걸린 뉴스나 내용이 중복되는 콘텐츠는 자동으로 필터링 된다.
### 미디어 스파이더
* 사용자 관심 분야에 맞는 뉴스와 정보 큐레이션
* SNS와 연동해 SNS 구독 서비스도 함께 제공
### 빅카인즈
* 매일 수집된 뉴스의 주요 이슈, 언론사별 뉴스, 나의 관심뉴스, 주요 키워드를 한눈에 볼 수 있는 서비스
	* 뉴스 내 중요키워드 추출 (토픽랭크 알고리즘)
	* 유사한 뉴스를 클러스터링 (코사인 유사도)
	* 클러스터 내 대표 키워드를 추출해 오늘의 이슈 제목 구성
* 매일 수집된 뉴스 속에서 주요 인물ㆍ장소ㆍ기관을 분석해 해당 키워드가 포함된 뉴스 건수가 높은 순으로 보여준다. 수집된 뉴스는 내용에 따라 정치, 경제, 사회, 문화 등 분야별로 확인 가능.

## 네이버 뉴스 추천 알고리즘
<a href="https://m.blog.naver.com/naver_search/222439351406">([참고] 네이버 기술 블로그-네이버 뉴스 추천 알고리즘에 대해)</a>
### MY 뉴스
다양한 언론사의 수많은 뉴스 기사를 제공받아, 기계학습 등의 추천 알고리즘에 기반하여 사용자들에게 맞춤형 뉴스 추천 서비스를 제공한다.
<br>
사용자의 과거 뉴스 소비 이력과 제목, 본문 등 뉴스 기사 내용의 관계를 분석하여 각 사용자들이 관심을 가질 만한 뉴스 기사를 예측한다. 이때, 예측에 사용되는 기계학습 모델은 언론사에서 제공하는 뉴스 기사 데이터와 뉴스 서비스 사용자들이 남기는 로그 데이터를 학습데이터로 이용한다.

### 뉴스 추천 알고리즘 설계 고려사항
* [DC1] 실시간으로 사용자의 선호도 예측 : 매일 수만건의 뉴스 기사가 생산되는 뉴스 도메인 특성을 감안해 사용자의 선호도를 즉각적으로 예측할 수 있는 가볍고 효과적인 추천 모델이 필요하다.
* [DC2] 자동화 방식의 뉴스 품질 측정 : 저품질 뉴스 기사가 사용자에게 추천될 경우 사용자의 관심 주제에 맞더라도 만족도를 떨어뜨리고 뉴스 서비스에 대한 불신을 야기한다. 수동으로 품질을 측정할 수 있으나 편집자의 주관이 개입될 가능성이 있어 자동으로 뉴스 기사의 품질을 측정할 방법이 필요하다.
* [DC3] 시의 적절한 주요 이슈 감지 : 사용자들의 개별적인 선호도와 무관하게 공통적으로 관심을 갖는 뉴스 기사가 존재한다. 따라서 여러 언론사에서 공통적으로 다루는 사회적 이슈에 관한 뉴스 기사를 추천하는 것이 필요하다.
* [DC4] 확장성 있는 시스템 구조 : 안정적으로 뉴스 추천 서비스를 제공하기 위해 확장 가능한 시스템 구조가 필요하다.
### DCs 반영
* 개인화된 뉴스 추천을 위해 다음 2단계를 수행한다.
	* 후보 뉴스 기사 생성 : 랭킹 단계의 불필요한 계산량을 줄여 서버를 효율적으로 활용하기 위해 후보 뉴스를 선별하는 단계
	* 후보 뉴스 기사 랭킹 : 사용자와 뉴스 기사에 관한 여러 타입의 feature 점수를 통합하여 최종 추천 점수를 계산, 최종적으로 사용자가 만족할 만한 상위 k개의 기사를 추천하게 된다.
* DC1을 고려해 협업 필터링(CF) 모델은 로그인한 사용자의 최근 소비 이력과 유사한 사용자들이 공통적으로 소비한 뉴스 기사를 파악한다.
* DC2를 고려해 품질 예측(QE) 모델은 뉴스 기사의 내용과 사용자의 피드백 데이터(e.g. 조회수와 체류 시간)에 기반한 고품질의 기사를 파악한다.
* DC3를 고려해 사회적 영향도(SI) 모델은 시의 적절하게 최근에 많은 사용자들이 관심을 가지는 뉴스 기사와 여러 언론사에서 공통적으로 다루고 있는 주요 이슈 기사를 파악한다.
* DC4를 고려해 실시간 추천을 지원하는 확장 가능한 시스템 아키텍처를 구축헸다.

### 후보 뉴스 기사 생성 단계
* 협력 필터링 알고리즘
	* 네이버 뉴스 추천시스템에서는 Normalized Point-wise Mutual Information(NPMI)을 이용하는 item-based CF 모델을 사용하고 있다. 이 모델은 사용자의 최근 기사 소비 이력과 유사한 다른 사용자들의 기사 소비 U~(iobj)~ 가 선호할 만한 최신 기사를 효과적으로 찾아준다.
	* 모든 뉴스 기사 페어에 대해서 NPMI 점수가 계산된 후 최근 y 시간 내에 송고된 뉴스 기사들 V~(k)~에 대한 사용자 U~(i)~의 선호도를 예측한다.
* 자동화 방식의 뉴스 품질 측정
	* 각 뉴스 기사의 제목, 본문 등 콘텐츠 정보와 네이버 뉴스를 이용하는 수천만명 사용자들의 피드백 데이터(즉, 클릭 수와 체류 시간)를 함께 고려하여 해당 기사의 품질을 추론하는 QE 모델을 학습하여 사용한다.
	* 뉴스 추천시스템에서는 뉴스를 소비하는 사용자의 관점에서 많은 사용자들이 만족스럽게 읽은 기사를 고품질 기사라고 간주한다.
	 	* 클릭수가 높더라도 체류 시간이 낮은 기사는 고품질 기사가 될 수 없다. 기사 내용을 확인하는 과정에서 사용자의 이탈을 유발하여 불만족스러운 경험을 주었기 때문.
	 	* QE 모델은 네이버 뉴스 전체 사용자들이 만족했던 뉴스 기사들과 유사한 콘텐츠 특성을 갖는 최신 뉴스들을 후보 기사로 추출한다.
	 * 기사의 2가지 내용적인 feature과 기사의 구성적인 형식과 관련된 4가지 범주형 feature을 사용한다(제목, 본문/기자 정보,섹션 정보,콘텐츠 타입 정보, 이미지 혹은 동영상 관련 정보).
	 * DNNs 구조를 통해 각 피쳐들의 비선형적 결합을 통해 품질 점수를 예측한다. 
	 *  사용자들의 만족도는 시간이 지남에 따라 계속 변할 수 있기 때문에 이를 모델 학습에 반영하기 위해, 최근 x 일 내에 송고된 뉴스 기사만을 학습에 사용한다.
	 * 각 언론사마다 최대 10,000건의 과거 기사만 랜덤 샘플링(하여 송고된 기사 수가 많은 언론사의 기사들이 학습데이터에 더 많이 포함되지 않도록 했다. 
	 * 각 뉴스 기사가 송고된 이후 y 시간 동안의 총 클릭 수 cj 와 평균 체류 시간 dj 을 측정하여, vj 의 총 클릭 수 cj 와 평균 체류시간 dj 을 기반으로, 기사의 페어링(pairing)을 생성하는 2가지 방식을 실험했다.
* 시의 적절한 주요 이슈 감지
	* 사용자들이 기존에 관심을 가진 사안에 대한 뉴스만 추천 받지 않도록 주요 이슈와 관련된 뉴스 기사들을 시의 적절하게 찾아내야한다.
	* 많은 사용자들에 의해 단기간에 많이 클릭된 기사를 찾는 SI(user) 모델을 사용한다.
	* 뉴스 공급자 관점에서의 사회적인 이슈를 탐지하기 위해, 다양한 언론사로부터 최근 송고된 뉴스 기사들에 빈번히 등장하는 키워드를 많이 포함하는 뉴스 기사를 찾는다.
	* 두 유형의 SI 모델을 통해 각각 소비자 관점과 공급자 관점에서의 사회적인 이슈를 모두 고려하는 후보 기사를 시의적절하고 빠르게 추출할 수 있게 된다.
* 뉴스 추천의 다양성 반영
	* 기사에 대한 최종 선호도를 높은 순으로 정렬하여 그대로 추천할 경우, 아래와 같은 기사들이 노출되는 현상이 관찰되어 아래 조건에 해당하는 기사들은 후처리 로직을 통해 하위에 노출될 수 있도록 했다.
		* 사용자가 과거에 이미 클릭하여 읽은 기사
		* 동일한 클러스터내의 기사가 여러 건 추천되는 경우
		* 거의 동일한 제목의 기사가 여러 건 추천되는 경우
		* 동일한 썸네일 이미지 기사가 여러 건 추천되는 경우
	* 섹션 선호도 점수를 활용하여, 선호하는 섹션의 기사들이 먼저 추천될 수 있도록 한다.

## 가짜 뉴스 탐지
### 사용자의 일반적인 가짜 뉴스 판별
1. 뉴스의 출처를 파악하라. 
2. 글을 끝까지 읽어라. 
3. 작성자를 확인하라. 
4. 근거자료를 확인하라. 
5. 작성 날짜를 확인하라. 
6. 자신의 생각이 한쪽으로 치우친 것은 아닌가 생각해보라. 
7. 전문가에게 물어보라.

### 뉴스와 소셜 데이터를 활용한 텍스트 기반 가짜 뉴스 탐지 방법론
> 현윤진, 김남규.(2018).뉴스와 소셜 데이터를 활용한 텍스트 기반 가짜 뉴스 탐지 방법론.한국전자거래학회지,23(4),19-39. 을 읽고 정리한 자료입니다.

기존의 뉴스 자체만으로는 식별하기 어려운 뉴스의 진위여부를 소셜미디어(트위터) 상의 여론을 추가로 활용해 더 정확하게 진위여부를 식별할 수 있을 것으로 판단했다.
<br>
각 뉴스와 관련된 Tweet을 추려내고, 이들로부터 각 뉴스에 대한 반응들을 벡터 형태로 산출한 후, 뉴스 원문에서 추출한 기사의 주제적 특성(Topic Features)과 트위터 반응 벡터(Twitter Topic Feature)와의 결합을 통해 각 뉴스의 진실/거짓 여부를 더욱 정확하게 탐지하고자 한다.
<br>
* 토픽 모델링 : 각 문서에 포함된 용어의 빈도수에 기반해 유사 문사를 그룹화한 뒤 각 그룹을 대표하는 주요 용어들을 추출함으로써 해당 그룹의 토픽 키워드 집합을 제시하는 방식
	* 벡터 공간 모델 
	* TF-IDF : 여러 문서에서 자주 출현하는 일반적인 단어에 대한 가치는 낮게 부여하고 특정 문서에 서만 출현하는 특수한 단어에 대해 가치를 높게 부여한다.
* 정확도 과평가 방지를 위한 데이터 분할
	* 뉴스 데이터를 대상으로 토픽모델링을 통해 구조화하면 문서/토픽 대응 행렬을 얻을 수 있다.
	* 이 결과를 활용하여 클러스터링을 수행함으로써 유사한 이슈들을 갖는 뉴스 그룹을 생성하게 된다.
	* 생성된 각 이슈 그룹들 내 뉴스 기사들의 진위여부에 따라 학습과 검증 데이터로 분리한다.
	* 분리된 뉴스 기사에 따라 트위터 데이터 역시 학습과 검증 데이터로 분리된다. 이때, 트위터 데이터는 뉴스 기사와 종속 관계를 가지고 있기 때문에 관련된 뉴스의 진위여부에 따라 해당 트윗의 진위여부가 결정된다.
* 뉴스 데이터를 활용한 가짜 뉴스 탐지 모델
	* 뉴스의 주제적 특성을 활용하여 기계학습 알고리즘을 사용하여 가짜 뉴스 탐지 모델을 구축한다.
* 트위터 데이터를 활용한 가짜 뉴스 탐지 모델
	* 뉴스별 트윗들의 벡터 값을 차원(Topic)별로 평균하여 뉴스별 트위터 벡터 값을 산출하게 된다.
	* 뉴스별 트위터 벡터 값을 입력 값으로 하여 기계학습 알고리즘을 사용해 가짜 뉴스 탐지 모델을 구축한다.
	* 트위터 데이터만으로 뉴스의 진위여부를 판정하는데 한계가 있다.
* 뉴스와 트위터 데이터를 모두 활용한 가짜뉴스 탐지 모델
	* 뉴스 데이터의 속성과 트위터 데이터의 속성을 결합하는 과정이 우선적으로 수행되어야 한다
		* 뉴스의 주제적 특성에 뉴스별 트위터 벡터 값들을 새로운 변수로 추가하는 방식으로 이루어진다.
	* , 뉴스의 주제적 특성과 트위터 벡터 값이 결합된 산출물을 입력 값으로 하여 기계학습 알고리즘을 사용해 가짜 뉴스 탐지 모델을 구축하게 된다.
<br>
 제안 방법론은 뉴스 데이터를 구조화하여 클러스터링을 통해 이슈 그룹을 생성
하고, 이슈 그룹 내 진위여부가 다른 뉴스 기사를 학습 및 검증 데이터로 분리함으로써 가짜 뉴스 탐지 모델 정확도의 공정성을 확보하였다.
 또한 뉴스와 트위터 데이터를 활용한 제안 방법론의 정확도가 뉴스와 트위터를 각각 활용한 가짜 뉴스 탐지 모델에 비해 약 28%p 향상됨을 증명하였다.

### 디지털 뉴스 알고리즘 플랫폼의 뉴스 신뢰도와 합의착각 효과: 이용 동기, 지각된 유용성, 지각된 위험성과 지각된 편향성의 영향
> 김미경 and 이은지. (2019). 디지털 뉴스 알고리즘 플랫폼의 뉴스 신뢰도와 합의착각 효과: 이용 동기, 지각된 유용성, 지각된 위험성과 지각된 편향성의 영향. 정치커뮤니케이션연구, 55, 39-83. 를 읽고 정리한 자료입니다.

한국에서 디지털 뉴스 알고리즘 플랫폼으로 뉴스를 보는 이용자 비율은 77%에 이른다. 한국은 디지털 뉴스 알고리즘 플랫폼의 의존도가 가장 높고 언론사 방문 비율은 가장 낮다.

* 알고리즘에 의해 편집된 뉴스는 접근성과 편의성으로 인해 개인의 성과를 높이는 것으로 간주되고 있다. 따라서 알고리즘에 의해 뉴스를 제공하는 플랫폼에 대해 이용자의 뉴스 신뢰도가 영향을 받는 것으로 해석할 수 있다. 
* 지각된 편향성은 뉴스 신뢰성에 영향을 미치는 것으로 나타났다. 
	* 디지털 뉴스 알고리즘 플랫폼에 대한 지각된 편향성은 디지털 뉴스 알고리즘 플랫폼이 제공하는 뉴스를 소비하면서 미디어의 기울기를 지각하는 것이다. 
	* 여기에는 이용자 자신이 지니는 개인의 인지편향도 포함하여 해당 뉴스에 대한 신뢰성 평가에도 영향을 미친다.
	*  지각된 편향성과 뉴스신뢰도는 합의착각의 관계에서 더욱 강화된 확증편향을 만들고, 더욱 편향적인 소비를 가능하게 하는 필터 버블에 갇히는 가능성에 대해서도 추정할 수 있다.
*  알고리즘 추천 기술이 플랫폼의 신뢰도를 촉진한다는 인식은 사람에 의한 편집보다 알고리즘이 더 중립적이라고 인식하고 있음을 추정하게 한다
* 디지털 뉴스 알고리즘 플랫폼은 뉴스의 선별적 제공으로 다양한 의견을 배제할 위험성이 내포하고 있다.
	*  디지털 뉴스 알고리즘 플랫폼은 이용자가 많이 보는 기사를 중요한 뉴스로 추천한다. 이러한 추천의 위험성과 편향성을 이용자가 지각하는 것은 다른 사람도 나와 비슷한 의견을 갖고 있다고 추정하게 함으로써 확증편향을 강화할 가능성을 나타내기도 한다.
* 알고리즘 편집으로 인한 공정성 훼손의 위험성도 존재한다.
	* 이용자들의 이러한 위험성 인식에도 여론이 자신의 의견과 유사하게 형성된다고 믿는 경향을 보였다. 디지털 알고리즘의 위험성을 지적하면서도 여론 해석의 확증편향에서 벗어나지 못하고 있음을 보여주고 있다. 
* 디지털 뉴스 알고리즘 플랫폼에 대한 지각된 편향성은 수용자 각자에게 유리
한 쪽으로 여론을 지각하는 경향성을 보였다.

### TF-IDF를 활용한 k-means 기반의 효율적인 대용량 기사 처리 및 요약 알고리즘
> 장민서, 오수진, 김응모. (2018). TF-IDF를 활용한 k-means 기반의 효율적인 대용량 기사 처리 및 요약 알고리즘. 한국정보처리학회 학술대회논문집, 25(1), 271-274.
우선 k-means 알고리즘을 적용하여 대용량 기사를 k개의 군집으로 분류한다. 
그리고 TF-IDF 가중치 모델을 적용하여 불필요한 문장을 제거하고 핵심 문장을 추출한다.

최적의 k값을 정하기 위해 elbow방법을 이용했다. 문서 군집에서 가장 적합하다고 여겨지는 거리 코사인 유사도를 사용하며, 초기값으로 20을 지정한다. 
(그림 2)에서 군집 내 분산이 급격하게 줄어들다 점점 완만해지는 경향을 띔을 알 수 있다. 기울기가 완만해지는 지점이 가장 적은 오차 값에 가지는 지점으로
볼 수 있기 때문에, 본 논문에서 최적의 k값으로 15를 선정하였다. 따라서 수집된 데이터들은 총 15개의 군집으로 나눠졌으며, 각 군집에 속하는 기사들을 군집별로 리스트 화되어 저장된다.

. TF-IDF을 이용하여, 각 문장에 속해있는 단어들을 비교하고 문장의 유사도를 측정한다. 비슷한 공간에 매핑 될 때를 유사성이 높다고 판단하며, 전체 문서 집합에서 여러
문장에 걸쳐서 함께 등장하는 단어 간에는 관련성이 있다고 판단한다. 이와 같은 방식으로 얻어진 문장 유사도와 단어 간 관련성의 평균을 이용하여 불필요한 문장들을 제거한다. 

앞서 구한 빈출 빈도가 높은 단어와 문장의 유사도를 기반으로 각 군집별로 한 개의 핵심문장을 추출한다. 다른 문장들과의 유사도 합이 높을수록 문서 내에서
중요한 문장으로 구분된다. 문장 간의 유사도 측정을 통해, 기사 내에서 해당 문장이 가지는 비중을 구할 수 있다. 어떤 한 문장이 기사 내에서 큰 비중을 가진다면, 이
는 중요한 문장이라 할 수 있으며, 이 중 가장 큰 비중을 차지하는 문장을 군집의 핵심 문장이라고 판단한다.

### 다중 요약
https://www.dbpia.co.kr/pdf/pdfView.do?nodeId=NODE08000289




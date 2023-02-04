# <div align=center> A Prediction of Psychological Tendency based on Model Ensemble <br/> using Psychological Test Dataset </div>

<div align=right> <img alt="GitHub repo size" src="https://img.shields.io/github/repo-size/HJK02130/A-Prediction-of-Psychological-Tendency-based-on-Model-Ensemble-using-Psychological-Test-Data?style=flat-square"> <img alt="GitHub code size in bytes" src="https://img.shields.io/github/languages/code-size/HJK02130/A-Prediction-of-Psychological-Tendency-based-on-Model-Ensemble-using-Psychological-Test-Data?style=flat-square"> <img alt="GitHub language count" src="https://img.shields.io/github/languages/count/HJK02130/A-Prediction-of-Psychological-Tendency-based-on-Model-Ensemble-using-Psychological-Test-Data?style=flat-square"> </div>


### Contents
1. [Overview](#overview)
2. [Requirements](#requirements)
3. [Architecture](#architecture)
4. [Result](#result)
5. [Conclusion](#conclusion)
6. [Reference](#reference)
7. [Developer](#developer)


### Overview
[[DACON contest]](https://dacon.io/competitions/official/235902/overview/description)<br/>
This repository includes algorithms that predict psychological propensity by analyzing psychological test data. We proceeded this project in the preliminary round of the software-oriented university joint AI competition organized by DACON. As the range of psychological tests widens, we analyze the psychological tendency of national election voters/non-voters to explore how to analyze data in that area. Using the data provided by DACON, we classified 'nerdiness'(target value) based on the personal information and answers of the questionnaire. So, We implemented a binary calssification algorithm using pycaret model ensemble. We used area under the ROC curve(AUC) for performance evaluation metrics. Based on AUC, three models with the highest score (Extra Trees Classifier, Random Forest Classifier, and Extreme Gradient Boosting) were selected and we ensembled them. Performance evaluation results showed a high performance of 0.899 for public test set and 0.896 for the private test set.
<br/>
<br/>
본 repository는 심리학 테스트 데이터 분석을 바탕으로 심리 성향을 예측하는 알고리즘을 포함하고 있습니다. 이 프로젝트는 DACON에서 진행한 SW중심대학 공동 AI 경진대회 <예선>에서 진행된 프로젝트입니다. 본 프로젝트에서는 심리학 테스트의 범주가 넓어짐에 따라 해당 영역의 데이터 분석 방법을 탐구하기 위해 국가 선거 투표자/미투표자의 심리학적 성향을 분석합니다. DACON에서 제공하는 데이터를 사용하여, 개인 정보와 설문 조사 답변 내용을 바탕으로 nerdiness를 예측하였습니다. Pycaret 앙상블을 이용한 이진 분류 알고리즘을 구현하였으며, AUC를 성능 평가 지표로 활용하였습니다. AUC 기준 가장 성능이 높은 3가지 모델(Extra Trees Classifier, Random Forest Classifier, and Extreme Gradient Boosting)을 선정하여 앙상블 하였습니다. 성능 평가 결과, DACON에서 제공하는 public test set에서는 0.899, private test set에서는 0.896의 높은 성능을 보여주었습니다.

### Requirements
+ Python 3.6


### Architecture
+ [Data](https://dacon.io/competitions/official/235902/data)
	+ Psychological test answer results
	<br/><br/>
	
+ Preprocessing
	+ Feature processing
		+ 'country' : Change all values except for the top 90 species in order of frequency to 'etc'
		+ 'introelapse', 'testelapse', 'surveyelapse', 'age', 'familysize' : Percentile processing
		<br/>
	+ Missing value
		+ 'country', 'education', 'gender', 'engnat', 'hand', 'voted', 'married', 'ASD' : Create new feature named 'feature_isna' to classify missing values
		+ 'Q', 'TIPI' : Imputation of missing value to 3
		+ 'VCL' : Imputation of missing value to mode
		+ 'familysize' : Imputation of missing value to rounding of mean
		<br/>
	+ Feature Engineering
		+ 'T' : Tatic question summation of feature 'Q'
		+ 'V' : View question summation of feature 'Q'
		+ 'M' : Morality question summation of feature 'Q'
		+ 'Mach_score' : Mean of feature 'Q'
		+ 'lier' : Person who answered that he know the question of feature 'VCL6', 'VCL9', 'VCL12'
		+ 'Ex' : Extraversion question summation of feature 'TIPI'('TIPI1'-'TIPI6')
		+ 'Ag' : Agreeableness question summation of feature 'TIPI'('TIPI7'-'TIPI12')
		+ 'Con' : Conscientiousness question summation of feature 'TIPI'('TIPI3'-'TIPI8')
		+ 'Es' : Emotional Stability question summation of feature 'TIPI'('TIPI9'-'TIPI4')
		+ 'Op' : Openness question summation of feature 'TIPI'('TIPI5'-'TIPI10')
		+ 'is_adult' : Person who 'age' >= 18
		<br/>
	+ One-Hot Encoding : Categorical feature 'country', 'urban', 'gender', 'engnat', 'hand', 'religion', 'orientation', 'voted', 'married'
	<br/>
	
	+ Log-Scaling : Feature 'introelapse', 'testelapse', 'surveyelapse'
	<br/><br/>
+ Modeling
	+ Pycaret
		+ Settings of comaparing models
			1. 5 fold
			2. Select 3 models(Extra Trees Classifier, Random Forest Classifier, and Extreme Gradient Boosting) with high performance based on AUC
			3. Tunning
			4. Other : Predict with probability
			<br/><br/>
+ Result Modifying
	+ If predicted probability exceeds 0.9, we modified it to 1 and if predicted probability is less than 0.1, we modified it to 0
		

### Result
|Model|Accuracy|AUC|Recall|Prec.|F1|Kappa|MCC|TT(Sec)
|:---:|:---|:---|:---|:---|:---|:---|:---|:---|
|Extra Trees Classifier|0.7645|0.8531|0.8269|0.7663|0.7953|0.5191|0.5215|0.2900|
|Random Forest Classifier|0.7635|0.8490|0.8276|0.7647|0.7948|0.5167|0.5193|0.2880|
|Extreme Gradient Boosting|0.7511|0.8200|0.8059|0.7593|0.7819|0.4927|0.4940|1.2060|


|Ensembled Model|Public test set|Private test set|
|:---:|:---|:---|
|AUC|0.89935|0.89578|

### Conclusion
The process of selecting the best model based on specific performance evaluation metrics through Pycaret is very simple and efficient method, but it seems that performance can be further improved if model tuning and parameter adjustment are made more detailed. And it is also a good idea to select a model in pycaret then import and train the selected model, rather than training it inside pycaret after selection. In addition, if the source of data is unknown, we think it could have been good method that training without complicated preprocessnig like creating new features and imputation missing values. At the end, we can improve performance by rounding the probability results. If the performance is improved considering these limitations and improvements, it is expected that it will be a psychological tendency analysis that helps to explore data analysis in the field of psychological testing.

### Reference
[1] Payton J. Jones. (2019). psychometrics[Text document]. https://github.com/paytonjjones/psychometrics.<br/>
[2] Pycaret. (2022). pycaret[Source code]. https://github.com/pycaret/pycaret.<br/>
[3] PyCaret Official. (n.d.). Retrieved from https://pycaret.gitbook.io/docs/<br/>
[4] John, O. P., & Srivastava, S. (1999). The Big-Five trait taxonomy: History, measurement, and theoretical perspectives. Handbook of personality: Theory and research, 2, 102–138. New York: Guilford Press.

### Developer
Hyunji Kim, Yeaji Kim, Changhyeon Lee.
<br />
Hyunji Kim <a href="mailto:hjk021@khu.ac.kr"> <img src ="https://img.shields.io/badge/Gmail-EA4335.svg?&style=flat-squar&logo=Gmail&logoColor=white"/> 
[<img src="https://img.shields.io/badge/Notion-000000?style=flat-square&logo=Notion&logoColor=white"/>](https://read-me.notion.site/Hyunji-Kim-9dbdb62cc84347feb85b3c58225bb63b)
	<a href = "https://github.com/HJK02130"> <img src ="https://img.shields.io/badge/Github-181717.svg?&style=flat-squar&logo=Github&logoColor=white"/> </a>

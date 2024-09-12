# dacon-cancer-classification-competition
2024 생명연구자원 AI활용 경진대회 소스 코드

추가한 사항 -> 이거는 읽고 나중에 지우면 될 듯
1. 내 이름 폴더로 내가 관련된 파일 전부 넣어놨음
2. 확인하면 좋은 파일은 다음과 같음
   1. Baseline_model_comparision (1).ipynb
   이거는 처음에 만든 파일으로 기본적인 모델들을 단순한 인코딩(레이블 인코딩)을 이용해서 모델끼리의 성능 비교를 해본 것
   + 여기서 알아낸 것
     + 특정 모델이 특정 클래스를 잘 잡아냄
     + 하나의 feature에 띄어쓰기를 기준으로 multi feature를 갖는 경우가 있음
   1. automl (1).ipynb
   autogluon이라는 GPU를 사용하는 모델을 통해서 구해본 것, 여기 있는 input 데이터는 X_encoded_pca라는 건데, raw data에서 multi_label을 MultiBinaizer이라는 걸로 구분해서 총 feature을 완전 분리해냄, 마치 ohe처럼 (4385 features -> 20만 얼마로 늘어남)
   따라서 이렇게 늘어난 것을 줄이고자 pca로 특징을 3천개 정도로 줄이고 학습을 돌림(이유는 시간 절약 및 여러가지 이유가 있음)
   현재 성능 0.26~으로 나온 모델이 이 모델로 돌린 것임
   시간은 약 : 1시간정도

추가 정보
1. joblib이라는 파일은 모델을 학습하거나, 인코딩할때 시간이 굉장히 많이 들기 때문에 시간을 아끼고 이를 저장하는 용도로 사용되는 폴더임
모델 학습 : 2시간정도
데이터 인코딩 : 4시간 넘게 걸렸음 
2. 이런 코드를 내가 다 짰는가? -> GPT한테 프롬프팅 잘 먹여서 하면 됨. 나는 이제 프롬프팅은 저장해놓는 방식으로 사용하고 있음
Ex.
=====================
나는 'SUBCLASS'를 분류하기 위해 train data로 머신러닝 모델을 학습하고 test data로 분류한 submission.csv를 만드는 것이다. 실행환경은 GPU를 사용할 수 있는 로컬 컴퓨터이고 vscode에서 돌릴 코드를 알려줘.
data : train.csv, test.csv
data shape : train.csv(6201, 4385), test.csv(2546, 4384)
학습 방법 :
1. 데이터를 read_csv로 가져온다. index=ID로 설정해줘
2. target값이 될 'SUBCLASS' 열을 분리하고 보관해줘.
3. train data의 열이 범주형 multi value를 갖고 있기 때문에 인코딩이 필요하다. 따라서 범주형과 숫자형 열을 구분해서, 범주형 열을 각기 인코딩을 해야한다. MultiLabelBinarizer를 이용해서 열마다 인코딩을 하고 그에 대한 MultiLabelBinarizer객체를 저장해놓도록 한다. 추가로 multi value는 띄어쓰기로 구분되어 있으며, 분리하고 나서 숫자형 값으로 나오는 것도 범주형으로 바꿔서 진행할 수 있도록 해줘. 인코딩된 값은 새로운 데이터프레임에 저장할 수 있도록 해.
4. 이전에 인코딩한 데이터프레임과 'SUBCLASS'열을 갖고 train_test_split으로 분리를 하는데 stratify=y으로 설정해줘
5. random forest모델을 이용해서 학습하고 검증 데이터를 평가하는 것을 보여줘.
6. 학습된 모델은 joblib을 이용해서 저장해줘.
7. 테스트 데이터의 각 열을 저장해놓은 인코딩하고 저장된 모델을 사용해서 inference하고 [ID, SUBCLASS]이 있는 submisiion.csv를 만들어줘.
=====================
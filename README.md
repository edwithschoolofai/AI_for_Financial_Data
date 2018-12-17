# 개요

이 코드는 Siraj Raval의 다음 유튜브 [영상](https://www.youtube.com/watch?v=UNgdIkuVC6g)을 위한 코드입니다. 익명의 신용 카드 거래를 기반으로 한 이상 거래 탐지 모델입니다.

## 시작하기
이상 거래 탐지 POST endpoint를 노출하는 마이크로서비스를 설정하려면, 다음 단계별로 따라하세요:

1. repository에서 코드를 받아옵니다.
```
git clone https://github.com/cloudacademy/fraud-detection.git 
```
2. 거래 분류기 학습에 사용될 다음 [데이터셋](https://clouda-datasets.s3.amazonaws.com/creditcard.csv.zip)을 다운받으세요. 압축을 풀고 컨텐츠(creditcard.csv)를 폴더 안에 넣습니다.

3. 가상 환경을 생성한 후 (예를 들어, fraud-detection 라는 이름으로 설정), 활성화하고 필요한 파이썬 패키지를 모두 받아옵니다.
```
pip install -r requirements-dev.txt
```
이 repo와 연관된 test를 실행할 필요가 없으시다면 다음 작업만 하면 됩니다.
```
pip install -r requirements.txt
```
그렇지 않다면

4. repo root에서 이상 거래 탐지 모델을 위한 학습을 실행합니다.
```
python src/train.py 
```
학습의 발전 단계, 최적화되는 파라미터 그리고 다양한 경우에서 얻은 품질 메트릭에 대한 정보를 보여줍니다. 이 단계는 models 폴더 아래 model.pickle 파일을 생성하며 작업을 마칩니다.

5. Flask app을 실행합니다.
```
export FLASK_APP=src/flask_app.py
flask run
```
이후에, POST endpoint는 http://127.0.0.1:5000/ 에 나타납니다. 다음과 같은 형태의 application/json body를 보낼 수 있습니다.
```
{
    "features": [
    	[0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],
    	[1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0]
    ]
}
```
즉, 키 "features"의 값은 30개의 float 값으로 구성된 리스트이며, 거래와 연관된 값을 나타냅니다.
다음과 같은 JSON을 반환받습니다.
```
{
    "scores": [
        0.0323602000089039, 
        0.00037634905230425804
    ]
}
```
즉, 거래 당 1개의 사기일 확률 값 리스트를 제출합니다.

6. requirements-dev가 설치되고 나면, repo root에서 다음 작업을 통해 마이크로서비스를 위한 test를 실행할 수 있습니다.
```
nosetests
```

## 저작권

이 코드에 대한 저작권은 [cloudacademy](https://github.com/cloudacademy/fraud-detection)에 있습니다. 저는 처음 시작하는 사람들을 위해 조금 변형했을 뿐입니다.

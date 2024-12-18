![image](https://github.com/user-attachments/assets/49df589a-1451-411f-84bb-c52922a5d0e0)# 기말고사 프로젝트

# 동기
프로젝트 추천 아이디어를 보던 중, Recommender System라는 주제가 흥미로워서 이 아이디어를 응용하였다. 여자친구와 데이트 코스를 정할 때, 무엇을 하고 무엇을 먹을 지 고민을 하는 경우가 많다. 이때 도움을 주기 위해서 랜덤으로 데이트 코스를 계획해주고 고른 데이트 코스의 데이터를 학습해서 데이트 코스를 추천하는 프로그램을 고안하였다.

# 데이터 획득 방법
데이터 획득의 경우 여자친구와 여러번 프로그램을 돌려보면서 획득하였다.

# 모델
![image](https://github.com/user-attachments/assets/91ee6de0-3044-447b-be9b-6970e477f5b9)
import random은 데이트 코스 추천 시 카테고리를 정하면 그 중에서 랜덤으로 고르기 위해 사용하였다. 
import requests는 웹 API와 상호작용하기 위해 HTTP 요청을 보내기 위해 사용하였다.
import json은 JSON(JavaScript Object Notation) 형식의 데이터를 다루기 위해 사용하였다. 특히 추천한 데이터를 JSON파일로 저장하는 것이 핵심 기능이다. 
from datetime import datetime은 날짜와 시간을 다루기 위해 사용하였다. 
import torch는 딥러닝 모델을 구축하고 학습하기 위한 라이브러리이다.
import torch.nn as nn은 신경망 모델을 정의하고 구축한다.
import torch.optim as optim은 모델 학습을 위한 옵티마이저이다.
이 기능들을 활용하여 데이트 코스 추천 프로그램을 코딩하였다. 

![image](https://github.com/user-attachments/assets/fb994e08-0a1a-4407-974a-0171292353ff)
API_KEY는 openweathermap에서 날씨 정보를 불러오기 위해 사용하였고, 계정을 따로 생성하여 나만의 API를 생성했다.
RECORD_FILE은 JSON 파일의 경로를 문자열로 지정한다.
KOREAN_CITIES의 경우 날씨를 조사하기 위해 정리하였고, 데이트를 할 만한 한국의 도시들의 목록이다.
activity_sub은 관람, 액티비티, 실내, cuisine_sub은 한식, 중식, 양식, 일식으로 나누었으며 평소에 데이트하는 코스를 반영하였다.

![image](https://github.com/user-attachments/assets/87565617-8e55-427a-83bb-476590183dc6)
map_weather_to_id는 날씨 정보를 문자열 형태에서 정수 ID로 변환하는 역할을 한다.

![image](https://github.com/user-attachments/assets/ff20e2dc-d76b-4f7b-b3f3-7ce7014b90e0)
RecommendationModel의 경우 PyTorch의 nn.Module을 반영한다.
__init__는 nn.Embedding으로 범주형 데이터를 고차원 벡터로 변환한다. nn.Linear는 입력과 출력을 연결하는 완전 연결 레이어로 학습 가능한 가중치와 바이어스를 포함한다. forward에서  각각의 정수 ID를 nn.Embedding을 통해 벡터로 변환한다. torch.cat에서는 활동, 음식, 날씨 임베딩을 하나의 벡터로 연결하고 torch.relu에서 활성화 함수로 ReLU(Rectified Linear Unit)를 사용해서 비선형성을 추가하여 모델의 학습 능력을 향상한다. output는 최종 완전 연결층을 통해 활동 대분류의 예측값을 반환한다.

![image](https://github.com/user-attachments/assets/1f190c38-5f62-4d3c-8b77-30ec91c5fe38)
get_user_preferences에서 "오늘 데이트는 어떻게 할까?"와 같은 메시지를 출력하여 사용자에게 입력을 유도한다. 이때 평소에 여자친구와 쓰는 말투를 이용해서 현실에서 사용하는 것 같은 느낌을 주었다. 데이트 장소와 활동, 음식을 물어보는 질문을 포함한다. 

![image](https://github.com/user-attachments/assets/868c7de6-36ff-461a-9514-c23eec0aaaba)
get_weather에서는 OpenWeatherMap API URL을 이용하여 도시의 온도를 확인한다. 이 API 응답 데이터를 JSON 형식으로 변환하여 저장한다. 예외의 경우 평소 말투를 활용하여 기본값으로 반환한다.

![image](https://github.com/user-attachments/assets/5a84e063-0c34-4267-8e9e-951e13c67355)
train_model은 PyTorch를 활용하여 학습하는 코드이다. model은 학습할 PyTorch 모델, data는 학습 데이터, epochs는 학습 반복 횟수, learning_rate는 옵티마이저의 학습률이다. criterion은 손실함수로



# 퍼포먼스
OOO 기사의 내용을 요약하여 정리한 결과이다.

내가 원하는 기능은 수입과 수출을 카테고리를 직접 작성하여서 날짜별로 저장하는 것이며 이 기록을 원하는 달에 한해서 다 삭제할수 있게 하는 것이다. 이 기능이 코드에서 1,2,3번의 내용이다. 또한 가계부를 한 번에 만들고 싶어서 만든 코드가 4번이고 5,6,7번의 경우는 달별 평균 수입, 수출, 달별 저축률, 달별 소비율을 보고 싶어서 만든 기능이다. 따라서 내가 원하는 기능만을 가진 나만의 가계부가 완성되었다.
이 아이디어를 응용해서 어플을 만들어서 사용하면 좋을 것 같다는 생각이 들었다. 더 나아가서 많은 기능들을 가진 코드를 만들고 이용자마자 각자 이용하고 싶은 기능을 고르면 그 기능들만 가지고 있는 어플을 만들면 많은 사람들이 만족하면서 어플을 사용할 수 있을 것 같다는 생각이 들었다.

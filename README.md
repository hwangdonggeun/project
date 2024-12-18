# 기말고사 프로젝트

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

train_model은 PyTorch를 활용하여 학습하는 코드이다. model은 학습할 PyTorch 모델, data는 학습 데이터, epochs는 학습 반복 횟수, learning_rate는 옵티마이저의 학습률이다. criterion은 손실함수로 모델의 출력(예측값)과 실제 타겟값(정답)을 비교해 손실 값을 계산한다. optimizer는 학습률 조정과 모멘텀을 통해 효율적인 학습을 지원한다. epochs는 데이터셋을 몇 번 반복 학습할지 설정한다. torch.tensor에서 입력 데이터를 PyTorch 텐서로 변환한다. optimizer.zero_grad에서 이전 학습 단계에서 계산된 그래디언트를 초기화한다. 그래디언트는 모델 가중치 업데이트에 사용되며, 초기화하지 않으면 이전 값이 누적될 수 있다. loss.item 손실 값을 스칼라 값으로 변환한 것이다. 위 계산이 끝나면 값을 출력하는데 손실 값이 점점 감소하면 모델이 학습되고 있음을 의미한다.

![image](https://github.com/user-attachments/assets/291061a2-50b9-4340-b23e-2317943634a1)

predict에서 모델이 예측한 활동 대분류(정수 ID)를 반환한다. model.eval에서 모델을 추론 모드로 전환하고 학습 중 사용하는 드롭아웃 및 배치 정규화 같은 레이어를 비활성화하여 추론 모드에서 모델의 출력값이 안정적이고 일관되게 계산한다. 입력 데이터를 PyTorch 텐서로 변환하기 위해 torch.tensor를 사용하였다. 즉, 정수형 데이터를 PyTorch 텐서로 변환하는 것이다. torch.no_grad에서 그래디언트를 계산하지 않도록 설정하고 추론 시에는 역전파가 필요하지 않으므로 메모리 사용량과 계산 시간을 절약하게 한다. predicted_class에서 출력값에서 가장 높은 값을 가지는 클래스의 인덱스를 선택하여 모델이 예측한 활동 대분류의 클래스 ID를 반환한다.

![image](https://github.com/user-attachments/assets/0e6dc715-9dc0-40d8-8132-41cd8ee883ee)

load_records에서 JSON 파일에 저장된 추천 기록을 불러와 Python 객체로 변환하고 파일이 없거나 손상되었을 경우, 오류 메시지를 출력하거나 빈 리스트를 반환하여 프로그램이 정상적으로 작동하도록 처리한다. open에서 파일을 읽기 모드로 열고, UTF-8 인코딩을 사용한다. json.load에서 JSON 형식의 파일 내용을 읽어 Python 객체로 변환한. 파일이 없을 경우 FileNotFoundError에서 빈 리스트를 반환하여 데이터가 없는 상태를 나타내고 프로그램이 계속 실행될 수 있도록 안전하게 처리한다.

![image](https://github.com/user-attachments/assets/576ed5b5-f5d8-4821-8648-a7183a9cf890)

save_records에서는 주어진 records 데이터를 JSON 형식으로 파일에 저장한다. open에서 RECORD_FILE 파일을 쓰기 모드로 열고 UTF-8 인코딩을 사용한다. json.dump에서 Python 객체(records)를 JSON 형식으로 변환하여 파일에 저장한다.
save_recommendation에서는 새로운 추천 기록(record)을 기존 기록에 추가한 후 저장한다. load_records() 함수를 호출해 JSON 파일에 저장된 기존 추천 기록을 불러온다. records.append에서 새로운 기록을 기존 기록 리스트에 추가한다. save_records에서는 업데이트된 기록 리스트를 JSON 파일에 저장한다. 

![image](https://github.com/user-attachments/assets/1bd6bd6f-c6a2-414c-b72f-bc1ad273402c)

prepare_training_data은 추천 기록 데이터를 기반으로 모델 학습에 사용할 데이터를 준비하는 역할을 한다. 저장된 JSON 기록을 읽어 입력 데이터를 전처리하고, 모델이 학습할 수 있는 형태로 변환한다. records = load_records은 JSON 파일에서 추천 기록을 불러오는 함수이다. activity_map과 cuisine_map에서 각각의 항목을 정수 ID로 매핑한다. 이 data를 record에서 추천 기록 리스트의 각 요소. 딕셔너리 형태로 활동, 음식, 날씨 등의 정보를 포함하게 한 후, activity에서 활동 이름을 정수 ID로 변환한다. 이 ID들을 data.append에서 data에 추가하고 반환한다.

![image](https://github.com/user-attachments/assets/8804ae0f-c0e0-4df1-a0b6-2f6d8b29a328)

recommend_course_with_model에서는 딥러닝 모델을 사용하여 활동 대분류를 예측하고 예측된 활동과 입력받은 음식 대분류를 기반으로 세부 활동과 음식을 추천한다. 각각의 map들에서 활동과 음식을 정수 ID로 mapping하고 정수 ID를 문자열로 변환하기 위해 역으로 mapping한다. predicted에서는 RecommendationModel을 사용해 활동 대분류를 예측하고 recommended_activity에서 모델의 예측 결과를 활동 문자열로 변환한다. 정해진 대분류에서 세부 항목의 경우 랜덤으로선택하는 random.choice를 사용한다. 아직까지는 빅데이터의 자료가 학습률이 낮기 때문에 이러한 방법을 사용하였고, 시간이 지나면 세부항목도 추천하는 식으로 발전할 수 있는 요소가 있다.

![image](https://github.com/user-attachments/assets/f95dea0c-d575-4869-a19f-0a25c1ca69a2)

recommend_course_dl_only는 딥러닝 모델을 사용하여 랜덤 입력값을 기반으로 추천 결과를 생성하는 코드이다. 활동, 음식, 날씨 데이터를 랜덤으로 설정하고, 이를 딥러닝 모델에 전달하여 추천된 활동 대분류를 기반으로 세부 활동과 음식 메뉴를 랜덤하게 선택한다. 각각의 항목을 리스트로 정리하고 딥러닝으로 추천받은 대분류를 이용하여 세부 항목을 random.choice에서 골라서 추천한다.

![image](https://github.com/user-attachments/assets/059418bf-a968-4d93-8a68-3cefc1fa315b)
![image](https://github.com/user-attachments/assets/0d4e8666-7c7b-427e-a53d-59cd937c94b2)

위에서 학습한 결과들을 산출하는 과정이다. 즉, 메인 프로그램으로 데이트 추천 프로그램을 실행하는 흐름을 관리한다. 사용자가 입력을 통해 추천을 받을지, 아니면 랜덤 추천을 받을지 선택할 수 있다. prepare_training_data를 호출해서 학습 데이터를 준비한다. 데이터가 있으면 모델 학습을 시작하고, 진행 상황을 콘솔에 출력한다. while 반복문을 통해 사용자가 무엇을 할지 선택하도록 메뉴를 보여준다. 1의 경우 도시, 활동, 음식 데이터를 입력받아서 날씨 정보를 API를 통해 가져와서 현재 날씨와 온도를 출력한다. 이후 딥러닝 모델로 활동 대분류를 예측하고, 세부 활동과 음식 메뉴를 랜덤으로 골라서 출력한다. 2에서는 랜덤 활동과 음식을 생성하고, 딥러닝 모델로 활동 대분류를 예측한다. 이후 랜덤으로 세부 활동과 음식을 골라 추천 결과를 출력한다.


# 퍼포먼스
다음은 여자친구와 함께 코드를 파이썬에서 돌려본 결과의 일부이다.

![image](https://github.com/user-attachments/assets/933b35c2-b81a-49f7-988a-ace21dac266d)


소감 작성
내가 만든 코드를 실제로 사용할 수 있어서 매우 뿌듯했다. 딥러닝이라는 기술이 정말 대단하다고 생각했고, 이를 통해 많은 분야에 기여할 수 있다고 느꼈다. 이 코딩도 점점 발전을 할 수 있는 여지가 많다고 생각했다. 더 많은 코스, 추천 맛집 소개 등등 데이터를 학습하여 딥러닝 할 수 있고 이를 활용하여 많은 사람들이 이 코드를 사용하면 좋겠다고 생각한다.

# 기말고사 프로젝트

# 동기
중학생때는 손을 이용하여 가계부를 사용하였고 요즘은 어플을 이용하여 가계부를 사용한다. 손으로 사용하는 가계부의 경우 계산을 직접해야 한다는 불편함이 있고 어플의 경우 다양한 어플이 있지만 내가 원하는 기능만 가지고 있는 어플은 없었다. 따라서 python을 이용하여 내가 원하는 기능만을 가지고 있는 가계부를 만들어 보았다.

# 데이터 획득 방법
데이터는 6월달 내가 쓴 지출내역, 수입내역의 일부를 이용하여 획득하였고 개인 노트북에서 이 코드를 활용해보았다.

# 모델
import os  Python 표준 라이브러리 모듈인 os를 불러오는 명령이다. os 모듈은 운영 체제와 상호작용할 수 있는 기능을 제공하며 이 모듈을 사용하면 파일과 디렉터리를 조작하고, 환경 변수를 가져오거나 설정하며, 프로세스 관련 작업을 수행하는 등의 다양한 기능을 사용할 수 있어서 사용했다.
이 코드에서는 os.path.join, os.path.expanduser, os.path.exists, os.makedirs 에서 사용된다.

import pickle 은 Python의 pickle 모듈을 불러오는 명령이다. pickle 모듈은 Python 객체의 직렬화와 역직렬화를 지원한다. 즉, Python 객체를 바이트 스트림으로 변환하여 파일에 저장하거나 네트워크를 통해 전송할 수 있으며, 반대로 바이트 스트림을 원래의 Python 객체로 복원할 수 있어서 사용했다. 
이 코드에서는 pickle.load, pickle.dump 에서 사용된다. 

from datetime import datetime 은 Python 표준 라이브러리의 datetime 모듈에서 datetime 클래스를 불러오는 명령이다. datetime 모듈은 날짜와 시간을 조작하는 다양한 클래스와 함수를 제공한다. 이 모듈을 사용하면 날짜와 시간에 관련된 작업을 쉽게 수행할 수 있어서 사용했다.
이 코드에서는 날짜가 많이 나오기 때문에 많은 곳에서 등장할 예정이다.


![image](https://github.com/hwangdonggeun/project/assets/172406535/2d310483-53f5-43de-b0c2-9b1b57b8b3bd)

사용자의 홈 디렉토리에 budget 폴더를 생성하거나, 이미 존재하는 경우 해당 폴더의 경로를 반환하기 위한 함수이며 os를 이용하여 가계부의 데이터를 저장하는 저장고를 만들었다.


![image](https://github.com/hwangdonggeun/project/assets/172406535/a0bb5751-00f3-4cdd-bcaf-edabac6aba50)

먼저, get_budget_folder_path 함수를 호출하여 예산 폴더의 경로를 가져온다. 이후 budget_folder_path와 transactions.pkl 파일명을 결합하여 거래 내역 파일의 전체 경로를 만든다. os.path.exists를 이용하여 transactions.pkl 파일이 존재하는지 확인하고 파일이 존재할 경우, 아래의 코드를 실행한다. 이후 transactions_file을 읽기 모드('rb')로 연다음, pickle.load(file)을 사용하여 파일에서 거래 내역을 불러온다. 이후 updated = False 부분에서 updated 변수를 False로 초기화하고 transactions 리스트의 각 거래를 순회한다. if함수를 이용해서 거래에 category 필드가 없는 경우, category 필드를 'Uncategorized'로 설정하고, updated 변수를 True로 변경한다. 즉, category 필드가 없는 모든 거래에 대해 기본 값을 추가하는 함수라고 할 수 있다. 이후 업데이트된 내용에 대한 코드가 if updated인데 만약 하나 이상의 거래가 업데이트되었다면 (updated 변수가 True인 경우), 아래의 코드를 실행하고transactions_file을 쓰기 모드('wb')로 연다. 이후 pickle.dump(transactions, file)을 사용하여 업데이트된 거래 내역을 파일에 저장하고 "Existing transactions updated with default category." 메시지를 출력하여 업데이트가 완료되었음을 알린다.
정리하면 이 함수는 기존 거래 내역 파일을 열어 각 거래에 category 필드가 있는지 확인하고, category 필드가 없는 경우 기본 값으로 'Uncategorized'를 추가하는 기능을 한다. 업데이트된 거래 내역은 다시 파일에 저장되는데, 이 과정은 파일이 존재할 경우에만 수행된다.

![image](https://github.com/hwangdonggeun/project/assets/172406535/5218e3ba-133c-4285-bef0-45828295c175)

get_budget_folder_path 함수를 호출하여 예산 폴더의 경로를 가져온다. 이 함수는 사용자의 홈 디렉토리에 budget 폴더가 없으면 생성하고, 해당 폴더의 경로를 반환하는 함수이다. budget_folder_path와 transactions.pkl 파일명을 결합하여 거래 내역 파일의 전체 경로를 만든다. transactions_file이 존재하는지 확인하고 파일이 존재하지 않으면 빈 리스트 transactions를 초기화한다. 만약, 파일이 존재하면 파일을 읽기 모드('rb')로 열고, pickle.load(file)을 사용하여 파일에서 거래 내역을 불러온다. 불러온 거래 내역은 리스트 형태로 transactions 변수에 저장된다. 입력된 날짜 문자열 date_str을datetime.strptime(date_str, '%Y-%m-%d')을 사용하여 datetime 객체로 변환한다. 변환된 datetime 객체를 strftime('%Y-%m-%d %H:%M:%S')을 사용하여 문자열 형식으로 다시 변환한다. 변환 과정에서 ValueError가 발생하면 "Invalid date format. Please use YYYY-MM-DD." 메시지를 출력하고 함수를 종료다. 이후 새로운 거래를 나타내는 딕셔너리를 생성합니다. 이 딕셔너리는 다음과 같은 키와 값을 가진다.
'type': 거래 유형 (transaction_type)
'amount': 거래 금액 (amount)
'category': 거래 분류 (category, 기본 값은 'Uncategorized')
'date': 거래 날짜 (date)
생성된 거래 딕셔너리를 transactions 리스트에 추가한다. transactions_file을 쓰기 모드('wb')로 열고, pickle.dump(transactions, file)을 사용하여 업데이트된 거래 내역을 파일에 저장한다. 이후 새로운 거래가 추가되었음을 알리는 메시지를 출력한다. 메시지는 거래 유형을 대문자로 변환하여 (transaction_type.capitalize()), 거래 금액과 날짜, 분류 정보를 포함한다.
정리하면, 이 함수는 새로운 거래를 추가하는 기능을 수행한다. 먼저 예산 폴더 경로와 거래 파일 경로를 설정하고, 기존 거래 내역을 불러온다. 그런 다음 입력된 날짜 문자열을 검증하고 변환한 후, 새로운 거래를 생성하여 거래 리스트에 추가한다. 업데이트된 거래 내역은 파일에 저장되며, 성공 메시지가 출력된다.


![image](https://github.com/hwangdonggeun/project/assets/172406535/0067a3c8-54cb-48ea-807f-b7f88b9b3134)

get_budget_folder_path 함수를 호출하여 예산 폴더의 경로를 가져온다. 이후 budget_folder_path와 transactions.pkl 파일명을 결합하여 거래 내역 파일의 전체 경로를 만든다. transactions_file이 존재하는지 확인하고 만약 파일이 존재하지 않으면 "No transactions found." 메시지를 출력하고 함수를 종료한다. transactions_file을 읽기 모드('rb')로 열고, pickle.load(file)을 사용하여 파일에서 거래 내역을 불러온다. 불러온 거래 내역은 리스트 형태로 transactions 변수에 저장한다. category_report에서 딕셔너리를 초기화하여 각 카테고리별 수입과 지출을 저장할 공간을 만들고 total_income과 total_expense 변수를 0으로 초기화한다. 이후 모든 거래를 순회하면서 각 거래의 월(transaction['date'][:7])을 추출하여 target_month와 비교하고 target_month와 일치하는 거래에 대해 여러 작업을 한다. 거래의 카테고리를 가져오고 해당 카테고리가 category_report에 없으면 초기값으로 {'income': 0, 'expense': 0}을 설정한다. 거래 유형이 수입(income)이면 해당 카테고리의 수입과 total_income을 증가시킨다. 거래 유형이 지출(expense)이면 해당 카테고리의 지출과 total_expense를 증가시킨다. total_income과 total_expense가 모두 0이면 해당 월에 거래가 없음을 의미하며"No transactions found for {target_month}." 라는 메시지를 출력하고 함수를 종료한다. 각 카테고리에 대해 수입, 지출 및 잔액(수입 - 지출)을 출력하고 해당 월의 총 수입, 총 지출 및 전체 잔액(총 수입 - 총 지출)을 출력한다.
정리하면 이 함수는 특정 월(target_month)의 거래 내역을 불러와서, 각 거래를 카테고리별로 분류하여 수입과 지출을 집계하고 요약한다. 만약 해당 월에 거래가 없다면 적절한 메시지를 출력한고 거래가 있다면, 각 카테고리별 수입과 지출을 출력하고, 전체 수입, 지출 및 잔액을 출력하는 기능을 한.


![image](https://github.com/hwangdonggeun/project/assets/172406535/52c62e38-083e-4327-acf4-8a0d0ca1be9c)

get_budget_folder_path 함수를 호출하여 예산 폴더의 경로를 가져온다. 이 함수는 사용자의 홈 디렉토리에 budget 폴더가 없으면 생성하고, 해당 폴더의 경로를 반환한다. 이후 budget_folder_path와 transactions.pkl 파일명을 결합하여 거래 내역 파일의 전체 경로를 만든다. transactions_file이 존재하는지 확인하고 파일이 존재하지 않으면 "No transactions found." 메시지를 출력하고 함수를 종료한다. transactions_file을 읽기 모드('rb')로 열고, pickle.load(file)을 사용하여 파일에서 거래 내역을 불러온다. 불러온 거래 내역은 리스트 형태로 transactions 변수에 저장한다. 만약 target_month가 주어진 경우 transactions 리스트에서 각 거래의 월(transaction['date'][:7])이 target_month와 일치하지 않는 거래만 남긴다. 즉, target_month에 해당하는 모든 거래를 리스트에서 제거하고 "All transactions for {target_month} have been deleted." 메시지를 출력한다.
target_month가 주어지지 않은 경우에는 transactions 리스트를 빈 리스트로 초기화한다. 즉, 모든 거래를 삭제하고 "All transactions have been deleted." 메시지를 출력한다. 이후 transactions_file을 쓰기 모드('wb')로 열고, pickle.dump(transactions, file)을 사용하여 업데이트된 거래 내역을 파일에 저장한다.
정리하면 이 함수는 특정 달(target_month)의 거래를 삭제하거나, target_month가 주어지지 않으면 모든 거래를 삭제하는 기능을 수행한다. 먼저 예산 폴더 경로와 거래 파일 경로를 설정하고, 기존 거래 내역을 불러온다. 그런 다음, target_month가 주어진 경우 해당 월의 거래를 삭제하고, 그렇지 않으면 모든 거래를 삭제한다. 마지막으로, 업데이트된 거래 내역을 파일에 저장하고 적절한 메시지를 출력한다.

![image](https://github.com/hwangdonggeun/project/assets/172406535/40cae781-c93e-4821-ab3e-e5ca7c0eeb5b)

get_budget_folder_path 함수를 호출하여 예산 폴더의 경로를 가져오고 budget_folder_path와 transactions.pkl 파일명을 결합하여 거래 내역 파일의 전체 경로를 만든다. transactions_file이 존재하는지 확인하고 만약 파일이 존재하지 않으면 "No transactions found." 메시지를 출력하고 함수를 종료한다. transactions_file을 읽기 모드('rb')로 열고, pickle.load(file)을 사용하여 파일에서 거래 내역을 불러온다. 불러온 거래 내역은 리스트 형태로 transactions 변수에 저장한다. monthly_data에서 딕셔너리를 초기화하여 해당 월의 수입과 지출을 저장할 공간을 만든다. 모든 거래를 순회하면서 각 거래의 월(transaction['date'][:7])을 추출하여 target_month와 비교한다. target_month와 일치하는 거래에 대해 거래 유형이 수입(income)이면 monthly_data['income']을 증가시킨다. 만약 거래 유형이 지출(expense)이면 monthly_data['expense']를 증가시킨다. 그리고 해당 월의 총 수입과 총 지출을 출력한다.
정리하면 이 함수는 특정 월(target_month)의 총 수입과 총 지출을 계산하여 출력한다. 먼저 예산 폴더 경로와 거래 파일 경로를 설정하고, 기존 거래 내역을 불러온다. 그런 다음, target_month에 해당하는 거래들을 필터링하여 수입과 지출을 합산한다. 마지막으로, 해당 월의 총 수입과 총 지출을 출력한다.

![image](https://github.com/hwangdonggeun/project/assets/172406535/20554f62-aad4-4678-8373-3ed8efcd603f)

get_budget_folder_path 함수를 호출하여 예산 폴더의 경로를 가져온다. budget_folder_path와 transactions.pkl 파일명을 결합하여 거래 내역 파일의 전체 경로를 만든다. transactions_file이 존재하는지 확인한다. 만약 파일이 존재하지 않으면 "No transactions found." 메시지를 출력하고 함수를 종료한다. transactions_file을 읽기 모드('rb')로 열고, pickle.load(file)을 사용하여 파일에서 거래 내역을 불러온다. 불러온 거래 내역은 리스트 형태로 transactions 변수에 저장된다. total_income과 total_expense의 변수를 0으로 초기화한다.
모든 거래를 순회하면서 각 거래의 월(transaction['date'][:7])을 추출하여 target_month와 비교하고
target_month와 일치하는 거래에 대해 거래 유형이 수입(income)이면 total_income을 증가시킨다. 만약
거래 유형이 지출(expense)이면 total_expense를 증가시킨다. 만약 total_income이 0이면 해당 월에 수입이 없음을 의미며 "No income to calculate savings rate." 라는 메시지를 출력하고 함수를 종료한다.
savings_rate에서 저축률을 계산한다. 저축률의 식은 다음과 같다.
savings_rate = ((total_income - total_expense) / total_income) * 100
이후 저축률을 소수점 두 자리까지 한다. 출력 형식은 "Savings Rate for {target_month}: {savings_rate:.2f}%"이다.
정리하면 이 함수는 특정 월(target_month)의 저축률을 계산하여 출력한다. 먼저 예산 폴더 경로와 거래 파일 경로를 설정하고, 기존 거래 내역을 불러온다. 그런 다음, target_month에 해당하는 거래들을 필터링하여 총 수입과 총 지출을 합산한다. 수입이 없는 경우 적절한 메시지를 출력하고 함수를 종료하며, 수입이 있는 경우 저축률을 계산하여 출력한다.

![image](https://github.com/hwangdonggeun/project/assets/172406535/748848d1-b0be-4160-841d-677bab3986fe)

get_budget_folder_path 함수를 호출하여 예산 폴더의 경로를 가져온다. budget_folder_path와 transactions.pkl 파일명을 결합하여 거래 내역 파일의 전체 경로를 만든다. transactions_file이 존재하는지 확인한다. 만약 파일이 존재하지 않으면 "No transactions found." 메시지를 출력하고 함수를 종료한다. transactions_file을 읽기 모드('rb')로 열고, pickle.load(file)을 사용하여 파일에서 거래 내역을 불러온다. 불러온 거래 내역은 리스트 형태로 transactions 변수에 저장다. total_expense에서 total_expense 변수를 0으로 초기화하고 category_expense 딕셔너리를 초기화하여 각 카테고리별 지출을 저장할 공간을 만든다. 모든 거래를 순회하면서 각 거래의 월(transaction['date'][:7])을 추출하여 target_month와 비교하고 target_month와 일치하며 거래 유형이 지출(expense)인 거래에 대해total_expense를 해당 거래의 금액만큼 증가시키고 해당 거래의 카테고리를 가져오고 해당 카테고리가 category_expense에 없으면 초기값으로 0을 설정하며 해당 카테고리의 지출 금액을 증가시킨다. total_expense가 0이면 해당 월에 지출이 없음을 의미하며 "No expenses to calculate category spending ratio." 라는 메시지를 출력하고 함수를 종료한다. 각 카테고리에 대해 지출 비율을 계산합니다. 지출 비율은 다음과 같이 계산된다. ratio = (amount / total_expense) * 100 이후 각 카테고리별 지출 비율을 소수점 두 자리까지 출력한다. 출력 형식은 "Category: {category} - Spending Ratio for {target_month}: {ratio:.2f}%"이다.
정리하면 이 함수는 특정 월(target_month)의 각 카테고리별 지출 비율을 계산하여 출력한다. 먼저 예산 폴더 경로와 거래 파일 경로를 설정하고, 기존 거래 내역을 불러온다. 그런 다음, target_month에 해당하는 지출 거래들을 필터링하여 총 지출과 각 카테고리별 지출을 합산한다. 지출이 없는 경우 적절한 메시지를 출력하고 함수를 종료하며, 지출이 있는 경우 각 카테고리별 지출 비율을 계산하여 출력한다.

![image](https://github.com/hwangdonggeun/project/assets/172406535/2549cd81-00df-47bc-a482-0761028444f9)

while 문을 이용하여 8을 눌러서 코드를 종료하기 전까지 계속 기존의 만든 코드를 이용하는 가계부를 실행하는 기능이다. 소득 추가, 지출 추가, 거래 삭제, 카테고리 보고서 생성, 월별 평균 계산, 저축률 계산, 카테고리별 지출 비율 계산 등의 작업을 선택할 수 있다. 프로그램은 유효한 선택이 입력될 때까지 계속해서 사용자 입력을 받으며, '8'을 선택하면 종료된다.



# 퍼포먼스
내 수입, 수출중 일부 데이터를 활용하여 만들결과의 예시이다. 
이 아이디어를 응용해서 어플을 만들어서 사용하면 좋을 것 같다. 더 나아가서 많은 기능들을 가진 코드를 만들고 이용자마자 각자 이용하고 싶은 기능을 고르면 그 기능들만 가지고 있는 어플을 만들면 많은 사람들이 만족하면서 어플을 사용할 수 있을 것 같다는 생각이 들었다.

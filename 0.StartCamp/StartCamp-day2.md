# StartCamp-day2
## CLI= command line interface   (항상 자신이 어디에 있는지 확인!!)
## Git-bash 명령어
	ls 	  	리스트 보여주기
	ls -a	숨김파일 까지 다 보여줌(관리자 권한??)
	pwd
	mkdir	디렉토리 생성
	touch 	파일생성
	cd		디렉토리에 들어감
	cd .. 	상위 폴더로 나온다
	rm -r	폴더 지우기
	rm		파일 지우기
	echo 	메아리?
	tab키   자동완성
	code .	현재 폴더에서 비주얼 스튜디오코드 열어
## 다양한 파이썬 기능들
### 모듈가져오기
import xxx -외부 모듈을 불러옮
#### webbrowser  
webbrowser.open("xxx") - xxx 웹 열기
#### pip
pip install xxx -확장기능 설치

### 외부기능 사용
BeautifulSoup - 예쁘게 바꿔준다(우리눈엔 그대로 파이썬이 보기쉽게)
	ex)	from bs4 import BeautifulSoup

soup.select_one 	첫번째만 가져옮
soup.select		      부합하는거 다 가져옮
#### selector의 # = html의 id #

## 	Git-hub에 올리기
	git init				초기설정
	git add . 				커밋할 목록에 추가
	git commit -m "메시지"   커밋을 만들고 메시지 추가
	git push origin master	현재까지의 역사가 기록되어 있는 곳에 새로 생성한 커밋들 반영하기
	git pull origin master	가져오기

## 오늘의 파이썬 코딩
### browser.py

```python
import webbrowser
# 크롤링하기
url = "https://search.naver.com/search.naver?sm=top_hty&fbm=0&ie=utf8&query="
my_keywords= ["iu","bts","전소미","기아타이거즈"]

for i in my_keywords:
    webbrowser.open(url+i)
```

### exchnge.py

```python
import requests
from bs4 import BeautifulSoup

url = "https://finance.naver.com/marketindex/exchangeList.nhn"
response = requests.get(url).text
soup = BeautifulSoup(response, 'html.parser')

tr = soup.select('tbody > tr')
for r in tr:
    print(r.select_one('.tit').text.strip())
    print(r.select_one('.sale').text)
    

```

### file_change.py

```python
import os

os.chdir(r"C:\Users\student\StartCamp\students")
for filename in os.listdir("."):
    os.rename(filename, filename.replace("SAMSUNG_", "SSAFY_"))

```

### files.py

```phthon
from faker import Faker
import os

faker = Faker('ko_KR')
for i in range(100):
    filename = f"{i}_{faker.name()}.txt"
    cmd = f"touch {filename}"
    os.system(cmd)
```

### kospi.py

```python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://finance.naver.com/sise/").text
soup= BeautifulSoup(response, 'html.parser')
kospi = soup.select_one('#KOSPI_now')
print(kospi.text)

```

### naver.py

```python
import requests
from bs4 import BeautifulSoup

response = requests.get("http://naver.com").text
soup = BeautifulSoup(response, "html.parser")
naver = soup.select_one("#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul:nth-child(5) > li:nth-child(1) > a.ah_a > span.ah_k")
print(naver.text)
```




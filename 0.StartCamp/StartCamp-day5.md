# StartCamp-5일차
## NAVER api 를 이용한 Telegram 챗봇 만들기

### 

@app.route('/write')

def write():

​    return render_template("write.html")



@app.route('/send')

def send():

​    msg = request.args.get('msg')

​    url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"

​    res = requests.get(url)

​    return render_template("send.html")

### 서버구동

test.py

```python
import requests
from decouple import config
token = config("TELEGRAM_TOKEN")
url = f"https://api.telegram.org/bot{token}/"

user_id = config("CHAT_ID")

#send_url =f"{url}sendMessage?chat_id={user_id}&text=하이하이"
#requests.get(send_url)

ngrok_url= "https://seonwoongnam.pythonanywhere.com"
webhook_url = f"{url}setWebhook?url={ngrok_url}/{token}"
print(webhook_url)
```

### 환경변수

* 나의 토큰과 ID, SECRET 정보를 감추기 위해 생성한다.

  .env

##  JSON(JavaScript Object Notation)이란?

- JSON은 경량(Lightweight)의 DATA-교환 형식
- Javascript에서 객체를 만들 때 사용하는 표현식을 의미한다.
- JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다.
- 특정 언어에 종속되지 않으며, 대부분의 프로그래밍 언어에서 JSON 포맷의 데이터를 핸들링 할 수 있는 라이브러리를 제공한다.

### 2. JSON(JavaScript Object Notation) 형식

#### 2.1 name-value 형식의 쌍(pair)

- 여러 가지 언들에서 object, hashtable, struct로 실현되었다.
- { String key : String Value}

```
{
  "firstName": "Kwon",
  "lastName": "YoungJae",
  "email": "kyoje11@gmail.com"
}
```

#### 2.2 값들의 순서화된 리스트 형식

- 여러 가지 언어들에서 배열(Array), 리스트(List)로 실현되었다.
- [ value1, value2, ….. ]

```
{
  "firstName": "Kwon",
  "lastName": "YoungJae",
  "email": "kyoje11@gmail.com",
  "hobby": ["puzzles","swimming"]
}
```



## 오늘의 코딩

app.py

```python
from flask import Flask, request, render_template
from decouple import config
import requests
import random
from bs4 import BeautifulSoup
app = Flask(__name__)

api_url="https://api.telegram.org"
token = config("TELEGRAM_TOKEN")
chat_id = config("CHAT_ID")
naver_id = config("NAVER_ID")
naver_secret = config("NAVER_SECRET")

@app.route('/write')
def write():
    return render_template("write.html")

@app.route('/send')
def send():
    msg = request.args.get('msg')
    url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"
    res = requests.get(url)
    return render_template("send.html")

@app.route(f"/{token}", methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')

    if data.get('message').get('photo') is None: 
        if user_msg == "점심메뉴":
            menu_list= ["삼계탕","철판낙지볶음밥","물냉면"]
            result = random.choice(menu_list)
        
        elif user_msg[0:3] == "롤티어":
            lol_url = f"https://www.op.gg/summoner/userName={user_msg[3:]}"
    
            lol_html = requests.get(lol_url).text
            lol_soup = BeautifulSoup(lol_html, 'html.parser')
            lol_select = lol_soup.select_one('#SummonerLayoutContent > div.tabItem.Content.SummonerLayoutContent.summonerLayout-summary > div.SideContent > div.TierBox.Box > div > div.TierRankInfo > div.TierRank').text
            
            result=f'당신의 티어는{lol_select}입니다.'
        
        elif user_msg == "로또":
            lotto = sorted(random.sample(range(1,46),6))
            result = lotto 
        
        elif user_msg == "환율":
            url = "https://finance.naver.com/marketindex/?tabSel=exchange#tab_section"
    
            html = requests.get(url).text
            soup = BeautifulSoup(html, 'html.parser')
            select = soup.select_one('#exchangeList > li.on > a.head.usd > div > span.value')
    
            result=f'지금 1$는 {select.text}원 입니다.'
        
        elif user_msg[0:2] == "번역":
            # 번역 안녕하세요 저는 누구입니다.
            raw_text = user_msg[3:]
            papago_url = "https://openapi.naver.com/v1/papago/n2mt"
            data = {
            "source":"ko",
            "target":"en",
            "text":raw_text
            }
            header = {
            'X-Naver-Client-Id':naver_id,
            'X-Naver-Client-Secret':naver_secret
            }
            res = requests.post(papago_url, data=data, headers=header)
            translate_res = res.json()
            translate_result = translate_res.get("message").get('result').get("translatedText")
            print(res.text)
            result = translate_result   
        
        else : 
            result = user_msg
    else :
        # 사용자가 보낸 사진을 찾는 과정
        result="사진"
        file_id = data.get('message').get('photo')[-1].get('file_id')
        file_url = (f"{api_url}/bot{token}/getFile?file_id={file_id}")
        file_res = requests.get(file_url)
        file_path =  file_res.json().get('result').get('file_path')
        file =f"{api_url}/file/bot{token}/{file_path}"
        print(file)
    
        # 사용자가 보낸 사진을 클로버로 전송
        res = requests.get(file, stream=True)
        clova_url = "https://openapi.naver.com/v1/vision/celebrity"
        requests.post(clova_url)
        header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret
        }
        
        clova_res = requests.post(clova_url, headers=header, files={'image':res.raw.read()})
        
        if clova_res.json().get("info").get("faceCount"):
            # 누구랑 닮았는지 출력
            celebrity = clova_res.json().get('faces')[0].get('celebrity')
            name = celebrity.get('value')
            confidence = celebrity.get("confidence")
            result = f"{name}일 확률이 {confidence}입니다."
        else:
            # 사람이 없음
            result = "사람이 없습니다."
    
    res_url = f"{api_url}/bot{token}/sendMessage?chat_id={user_id}&text={result}"
    requests.get(res_url)
    return '', 200

if __name__=="__main__": 
    app.run(debug=True)
```

test.py

```python
import requests
from decouple import config
token = config("TELEGRAM_TOKEN")
url = f"https://api.telegram.org/bot{token}/"

user_id = config("CHAT_ID")

#send_url =f"{url}sendMessage?chat_id={user_id}&text=하이하이"
#requests.get(send_url)

ngrok_url= "https://seonwoongnam.pythonanywhere.com"
webhook_url = f"{url}setWebhook?url={ngrok_url}/{token}"
print(webhook_url)
```


```

```
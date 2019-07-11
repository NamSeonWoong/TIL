# StartCamp-4일차

##  flask를 이용한 웹 제작

### flask 기본형식

#### 서버 on 

python app.py

#### 기본 모듈 실행

 from flask import Flask, render_template, request

```html
@app.route("/hi")

def hi():

​    return "안녕하세요!!!"
```

- 위의 코드를 넣고 **templates** 폴더 생성 후 그 안에 html 파일 생성!!
- 

## HTML

### html 파일의 기본 구조

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<title> 간단한 예 </title>
</head>
<body>

<p>이 문서는 매우 간단한 예시이다! </p>

</body>
</html>
```






## 오늘의 코딩

```python
from flask import Flask, render_template, request
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

@app.route("/hi")
def hi():
    return "안녕하세요!!!"

@app.route("/html_tag")
def html_tag():
    return "<h1>안녕하세요</h1>"

@app.route("/html_tags")
def html_tags():
    return """
    <h1>안녕하세요</h1>
    <h2>안녕하세요.</h2>
    """

import datetime

@app.route("/dday") 
def dday():
    today = datetime.datetime.now()
    endday= datetime.datetime(2019,11,29)
    d = endday-today
    return f"1학기 종료까지 {d.days}일 남음!!"

@app.route("/html_file")
def html_file():
    return render_template('index.html')

@app.route("/greeting/<string:name>")
def greeting(name):
    return f"안녕하세요 {name}님!!"

@app.route("/cube/<int:num>")
def cube(num):
    a= pow(num,3)
    # num의 세제곱계산
    return f"{num}의 세제곱은 {a}입니다. "

@app.route("/cube_html/<int:num>")
def cube_html(num):
    cube_num = num**3
    return render_template("cube.html", num_html=num, cube_num_html=cube_num)#왼쪽은 html에서 사용하는 변수

@app.route("/greeting_html/<string:name>")
def greeting_html(name):
    return render_template("greeting.html", name=name)

import random
@app.route("/lunch")
def lunch():
    menu = {
        "짜장면" : "http://www.sporbiz.co.kr/news/photo/201904/329096_245779_5122.jpg",
        "스파게티" : "https://t1.daumcdn.net/cfile/tistory/203BDF3E4E9D975D36",
        "짬뽕" : "https://cdn.clien.net/web/api/file/F01/7342135/1ac2d7f7056553.jpg",
    }
    menu_list = list(menu.keys()) #["짜장면","스파게티","짬뽕"]
    pick = random.choice(menu_list)
    img = menu[pick]

    return render_template("lunch.html", pick=pick, img=img)

@app.route("/movies")
def movies():
    movie_list = ['스파이더맨','토이스토리','알라딘','존윅']
    return render_template("movies.html", movie_list=movie_list)

@app.route("/ping")
def ping():
    return render_template("ping.html")

@app.route("/pong")
def pong():
    user_input = request.args.get("test")
    # print(request.args.get("test"))
    # arguments = 매개변수들
    return render_template("pong.html", user_input=user_input)

@app.route("/naver")
def naver():
    return render_template("naver.html")

@app.route("/google")
def google():
    return render_template("google.html")

@app.route("/text")
def text():
    return render_template("text.html")

import requests
@app.route("/result")
def result():
    raw_text = request.args.get('raw')
    url = "http://artii.herokuapp.com/make?text="
    res = requests.get(url+raw_text).text
    return render_template("result.html", res=res)   

@app.route("/random_game")
def random_game():
    menu = {
        "짜장면" : "http://www.sporbiz.co.kr/news/photo/201904/329096_245779_5122.jpg",
        "스파게티" : "https://t1.daumcdn.net/cfile/tistory/203BDF3E4E9D975D36",
        "짬뽕" : "https://cdn.clien.net/web/api/file/F01/7342135/1ac2d7f7056553.jpg",
    }
    menu_list = list(menu.keys()) 
    pick = random.choice(menu_list)
    img = menu[pick]

    return render_template("random_game.html", pick=pick, img=img)

@app.route('/lotto')
def lotto():
    return render_template("lotto.html")

@app.route("/lotto_result")
def lotto_result():
    # 사용자가 입력한 정보 가져오기
    numbers = request.args.get('numbers').split()
    user_numbers = []
    for n in numbers:
        user_numbers.append(int(n))

    #로또 홈페이지에서 정보를 가져오기
    url = "https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=866"
    res = requests.get(url)
    lotto_numbers = res.json()

    winning_numbers = []
    for i in range(1,7):
        winning_numbers.append(lotto_numbers[f'drwtNo{i}'])
    bonus_number = lotto_numbers['bnusNo'] 
    bonus_numbers = [bonus_number]
    result = "1등"   

    matched = len(set(user_numbers) & set(winning_numbers))
    bonus = len(set(user_numbers) & set(bonus_numbers))

    if matched == 6:
        result = "1등"
    elif matched == 5 and bonus == 1:
        result = "2등"    
    elif matched == 5:
        result = "3등"     
    elif matched == 4:
        result = "4등"      
    elif matched == 3:
        result = "5등"
    else:
        result = "꽝!"

    return render_template("lotto_result.html", u=user_numbers, w=winning_numbers, b=bonus_number, r=result)

if __name__ == '__main__':
    app.run(debug=True)
```


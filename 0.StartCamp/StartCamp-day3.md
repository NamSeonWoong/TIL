# StartCamp-day3

## 파일 만들기

### open(), close()

```python
f= open("student.txt", 'w')
f.write("안녕하세요")
f.close
```

   'w' = write
	'r' = read
	'a' = 

### with open()

```python
# with 구문 완료시 자동 종료 되어 close() 구문 따로 사용안해도 됨!!
with open("ssafy.txt". 'w') as f
```
## CSS, HTML

### CSS

```css
/* 여기는 css파일입니다!!! */
    h1  {
        background-color:red;
    }
    a   {
        color:brown;
    }
    .blue {
        background-color:blue;
    }
    /* id가 git인것 */
    #git {
        background-color:black
    }
```



### HTML

```html
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="./intro.css">
    </head>
    <body>
            <h1>HTML</h1>
            <h1 id="git" class="blue">CSS</h1>
            <h2 class="blue">HyperText Markup Language</h2>
            <a href="https://naver.com">네이버</a>
            
            <!-- <태그이름 속성명="속성값" 속성명2="속성값2">내용</태그이름> --> 
            
            <h3 class="blue">우리가 공부한것 </h3>
            <ol>
                <li><strong>파이썬</strong></li>
                <li>HTML</li>
                <li>Git</li>
            </ol>>
    </body>
</html>
```

 <strong></strong>>  = 진하게

<a href=""></a>> = 사이트 링크	




## 오늘의 코딩

### exchange_csv.py

```python
import csv
import requests
from bs4 import BeautifulSoup

url = "https://finance.naver.com/marketindex/exchangeList.nhn"
response = requests.get(url).text
soup = BeautifulSoup(response, 'html.parser')

tr = soup.select('tbody > tr')
with open("naver_exchange.csv", "w", encoding="utf-8", newline="") as f:
    csv_writer = csv.writer(f)
    for r in tr:
            print(r.select_one('.tit').text.strip())
            print(r.select_one('.sale').text)
            row = [r.select_one('.tit').text.strip(), r.select_one('.sale').text]
            csv_writer.writerow(row)
```


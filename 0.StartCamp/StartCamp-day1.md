# startcamp-day1

## Python의 기초

### 입력과 출력
- 출력=print()
- 입력=input()
- 불러오기=import.xxx 
###  if문 (조건문)
- if/elif/else 
### while,for문
- Ex) while num <= 100:
- Ex) for i in range(4, 8):

#### range(a,b)
a부터 b-1까지
Ex
```python
range(0,46) #0부터 45까지
```
### random
- random.xxx

> choice:하나선택
> sample:비복원추출
> shuffle:섞는다
> 등등

로또번호생성
```python
import random

numbers = range(1,46)
lotto = random.sample(numbers, 6)
print('''이번주 행운의 번호!
''' + str(sorted(lotto)))

```
###  list와 dictionary
- list
```python
list['짜장면','스파게티',5,10]
```
- dictionary
```python
list['짜장면':'중국','스파게티':'이탈리아']
```
### 사칙연산 계산기
```python
num1=int(input("첫번째 숫자 입력"))
num2=int(input("두번째 숫자 입력"))

a=int(input("원하는 계산입력 (1=덧셈,2=뺄셈,3=곱셈,4=나눗셈) :"))

if(a==1):
    print("결과는 %d 입니다." %(num1+num2))
elif(a==2):
    print("결과는 %d 입니다." %(num1-num2))
elif(a==3):
    print("결과는 %d 입니다." %(num1*num2))
elif(a==4):
    print("결과는 %d 입니다." %(num1/num2))
else:
    print("계산할수 없음")
```

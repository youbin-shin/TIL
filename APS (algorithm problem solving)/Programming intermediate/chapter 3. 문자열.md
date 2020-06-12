# chapter 3. 문자열 string

> 2020.02.10 알고리즘_4

### **문자의 표현**

아스키코드

유니코드 인코딩 (Unicode Transformation Format)

- UTF - 8 (in web, python)
- UTF - 16 (in windows, java)
- UTF - 32 (in unix)

c 언어 - 문자열 = 문자배열 

			-  문자열 끝에 `\0` 으로 표시
			-   'a' 는 문자 (char) 로  "a" 이면 문자열로



### 문자열 비교

c strcmp() 함수 제공

java equlas() 메소드 제공

python ==연산자와 is 연산자 제공



### 문자열 숫자를 정수로 변환하기

c atoi() 함수제공 / 역함수 itoa()

java 숫자클래스의 parse 메소드 제공

python 숫자와 문자변환 함수 제공 int(), float()

```python
# c의 atoi() 알고리즘을 이용하여 python 숫자를 정수로 바꾸는 함수 만들기

def my_atoi(string):
    value = 0
    for i in range(len(string)):
        if ord('0') <= ord(string[i]) <= ord('9'):
            digit = ord(string[i]) - ord('0')
        else:
            print('error')
            break
        value = (value * 10) + digit
    return value

print(my_atoi('123'))
```

```python
# 숫자를 문자로 바꾸는 함수

def myitoa(x): # x = 123
    sr = ''
    while True:
        r = x % 10 # 3
        sr = sr + chr(r + ord('0'))
        x //= 10
        if x == 0:break
            
	s = ''
    for i in range(len(sr)-1, -1, -1):
        s = s + sr[i]
```



### 패턴 매칭 알고리즘

#### 고지식한 알고리즘 _ Brute Force

: 패턴 내의 문자들을 일일이 비교하는 방식

시간 복잡도 O(n*n)

##### my practice code

```python
p = 'youbin'
t = 'Hi youbin. How are you today?'
M = len(p)
N = len(t)

def BruteForce(p, t):
    i = 0
    j = 0
    while j < M and i < N:
        if t[i] != p[j]:
            i -= j
            j = -1
        i = i + 1
        j = j + 1
    if j == M :
        return i - M
    else:
        return -1

print(BruteForce(p,t)) # 3
```

##### find method 활용하기

```python
# 

p = 'abcdabcef'
t = 'alksdabcaabcdabceflasabcdabcabcdabcefaflkjdkdabcdabcefdksl'

n, m = len(t), len(p)

idx = t.find(p)
print(idx, t[idx:idx + m])

idx = t.find(p, idx + m)
print(idx, t[idx:idx + m])

idx = t.find(p, idx + m)
print(idx, t[idx:idx + m])

idx = t.find(p, idx + m)
print(idx, t[idx:idx + m])

# 출력
#9 abcdabcef
#28 abcdabcef
#45 abcdabcef
#-1 
```

##### 기준으로 나눠서 만들기

```python
# 같을 때를 기준으로 만들기

p = 'abcdabcef'
t = 'alksdabcaabcdabceflasabcdabcabcdabcefaflkjdkdabcdabcefdksl'

n, m = len(t), len(p)

i = j = 0
while i < n:
    if t[i] == p[j]:
        i, j = i+1, j+1
        if j == m:
            print(i - j, t[i - j:i - j + m])
            j = 0
    else:
        i = i - j + 1
        j = 0
        
# 출력
#9 abcdabcef
#28 abcdabcef
#45 abcdabcef
```

```python
# 다를 때를 기준으로 만들기
p = 'abcdabcef'
t = 'alksdabcaabcdabceflasabcdabcabcdabcefaflkjdkdabcdabcefdksl'

n, m = len(t), len(p)

i = j = 0
while i < n:
    if t[i] != p[j]:
        i = i - j + 1
        j = 0
    i, j = i+1, j+1
    if j == m:
        print(i - j, t[i - j:i - j + m])
        j = 0
        
# 출력
# 9 abcdabcef
# 28 abcdabcef
# 45 abcdabcef
```



#### KMP 알고리즘

:  불일치가 발생한 앞 부분 다시 비교하지 않음

시간 복잡도 O(M+N)



#### 보이어-무어 알고리즘

:  오른쪽에 문자가 불일치면 그 문자가 패턴 내에 존재하는지 확인한 후 없으면 이동 거리를 패턴의 길이만큼 된다.

아이디어 존재 알고 있기!!

시간 복잡도 최선 O(n) / 최악 O(mn)

```python
str1 ="12345"
val = 0

for i in range(len(str1)):
    if ord('0') <= ord(str1[i]) <= ord('9'):
        digit = ord(str1[i]) - ord('0')
    else:
        print("error")
        break
    val = val*10 + digit

print(type(val), val)

def myitoa(x):
    sr =''
    while True:
        r = x % 10
        sr = sr + chr(r + ord('0'))
        x //= 10
        if x == 0: break

    s = ''
    for i in range(len(sr) - 1, -1, -1):
        s = s + sr[i]

    return s

def bForce(t,p):
    i = 0
    j = 0
    M = len(p)
    cnt = 0
    while j < M and i < len(t):
        cnt += 1
        if t[i] != p[j]:
            i = i-j + 1
            j = 0
        else:
            i += 1
            j += 1

    print(cnt, " times computations", end = " ")
    if j == M: return i-M
    else: return -1


def computejump(pattern, pi):

    L = len(pattern)
    for i in range(L-1):
        pi[ord(pattern[i])] = L-i-1


def bmoore(tstr, pstr):
    n = len(tstr)
    m = len(pstr)
    jump = [m for _ in range(128)]
    computejump(pstr, jump)

    i = 1
    cnt =0
    while i < (n - m + 1):
        j = m - 1
        k = i+ m -1
        cnt += 1

        while j>0 and pstr[j] == tstr[k] :
            j -= 1
            k -= 1
            cnt += 1

        if j == 0:
            print("%d times computations" % cnt, end=" ")
            return i

        i += jump[ord(tstr[i+m-1])]

    print("%d times computations" % cnt, end=" ")
    return -1

text = "a pattern matching algorithms"
pattern = "rithm"

print("Brute force algorithm : ", end = ' ')
print("%d th position" % bForce(text, pattern))
print("Boyer Moore algorithm : ", end = ' ')
print("%d th position" % bmoore(text,pattern))

str = input()
s=''

N = int(len(str))
for i in range(N):
    s += str[N-i-1]

print(s)
print(myitoa(1234))
```


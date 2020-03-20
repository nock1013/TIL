# R

### 1. 단축키

ctrl + enter : 실행

ctrl + L : 지우기

ctrl + 1,2 : 

shift + "" : 전체 "  "

alt  + -() : <-



### 2. 함수

* class(test)  : data타입 체크?

* test <- as.numeric(result1) 
  * 타입변경 

* sqrt(x=9) 
  * #x는 함수의 매개변수로 인식

* sqrt(x <- 9)
  * #x라는 변수로 활용

### 3.vector

#### 정수형 데이터

* myvector1 <- c(100,200,300)
  * myvector1에 100,200,300을 넣는다.
  * c (값)   = 값을 묶어서 넣어줄 때 쓰이는 함수?

#### 문자형 데이터

* java,R,haoop,android의 값을 mytext에 삽입

```R
mytext <- c("java","R","hadoop","android")
```

* matrix에 사용하므로 벡터에서 쓸 수 없다.

```R
mytext[1,2] 
```

* 벡터의 1번요소와 2번요소만 출력

```R
mytext[c(1,2)]
```

* 벡터의 1번요소와 3번요소만 출력 

```R
mytext[c(1,3)]
```

* 1번~3번까지의 요소 선택

```R
mytext[c(1:3)] 
```

* 1번요소 제외한 나머지

```R
mytext[-1] 
```

* 1번과 3번요소를 제외한 나머지

```R
mytext[c(-1,-3)]
```

* 1번~3번요소를 제외한 나머지 = mytext[-c(1:3)] 같다

```R
mytext[c(-1:-3)] 
```

---



#### TRUE와 FALSE를 이용해서 작업하기

* TRUE로 표현된 요소만

```R
mytext[c(T,T,F,F)]
```

* 조건을 만족하는 요소만 추출

```R
mytext[mytext=="java"] 
```

* 1~100까지 요소 생성

```R
numlist <- 1:100 
```

* %%가 나머지 구하기

```R
numlist %%2==0  
```

* 짝수구하기

```R
numlist[numlist %%2==0]
```

* 홀수하기

```R
numlist[numlist %%2==1 ] 
```



* seq 연속된숫자 모두입력 (1~10) 
  * 결과 1,2,3,4,5,6,7...10

```R
numlist2 <- seq(1,10)
```

* 결과 1,3,5,7,9

  by = 2  #2씩 

```R
numlist2 <- seq(1,10,by = 2)
```





#### 조인 출력

* v1에 70,80,90,100을 생성

```R
v1 <- c(70,80,90,100)
```

* names는 벡터의 각 요소에 이름을 출력

```R
names(v1) <- c("국어","수학","영어","자바")
```

* 문자열길이

```R
length(v1) 
```

결과 

> v1 <- c(70,80,90,10)
> names(v1) #names는 벡터의 각 요소에 이름을 출력
> NULL
> names(v1)
> NULL
> names(v1) <- c("국어","수학","영어","자바")
> v1
> 국어 수학 영어 자바 
> 70   80   90   10 
> length(v1)
> [1] 4



### 4.matrix

-----

#### 미니실습2

```R
m1 <- matrix(c(80,90,70,100,80,99,78,72,90,78,82,78,99,89,78,90),ncol=4,byrow = T)
m1
colnames(m1) <- c("국어","영어","과학","수학") 
m1
rownames(m1) <- c("kim","lee","hong","jang")
m1

avg_name <- matrix(c(mean(m1[1,]),mean(m1[2,]),mean(m1[3,]),mean(m1[4,])),ncol = 4,byrow=T)
avg_name
colnames(avg_name) <- c("kim","lee","hong","jang") 
avg_name

avg_subject <-matrix(c(mean(m1[,1]),mean(m1[,2]),mean(m1[,3]),mean(m1[,4])),ncol = 4,byrow=T)
avg_subject
colnames(avg_subject) <- c("국어","영어","과학","수학")
avg_subject
```

#### 실행결과 

----



> m1 <- matrix(c(80,90,70,100,80,99,78,72,90,78,82,78,99,89,78,90),ncol=4,byrow = T)
> colnames(m1) <- c("국어","영어","과학","수학") 
> rownames(m1) <- c("kim","lee","hong","jang")
> m1
> 국어 영어 과학 수학
> kim    80   90   70  100
> lee    80   99   78   72
> hong   90   78   82   78
> jang   99   89   78   90
> avg_name <- matrix(c(mean(m1[1,]),mean(m1[2,]),mean(m1[3,]),mean(m1[4,])),ncol = 4,byrow=T)
> colnames(avg_name) <- c("kim","lee","hong","jang") 
> avg_name
> kim   lee hong jang
> [1,]  85 82.25   82   89
> avg_subject <-matrix(c(mean(m1[,1]),mean(m1[,2]),mean(m1[,3]),mean(m1[,4])),ncol = 4,byrow=T)
> colnames(avg_subject) <- c("국어","영어","과학","수학")
> avg_subject
>  국어 영어 과학 수학
> [1,] 87.25   89   77   85







### 4. dataframe

#### 혼자서해보기

```R
product <- data.frame("제품"=c("사과","딸기","수박"),"가격"=c(1800,1500,3000),"판매량"=c(24,38,13))
product
mean(product$가격) # 가격평균
mean(product$판매량) #판매량 평균
```

------

#### csv.exam 실습

```R
mydata <- read.csv("csv_exam.csv")
mydata
mydataResult <- mydata[mydata$science>=80,]
mydataResult
#as._ric? = 숫자형의 데이터가 아니면 숫자형으로 데이터타입을 바꾸는 작업
mydataResult$mytotal <- as.numeric(mydataResult$math+mydataResult$english+mydataResult$science)
mydataResult
mydataResult$myavg <- as.numeric(mydataResult$mytotal/3)
mydataResult
```

실행결과

> mydata <- read.csv("csv_exam.csv")
> mydata
> id class math english science
> 1   1     1   50      98      50
> 2   2     1   60      97      60
> 3   3     1   45      86      78
> 4   4     1   30      98      58
> 5   5     2   25      80      65
> 6   6     2   50      89      98
> 7   7     2   80      90      45
> 8   8     2   90      78      25
> 9   9     3   20      98      15
> 10 10     3   50      98      45
> 11 11     3   65      65      65
> 12 12     3   45      85      32
> 13 13     4   46      98      65
> 14 14     4   48      87      12
> 15 15     4   75      56      78
> 16 16     4   58      98      65
> 17 17     5   65      68      98
> 18 18     5   80      78      90
> 19 19     5   89      68      87
> 20 20     5   78      83      58
> mydataResult <- mydata[mydata$science>=80,]
> mydataResult
> id class math english science
> 6   6     2   50      89      98
> 17 17     5   65      68      98
> 18 18     5   80      78      90
> 19 19     5   89      68      87
> #as._ric? = 숫자형의 데이터가 아니면 숫자형으로 데이터타입을 바꾸는 작업
> mydataResult$mytotal <- as.numeric(mydataResult$math+mydataResult$english+mydataResult$science)
> mydataResult
> id class math english science mytotal
> 6   6     2   50      89      98     237
> 17 17     5   65      68      98     231
> 18 18     5   80      78      90     248
> 19 19     5   89      68      87     244
> mydataResult$myavg <- as.numeric(mydataResult$mytotal/3)
> mydataResult
> id class math english science mytotal    myavg
> 6   6     2   50      89      98     237 79.00000
> 17 17     5   65      68      98     231 77.00000
> 18 18     5   80      78      90     248 82.66667
> 19 19     5   89      68      87     244 81.33333



### 5. 파일 import & export

#### import방법

```R
mydata <- read.csv("csv_exam.csv")
```



#### export방법

```R
write.csv(mydataResult,file = "result2.csv")
```


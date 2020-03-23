# crawling(크롤링)

1. install & library

```
install.packages("mongolite")
library("stringr")
library("mongolite")
```

=============================================================

2. 정보 확인(정보확인용) :encoding 타입을 크롤링할 사이트와 동일하게 맞춰야함

   ```R
   url <- "https://www.clien.net/service/group/community"
   url_data <- readLines(url,encoding = "UTF-8")
   url_data
   class(url_data)
   
   length(url_data)
   
   head(url_data)
   
   tail(url_data)
   ```

   * url_data[200]
     조건에 만족하는 데이터를 필터링
     str_detect : 문자열에 패턴을 적용해서 일치여부를 T/F로 리턴



## - 데이터 필터링 : title 

1. str_detect(패턴을 검사할 문자열, 패턴)를 이용해서 웹페이지 전체에서 필요한 데이터만 먼저 추출
2. str_detect : 문자열에 패턴을 적용해서 일치여부를 T/F로 리턴
```R
   filter_data<- url_data[str_detect(url_data,"subject_fixed")]
```

2. 추출한 데이터 전체에서 내가 필요한 문자열만 추출
   * str_extract -> 패턴에 일치하는 문자열을 리턴
   * 후방,전방 탐색 정규 표현식 이용 
     * 후방탐색 기호 (?<=) , 
     * 전방탐색 기호(?=)

```R
title <- str_extract(filter_data,"(?<=\">).*(?=</span>)")
title
```

## - 데이터 필터링 : hit(조회수)

```R
hit_data <- url_data[str_detect(url_data,"<span class=\"hit\">")]
hit_data

hit <- str_extract(hit_data,"(?<=\">).*(?=</span>)")
hit
```

## - 데이터 필터링 : url

* which - True인 위치 값을 출력해주는 메소드

  ```
  myurl <- url_data[which(str_detect(url_data,"subject_fixed"))-3]
  myurl
  ```

* str_extract -> 패턴에 일치하는 문자열을 리턴

  ```R
  url_val<- str_extract(myurl,"(?<=href=\").*(?=data-role)")
  url_val
  ```

* 필요없는 문자열을 잘라내기 =end = 3 뒤에서 3개를 자르겠다.

  ```R
  url_val<- str_sub(url_val,end = -3)
  url_val
  ```

* paste - 벡터를 연결해서 하나의 문자열로 보여주기
  paste0 - 여러 개를 연결

* ```R
  url_val <- paste0("https://www.clien.net",url_val)
  url_val
  ```

## - csv파일로 생성 cbind = 붙이는 작업

```R
final_data <- cbind(title,hit,url_val)
final_data
write.csv(final_data,"crawl_data.csv")
```

---



## MongoDB 연동

#### -mongodb에 저장하기

1. 메소드 지우기

   ```R
   if(con$count()>0){
     con$drop()
   }
   final_data
   class(final_data)
   ```

2. mongodb에 데이터를 저장하기 위해서 dataframe으로 변환

   ```R
   final_data <- data.frame(final_data)
      con$insert(final_data)
   ```

   ---



## 전체 데이터 뽑기 예제

#### - 모두의 광장의 1페이지 : 10페이지의 모든 게시글 크롤링

1. 모든페이지의 title,hit,content 추출하기

2. crawl_result.csv, crawl_result.RData저장

3. mongodb저장 (300개 저장)

4. for, if문을 활용

```
library("stringr")
library("mongolite")

con <- mongo(collection = "crawl",
             db = "bigdata",
             url = "mongodb://127.0.0.1")
```

```R
#0번부터 9번 페이지까지 반복 작업
final_data_list=NULL
for (i in 0:9) {
  myurl <- paste0("https://www.clien.net/service/group/community?&od=T31&po=",i)
  url_data <- readLines(myurl,encoding = "UTF-8") 
#title
final_filter_data<- url_data[str_detect(url_read,"subject_fixed")]
title <- str_extract(final_filter_data,"(?<=\">).*(?=</span>)")
#hit추출
hit_data <- url_data[str_detect(url_data,"<span class=\"hit\">")]
hit <- str_extract(hit_data,"(?<=\">).*(?=</span>)")[-1]
#url추출
myurl <- url_data[which(str_detect(url_data,"subject_fixed"))-3]
url_val<- str_extract(myurl,"(?<=href=\").*(?=data-role)")
url_val<- str_sub(url_val,end = -3)
url_val <- paste0("https://www.clien.net",url_val)

  ##############url을 이용해서 content항목 추출##############
  contentlist =NULL #최초 변수 선언시 null로 초기화
  content_filter_data=NULL
  for(page in 1:length(url_val)){
    contentdata <- readLines(url_val[page],encoding = "UTF-8")
    start = which(str_detect(contentdata,"post_content"))
    end = which(str_detect(contentdata,"post_ccls"))
    content_filter_data <- contentdata[start:end]
	content_filter_data <- paste(content_filter_data,collapse = "")
	content_filter_data <- gsub("<.*?>","",content_filter_data)
	content_filter_data <- gsub("\t|&nbsp","",content_filter_data)
	#기존의 저장되어 있는 contentlist의 내용에 추가
	contentlist <- c(contentlist,content_filter_data)
	#cat("\n",page)



  }
  final_data <- cbind(title,hit,url_val,contentlist)
  final_data_list <- rbind(final_data_list,final_data)
  #cat("\n",i)
}
#####csv파일로 생성##########
final_data_list

  write.csv(final_data,"crawl_data.csv")
  save(final_data,file = "crawl_data.RData")# 저장 속도가 csv보다 빠름 (R에서 사용)
#####mongodb에 저장하기#######
##메소드 지우기
if(con$count()>0){
  con$drop()
}
  ###mongodb에 데이터를 저장하기 위해서 dataframe으로 변환
  final_data <- data.frame(final_data)
  con$insert(final_data)

  final_data
```




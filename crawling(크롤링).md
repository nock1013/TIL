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



## 데이터 필터링 : title 

1. str_detect(패턴을 검사할 문자열, 패턴)를 이용해서 웹페이지 전체에서 필요한 데이터만 먼저 추출
   filter_data<- url_data[str_detect(url_data,"subject_fixed")]

2. 추출한 데이터 전체에서 내가 필요한 문자열만 추출
   * str_extract -> 패턴에 일치하는 문자열을 리턴
   * 후방,전방 탐색 정규 표현식 이용 
     * 후방탐색 기호 (?<=) , 
     * 전방탐색 기호(?=)

```R
title <- str_extract(filter_data,"(?<=\">).*(?=</span>)")
title
```

## 데이터 필터링 : hit(조회수)

```R
hit_data <- url_data[str_detect(url_data,"<span class=\"hit\">")]
hit_data

hit <- str_extract(hit_data,"(?<=\">).*(?=</span>)")
hit
```

## 데이터 필터링 : url

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

## csv파일로 생성 cbind = 붙이는 작업

```R
final_data <- cbind(title,hit,url_val)
final_data
write.csv(final_data,"crawl_data.csv")
```


# stash

>작업 내역을 임시 저장

## 기본 명령어

```bash
# 1. stash 공간에 작업내역 저장
$ git stash
Saved working diretory and index state WIP on master

#2. stash list 보기
$ git stash list
stash@{0} : WIP on master : a8cd8e9 Init
#3. 임시 공간 내용 가져오기
$git stash pop
```

## 예시

* 로컬에서 작업하고 있던중, `pull` 을 받아서 원격 저장소에 새로운 내용을 반영 해야 하는 경우

```bash
$git pull origin master


#1. 커밋하거나 => 버전으로 남겨도 되는 경우
#2. stash => 작업 중일 때
```

* 해결방법

```bash
$ git stash
$ git pull origin master

#만약 동일 파일 수정이 있으면,conflict를 발생시킨다.
$git stash pop
```

---

# reset vs revert

## 1. reset

* 특정 버전으로 되돌아가는 작업

  ```bash
  $git reset {커밋해시코드}
  ```

  * `reset` 명령어의 결과는 다음과 같다.

  ```bash
  $ git log --oneline
  275a7e2 ddd
  
  $git reset 00d69a4
  $git log --oneline
  ```

  * `reset` 옵셧
    * 기본 : 이전 이력의 변경 사항을 WD에 보관
    * `--hard` : 이전 이력의 변경 사항은 모두 삭제, **주의**

## 2. revert

* 특정 시점을 되돌렸다는 커밋을 발생 시킴

  ```bash
  $ git log --oneline
  
  $ git revert 275a7e2
  $ git log --oneline
  ```

  


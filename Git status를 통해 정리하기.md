# Git status를 통해 정리하기

## 기초 명령어

```bash
#list (파일 목록)
$ ls
# chage directory(디렉토리 변경)
$ cd
#파일 생성
$touch <파일명>
```

## 상황

### 1. add

```bash
$ touch a.txt
$ git staus

On branch master

No commits yet
# 트래킹X. 해로 생성된 파일.
Untracked files:
	# 커밋을 하기 위한 곳에 포함시키려면
	# Staging area로 이동시키려면, git add
  (use "git add <file>..." to include in what will be committed)
        a.txt
# WD O, Staging area X,
nothing added to commit but untracked files present (use "git add" to track)

```

```bash
$ git add a.txt
$ git status
On branch master

No commits yet
# 커밋될 변경사항들(staging ara 0)
Changes to be committed:
	# unstage를 위해서 활용할 명령어(add 취소)
  (use "git rm --cached <file>..." to unstage)
        new file:   a.txt

```

- add 취소

```bash
git restore --staged b.txt
```

## 2. commit

``` bash
$ git commit -m 'Create a.txt'
[master (root-commit) 8dfeabb] Create a.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt
 
$ git status
On branch master
nothing to commit, working tree clean

```

- 커밋 내역 확인
- log log -

## 3. 추가 파일 변경 상태

``` bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   a.txt
        new file:   b.txt

```





## 4. 커밋 메세지 변경

> **주의!!** 커밋 메시지 변경시 해시값 자체가 변경되어, 이미 원격저장소에 push 한 이력에 대해서는 메시지 변경을 하면 안된다.

``` bash
$ git commit - amend
```

- vim 텍스트 편집기가 실행 된다.
  - `i` : 편집 모드
  - `esc`  편집 모드를 종료하고, 명령 모드에서
    - `:wq`
      - write + quit

```bash
[master 51194bb] a.txt 추가
 Date: Thu Apr 23 10:48:11 2020 +0900
 2 files changed, 12 insertions(+)
```

#### 4-1. 특정 파일을 빼놓고 커밋 했을 때

```bash
$ git add <omit_file>
$ git commit --amend
```

* 빠뜨린 파일을 add한 이후에 `commit  --amend` 를 

  하면, 해당 파일까지 포함하여 재커밋이 이뤄진다.

## 5. 작업 내용을 이전 버전으로 되돌리기

* a.k.a. 작업 하던 내용 버리기

``` bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  # WD 변경사항들을 버리기 위해서는 restore
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")


$ git restore a.txt
$ git status
On branch master
nothing to commit, working tree clean
```

#### 6. 특정 파일/폴더 삭제 커밋

> 해당 명령어는 실제 파일이 삭제되는 것은 아니지만, 
>
> git에서 삭제되었다라는 이력을 남기는 것.

---

```bash
$ git rm --cached b.txt

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    b.txt
        
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        b.txt
# 주의!! 해당 파일이 물리적으로 삭제 되는 것은 아니다.
```

* 일반적으로는 `gitignore` 와 함계 활용한다.

  1. `.`gitignore` 에 해당 파일 등록
  2. `git rm --cached` 를 통해 삭제 커밋

  * 이렇게 작업을 하면, 실제 파일은 삭제되지 않지만 이후로 git으로 전혀 관리되지 않는다. 

  

## 3. 원격 저장소 활용 명령어

1. 원격 저장소 목록 조회

``` bash
$ git remote -v
origin URL~~
origin URL~~
```

2. 원격 저장소 설정 삭제

```bash
$ git remote rm{원격저장소이름}
```

3. 원격 저장소 설정

```bash
$ git remote add origin {URL}
```

---

# Git 2

Android(집)

> 안드로이드 수어 내용 복습 

git status

git add + git commit '집에서 복습'

```nginx
student@M1501 MINGW64 ~/Desktop/branch
$ git init
Initialized empty Git repository in C:/Users/student/Desktop/branch/.git/

student@M1501 MINGW64 ~/Desktop/branch (master)
$ touch README.md

student@M1501 MINGW64 ~/Desktop/branch (master)
$ git add .

student@M1501 MINGW64 ~/Desktop/branch (master)
$ git commit -m 'Init'
[master (root-commit) 261635e] Init
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md

student@M1501 MINGW64 ~/Desktop/branch (master)
$ git status
On branch master
nothing to commit, working tree clean

student@M1501 MINGW64 ~/Desktop/branch (master)
$ git branch feature/menu

student@M1501 MINGW64 ~/Desktop/branch (master)
$ git branch
  feature/menu
* master

student@M1501 MINGW64 ~/Desktop/branch (master)
$ git checkout feature/menu
Switched to branch 'feature/menu'

-----------------------------------------------------------

student@M1501 MINGW64 ~/Desktop/branch (feature/menu)
$ touch menu.txt

student@M1501 MINGW64 ~/Desktop/branch (feature/menu)
$ git add .

student@M1501 MINGW64 ~/Desktop/branch (feature/menu)
$ git commit -m 'Complete menu'
[feature/menu 3bebc93] Complete menu
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 menu.txt


```



---

2개의 로그 발생 



$ git merge feature/menu = 마스터와 menu를 합치는 작업

$ git branch -d feature/menu

!!반드시 병합이 된 branch는 삭제한다.

```
student@M1501 MINGW64 ~/Desktop/branch (feature/menu)
$ git log --oneline
3bebc93 (HEAD -> feature/menu) Complete menu
261635e (master) Init
```

---

나혼자 잡업 하는 경우


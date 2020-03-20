# hadoop

### 1. 명령어

1. 호스트이름 변경
   * hostnamectl set-hostname hadoop01
     firewalld
     ssh +ip주소

* systemctl stop 서비스명 : 

* systemctl status 서비스명 현재상태

* systemctl disable firewalld : 방화벽 off

  아이피 주소를 호스트이름으로 변경

### 2. hadoop 셋팅 순서

1. vmware 설치

2. 머신생성 - centos7

3. 머신 복제
  -ip확인

4. 머신4대를 클러스터링
    -방화벽해제 (systemctl disalbe firewalld)
    -hostname변경 (hostname set-hostname hadoop01[호스트명])
    -DNS설정

       * hosts파일 등록 = (내폴더-etc)ip 호스트네임
       * 네트워크 프로세스를 restart
       * 설정확인 - 설정을 성공 완료했는지 확인
       * 네 대에 모두 적용되도록
          hadoop01머신에서 hadoop02, hadoop03, hadoop04에 직접 접속
    > [원격 서버로 copy 명령어]
    > scp copy할파일(위치까지 명시) copy받을서버의 위치
    > scp      /etc/hosts/	  [서버]root@hadoop02:/etc/hosts

    ​     |				|										|

    명령어    copy할파일	       target서버의 위치와 파일명

    ---

    [원격 서버에 실행명령]
    ssh hadoop02 "/etc/init.d/network restart"

    ​    |								|

    ssh 서버 		"실행할명령문"

 ip, 도메인	

### 3. 암호화된 통신을 위해서 공개키생성 후 배포

> ssh-keygen -t rsa
> ssh-copy-id -i id_rsa.pub hadoop@hadoop02 공개키 보내기



---

---

## 20200217 hadoop

./hadoop-1.2.1/bin/hadoop jar multi-hadoop-examples.jar hdfs.exam.HDFSExam01 output.txt hellohadoop 

============================================

## 20200219 hadoop

~ : home 디렉토리 
/  : root 디렉토리
.   : 현재디렉토리
..  : home디렉토리
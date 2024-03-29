## Linux의 계열

- Devian(데비안) : 우분투(Ubuntu)
- Suse(수세) : OpenSuse
- RedHat(레드햇) : 페도라, CentOS

## Linux가 서버로 애용되는 가장 큰 특징

- 오픈 소스라 무료로 사용할 수 있는 큰 장점 -> 폐쇄 되지 않음
- 물론 레드햇 계열의 유료 매니지먼트 서비스들이 많긴 하다

## Linux의 세 가지 구성 요소

- 커널 : 하드웨어를 조작하는 Low Level의 Interface
	컴퓨터의 모든 자원과 프로세스, 메모리, 파일 시스템 등을 관리한다.
-  셸 : 커널과 사용자 사이의 Interface를 제공 사용자가 이해할 수 있는 언어로 명령을  내리고 프로그래밍 가능 다양한 셸들이 존재 (Bash, Zsh etc...)
- 응용 프로그램: 각종 프로그래밍 개발 도구, 문서 편집 도구, 네트워크 도구 등 사용자가 리눅스에서 사용할 수 있는 모든 프로그램

## Linux의 사용자 관리

- 리눅스는 기본적으로 Root 라는 SuperUser가 존재 프로세스 중에 중요한 프로세스들은 Root 계정의 UID로 메모리에 띄워지게 된다. (다른 권한 없는 유저가 함부러 프로세스를 Kill 하지 못하게 최소한의 방어 설계)

- /etc/passwd 파일에 사용자 정보들이 정의되어 있음

	### File Permission
	```
	리눅스의 파일 권한은
	파일 종류, 소유자 권한, 그룹 권한, Other 권한으로 나누어져 있다.

	ex) drwxrwxrwx == 디렉토리 777 권한
	이진법으로 계산

	```
	

## Linux Link

	1. Hard Link
	2. Symbolic Link

	두 가지 다 원본 파일을 복제함에 있어서 유사함을 가지지만 
	
	HardLink 는 INode를 공유 Primitive Data를 완전 복제해서 원본 파일을 삭제해도
	HardLink한 파일에는 영향이 가지 않지만
	
	Symbolic Link는 INode를 포인팅 한 INode를 새로 생성하여 원본 INode를 가르키게 된다 때문에 원본을 삭제하면 Link된 파일이 손상된다


## Package Manager

리눅스는 의존성을 가진 패키지들을 관리하기 위한 다양한 패키지 매니저들을 사용하는데 Redhat 계열은 ***rpm -> yum -> dnf 순으로 업데이트 해왔다***

#Ubuntu - apt
#MacOS - Brew 등이 있다.

## Linux의 파일 Managing

#Tar - 파일을 묶어서 관리 할 수 있는 명령어이다.

#find - 파일을 찾을 수 있는 기본 명령어이다.

	vi Editor
```
vi Editor의 단축키 정리

i - input(커서 자리 입력 모드)
a - append(커서 뒤로 입력 모드)
o - open line(다음 행으로 커서 이동 후 입력 모드)
O - Open line(현재 행을 밀어내며 입력 모드)
I - input(커서 위치 라인 첫 컬럼에서 입력 모드)
A - Append(커서 위치 마지막 컬럼에서 입력 모드)

^ - 현재 행의 첫 컬럼으로 이동
$ - 현재 행의 마지막 컬럼으로 이동
w - 다음 단어로 이동
10G - 10행으로 이동
:10 - 10행으로 이동

cw - 단어 수정
cc - 현재 전체 라인 수정
r - 현재 글자 다른 글자로 대체
x - 한 글자 지움
dw - 한 단어 삭제
dd - 한 라인 삭제
#dd - 숫자만큼 라인 삭제
yy - 현재 라인을 버퍼로 복사 (잘라내기)
#yy - 숫자만큼의 라인을 버퍼로 복사 (잘라내기)
p - paste

/String - String을 검색
n - 다음 단어 검색
N - 이전 단어 검색

```

## Linux의 Systemd

---
#Systemd : Systemd는 기존의 init 스크립트를 대체한 것 다양한 서비스를 동작시킨다.
제일 처음 생성되는 프로세스로 PID 1을 부여받는다.
Systemd 프로세스는 다양한 Unit들을 구성요소로 가지며
이 구성요소들은 전체 시스템(File, Directory, Run Level)을 관리하게 된다. 

#RunLevel 이란?
***Run Level 이란 시스템의 상황에 따라 사용자가 취사 선택할 수 있는 실행 환경으로 1레벨인 Rescue target은 시스템에 치명적인 오류가 발생했을 때 디버깅을 위해 사용하고 다른 사용자의 접속을 막아 2차 피해를 막는다  2, 3, 4 레벨은 Non-graphical Level, 5레벨은 Graphical Level이다.***

Default 는 5레벨이다.


## Linux Process

	하드웨어에 정의되어 있는 프로그램이, 메모리에 로드되어 활성화 된 것
	포그라운드 프로세스 - 사용자와 상호작용하는 프로세스
	백그라운드 프로세스 - Systemd, Server Demon 등...뒷단에서 실행되고 있는 프로세스

***모든 프로세스는 부모 프로세스를 가지고 있다.
부모 프로세스를 종료 시키면, 자식 프로세스도 종료 된다.

자주 사용하는 명령어
ps -ef | grep [프로세스 이름] 
kill -9 [프로세스 번호]

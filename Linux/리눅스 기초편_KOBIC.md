# 리눅스 기초편

> KOBIC 차세대 생명정보 교육
>
> 총 1강 ~ 10강까지 정리한 내용입니다.

<br>

## 제 1강. 운영체제와 리눅스

### 운영체제 (Operating System)

Windows 10 / macOS / Ubuntu

: 모든 하드웨어와 모든 소프트웨어를 관리하는 관리자

하드웨어 <-> OS <-> Applications <-> User

운영체제는 하드웨어와 응용 소프트웨어를 연결하는 역할

<br>

### 운영체제의 역사

UNIX

\> Linux

\> Mac OS

\> Microsoft Windows

![유닉스 계열 - 위키백과, 우리 모두의 백과사전](https://upload.wikimedia.org/wikipedia/commons/7/77/Unix_history-simple.svg)

<br>

### 리눅스의 구조, 그리고 리눅스의 두가지 의미, 커널과 배포판

<img src="https://miro.medium.com/max/636/1*LEp6Tu9LKTF0m0DXvgNMvg.png" alt="Difference Between Kernel and Shell | by Jagadish Hiremath | Medium" style="zoom:67%;" />

리눅스느 크게 커널과 배포판으로 두 가지 의미를 가진다.

<br>

### 리눅스 커널 (Linux Kernel)

![Linux Kernel Map](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2235B34C57BEDFCD26)

자체만으로 Linux kernel 이라고 한다.

<br>

![리눅스 커널 - 위키백과, 우리 모두의 백과사전](https://upload.wikimedia.org/wikipedia/ko/timeline/2e8e57ac8575e8aa808e4754cabc1d46.png)

\> Linux 2.0: 멀티프로세싱(SMP) 지원, 커널모듈 동적로드 등

\> V 2.2: 파일 마운트(autofs), MCA버스 지원, PCI 변경

\> V 2.4: 엔터프라이즈급 서버 지원, 물리메모리 64G 이상, 프로세스 무한대 가능

<br>

### 리눅스 배포판 (Linux Distribution)

* 사용자가 쓰기 편리하도록 리눅스 커널에 각종 응용 프로그램(패키지)을 결합하여, 독립적으로 배포하도록 함.

  > Ubuntu / fedora / CentOS

* 리눅스는 공짜지만, 리눅스 배포판은 유료일 수 있음. (예, 레드햇 에넡프라이즈 리눅스)

* 1993년부터 배포판이 나오기 시작함.

* 국내에는 1996년 알짜 리눅스가 최초로 소개됨. (리눅스코리아 이만용 이사, 최초 한국어 지원)

* 배포자가 다양한 목적으로 다양한 배포판을 만들어 제공함. (필요시 직접 만들 수 도 있음)

* 각 배포판은 별도의 응용 프로그램(패키지) 업데이트 시스템이 구비되어 있음. (yum, apt-get)

* 좋은 배포판의 조건 - 안정성, 빠른 업데이트, 사용편의성

<br>

### 리눅스 배포판의 종류

<데비안 (Debian) 계열>

서버의 목적보다 개인 사용자가 PC에서 쓰기 적합

지속적인 기술 업데이트

> Ubuntu가 대표적
>
> - 사용자의 편의성에 초점
> - GUI 환경이 매우 잘되어 있어 초보에게 적합
> - 연 2회 정기 업데이트, 2년에 1회 장기 업데이트

<br>

<레드햇(Redhat) 계열>

서버용 OS가 목적으로 보안, 안정성이 뛰어남

업데이트보단 보안, 안정성이 우선

> Fedora
>
> * 레드햇에서 개인용과 엔터프라이즈용 구분
> * 오픈소스 기반으로 컴퓨터 최신기술의 선도가 목표
> * 배포 간격과 이전버전 지원 기간이 짧음
>
> CentOS
>
> * 레드헷 앤터프라이즈 리눅스(RHEL)를 그대로 계승하는 무료 기업용 OS
> * 기억에서 무료 서버용 리눅스를 활용하는 점이 장점

<br>

### GUI vs CUI

GUI: Graphic user interface

> 그래픽적 환경 

CUI: Character user interface

> 명령 쉘 프럼프트

=> 두가지 특징이 사용자 인터페이스의 큰 특징

<br>

### 윈도우즈 vs 리눅스

**Windows** 

* GUI 기반으로 많은 사람들이 범용적으로 쓸 수 있도록 만들어진 OS
* 폴더 생성, 파일 생성, 작성 등 어떤 작업을 하려고 할 때 하려는 기능들이 그래픽 요소로 보여 쉽게 사용
* 내부 기능들을 모두 공개하지 않으며, 안정성 등의 이유로 내부 시스템을 수정하는 등의 작업을 할 수 없음.
* 유료 라이선스

<br>

**Linux**

* CUI 기반으로 명령행을 이용하여 모든 작업을 수행할 수 있음
* 리눅스 종류에 따라 준수한 GUI를 지원하는 경우도 있음
* 오픈 소스기반으로 모든 사용자가 모든 기능들을 직접 수정할 수 있음 - 작업 환경 구성이 용이
* 오픈소스 (무료 라이선스 - GPL)
* 분석에 사용하려는 개발 환경을 구성하기 용이하여 직접 분석 프로그램을 작성할 수 있음.
* 과학분야(생물정보) 많은 도구들이 리눅스에서 만들어짐

<br>

### 왜 리눅스를 사용하는가?

* 오픈 소스 기반으로 대부분의 리눅스가 무료로 사용 가능함.
* 서버용 프로그램은 사용이 편리한 FUI이기보다, 장시간 안정적인 CUI 환경이 많음.
* 서버용 운영체제로 널리 사용됨. (최근 유닉스 -> 리눅스)
* 다수가 사용하고 기여함으로써 보다 더 보안에 강력하며 안정적이고 신기술 활용이 가능함.
* 소프트웨어 개발과 관련된 많은 오픈 소스 프로그램을 쉽게 사용할 수 있음.
* 과학분야 (생물정보 등) 사용되는 수많은 도구들이 리눅스 환경에서 개발됨.

**사용자 인터페이스는 PC 운영체제에서, 서버 컴퓨팅은 리눅스 운영체제에서**

<br>

## 2강. 리눅스 설치하기

### 가상화(Virtualization)란?

: 컴퓨터 자원의 추상화 - 가상의 물리적 리소스를 만들어 냄 - 소프트웨어로 만든 가상의 하드웨어로 그 위에 운영체제를 설치할 수 있음.

![img](http://www.virtual-space.co.kr/images/virtualization-01.png)

가상의 메모리, 디스크, CPU 하드웨어를 프로그램으로 설치하고, 그 위에 다수의 운영체제 설치

<br>

**가상화의 장점**

* 서버 1대를 분할하여, 여러대의 논리적 서버로 사용할 수 있음.
* 웹 호스팅 서버 등에서 활용함 - 고객에게 필요에 따라 별도의 운영체제 제공
* 효율적인 서버자원의 활용 - 서버 구입비용, 전기료, 관리 비용의 절감
* 새로운 장치에 대한 장치 드라이버 불필요
* 윈도우즈 안에 별도의 리눅스 설치 가능

<br>

**가상화 소프트웨어**

* Vmware, Parallels (상용)
* VirtualBox (무료)
* Docker (무료)

<br>

###  VirtualBox

:오라클에서 개발한 가상화 소프트웨어 - 2007년 1월에 오픈소스로도 공개됨.

<br>

### 디스크 파티션; 파티션 분할

: 물리적으로 하나의 하드디스크이지만, 필요에 의해 별도의 디스크로 나눠 쓸 수 있다.

![img](https://t1.daumcdn.net/cfile/tistory/227DDA3D54ED51FD0E)

예) 윈도우즈에서 하나의 하나디스크를 C: 드라이브와 D: 드라이브로 나눠서 쓰기

<br>

**파티션을 분할하는 이유**

* 디스크 공간의 효율적 사용 - 기본 저장 단위인 섹터(sector)의 크기가 클수록 낭비가 크다.
* 다양한 파일시스템을 적용할 수 있다.
* 물리적 손상시 피해를 최소화할 수 있다.

<br>

 <파일 시스템>

: 컴퓨터에서 파일이나 자료를 쉽게 발견 및 접근할 수 있도록 보관 또는 조직하는 체제 - 하드디스크에 정보를 기록하는 방법 및 알고리즘으로 다양한 파일시스템이 개발되어 있음.



<파일시스템 종류>

* FAT31, NTFS: 윈도우즈에서 주로 사용되는 파일시스템
* ISO 9660: CD-ROM 매체를 위한 파일시스템 (.iso 확장자 파일로 만들기도 한다.)
* EXT1 ~EXT4: 리눅스, 유닉스에서 주로 사용되는 파일시스템 - 최근 XFS로 대체 되고 있음.
* ZFS: 기존의 유닉스 파일시스템 대체 - 무한대의 용량을 제공함.
* APFS: 애플에서 2016년 발표 - 암호화, 플래시 기반 - macOS, iOS에서 사용됨.

<br>

### 윈도우즈와 리눅스의 디렉토리 구조

![Linux 완전 기초! 파일 시스템, 프로세스 및 서비스, 로그파일 관리 ...](https://seonkyukim.github.io/assets/images/2019-07-08-Linux%20%EC%99%84%EC%A0%84%20%EA%B8%B0%EC%B4%88!%20%ED%8C%8C%EC%9D%BC%20%EC%8B%9C%EC%8A%A4%ED%85%9C,%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%20%EB%B0%8F%20%EC%84%9C%EB%B9%84%EC%8A%A4,%20%EB%A1%9C%EA%B7%B8%ED%8C%8C%EC%9D%BC%20%EA%B4%80%EB%A6%AC%EA%B9%8C%EC%A7%80!/01.png)



윈도우즈는 디스크가 C:, D: 로 구성되지만, 리눅스는 "/" 하위에 "dev" 하위에 존재하며 (예:/dev/sda2)

별도의 디렉토리에 연결(마운트) 하여 사용한다.

> 마운트: 디스크 파티션을 특정 디렉토리에 할당하는 것. (예. /dev/sda2 를 "/"에 할당, /dev/sda3을 "/home"에 할당 등)

<br>

### 윈도우즈 파티션 예제

예) "제어판 -> 컴퓨터관리"에서 "저장소 -> 디스크관리"를 보면 디스크 파티션 현황을 알 수 있다.

두개의 물리적 디스크가 있고, 디스크는 C:, D:로 구분되어 있다.

* 디스크 이름: 디스크 0, 디스크 1,...
* 볼륨 이름: C:, D:, E:
* 파일시스템: NTFS

<br>

### 리눅스 파티션 예제

예) 리눅스 파티션 관리 프로그램 GParted에서 본 파티션 설정 현황 물리적 디스크 "/dev/sda"를 /dev/sda1, /dev/sda2로 크게 나누고, /dev/sda2를 다시 /dev/sda5, /dev/sda6로 나누었다.

* 디스크 이름: /dev/sda (SCSI 디스크 첫번째)
* 볼륨(파티션) 이름: /dev/sda2, /dev/sda5, ...
* 파일시스템: ext3, linux-swap

> 유닉스/리눅스 계열은 가상메모리 파티션을 별도로 설정해야 한다. 보통 메모리의 2배정도 크기를 할당함.

<br>

### 리눅스 설치 실습

* 가상화 프로그램 VirtualBox 설치
* CentOS 홈페이지에서 "DVD ISO" 파일 다운로드
* VirtualBox 프로그램에서 CentOS 프로그램 설치 진행

<br>

##  3강. 리눅스 둘러보기

### VirtualBox 설정 몇가지

### 리눅스 GUI 환경: X Window System

X Window System (X11)

* 1984년 MIT 개발
* 마우스와 키보드 등의 입력장치 상호작용 관리
* 가장 최근 버전은 2012년 x11R7.7
* X11을 이용한 다양한 데스크탑 환경 프로그램이 별도로 존재함 (Genome, KDE, Xface)

<br>

### 리눅스 디렉토리 구조

* 사용자 디렉토리 /home 하위에 존재함
* /bin 디렉토리는 실행프로그램 존재
* /boot 디렉토리는 부팅과 관련된 파일들 존재
* /lib 디렉토리는 C 라이브러리를 비롯한 각종 프로그램 라이브러리
* /mnt 디렉토리는 USB 등 마운트한 저장장치
* /usr 디렉토리는 패키지 관리 프로그램이 설치한 프로그램
* /usr/local 디렉토리는 사용자가 직접 설치한 프로그램

<br>

### CentOS - Gnome Desktop 실습

**리눅스 GUI 환경실습**

### 쉘(CUI)에서 리눅스 다루기 - 터미널 프로그램 이용

$ls              # 현재 디렉토리 안의 항목들 조회

​		-a		# 숨겨진 항목들 전부

​		-l		 # 목록형 형식으로 출력

$ man [command]   # command에 대한 manual 보기 (도움말 볼 수 있음)

$mkdir [directory_name]    # directory_name 이름으로 된 디렉토리 생성

$ rmdir [directory_name]    # directory_name 이름으로 된 디렉토리 삭제

$pwd     # 현재 경로 확인

$cd [Path]       # Path 로 이동

> .. 는 상위로
>
> . 는 현재 디렉토리 의미

$ su - 		# root 관리자로 변경

$ yum update 	# 패키지 업데이트

<br>

### 쉘에서 USB 연결하여 데이터 가져오기

**윈도우즈에서 주로 사용하는 NTFS의 경우, 별도의 프로그램을 설치해야 한다.**

$ yum install epel-release # 확장 기능 EPEL 설치

$ yum install ntfs-3g 		# NTFS 파일시스템 설치

<br>

**ntfs-3g 패키지가 설치되면, USB 연결시 바로 자동 마운트 된다.**

<br>

## 4강. 터미널에서 리눅스 사용하기

### 리눅스 원격 접속

### SSH (Secure Shell)

: **시큐어 셸**(Secure Shell, **SSH**)은 [네트워크](https://ko.wikipedia.org/wiki/네트워크) 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해 주는 [응용 프로그램](https://ko.wikipedia.org/wiki/응용_프로그램) 또는 그 [프로토콜](https://ko.wikipedia.org/wiki/통신_프로토콜)을 가리킨다.

![SSH (Secure Shell) Home Page](https://images.ctfassets.net/0lvk5dbamxpi/7AWtdnEb8L92WkLkP4Tri8/23c91236721ec34ba42635ea5be05f2c/SSH_simplified_protocol_diagram)

<Br>

### SSH Client와 putty

SSH를 이용하기 위해서는 SSH Client (SSH 클라이언트)가 설치되어 있어야 한다. 리눅스 운영체제에는 기본적으로 SSH Client가 설치되어 있지만 윈도우에는 설치되어 있지 않기 때문에 설치가 필요하다. putty는 윈도우에서 사용할 수 있는 대표적인 SSH Client이다.

**Putty 프로그램**

=> SSH, Telent, rlogin, raw TCP를 위한 클라이언트.

Putty의 tty는 유닉스 전통의 터미널의 이름을 가리키며 teletype를 짧게 줄인 것

<br>

### 리눅스 디렉토리 구조

![Linux Directory Structure - Hello Python](https://raw.githubusercontent.com/lee-seul/lee-seul.github.com/master/static/img/_posts/linux_directory_structure.png)

<br>

### 리눅스 기본 명령어

윈도우와 같은 그래픽기반의 인터페이스와 달리 리눅스에서는 터미널에서 명령어를 사용하여 시스템을 제어할 수 있다. 리눅스에서 사용하는 기본적인 명령어는 ls, cp, mv 등이 있다.

<br>

**ls**

ls 명령어는 list directory contents라는 의미의 명령어로 해당 디렉토리 내에 있는 디렉토리 및 파일을 화면에 출력한다.

$ ls [옵션] [파일/디렉토리]

소유권이나 만든 날짜, 용량 등을 확인하려면 ls -al과 같이 옵션을 사용한다.

> option
>
>  -a >> 디렉토리 안에 있는 모든 파일과 디렉토리를 보여준다. (all)
>
>  -l >> 파일에 대한 정보를 자세하게 보여준다. 사용자의 권한, 소유자의 그룹, 크기, 날짜 등을 자세하게 보여준다. (list)
>
>  -h >> -l에서 지정한 크기 등을 사람이 읽기 쉽게 변환하여 보여준다. (humanize)

<br>

**pwd**

pwd 명령어는 print working directory에서 유래된 명령어로 현재 작업 중인 디렉토리의 위치를 출력한다.

$ pwd

pwd에는 -L, -P 옵션이 있다.

> option 
>
>  -L 심볼릭 링크 경로 표시 (logical location)
>
>  -P 심볼릭 링크가 가리키는 원래경로표시 (physical location)

<br>

**cd**

cd 명령어는 change directory의 준말이며 현재 작업디렉토리의 위치를 바꾸는 명령어이다.

$ cd [디렉토리상대경로 or 절대경로]

<br>

**cp**

cp 명령어는 copy의 준말로 특정 파일이나 디렉토리를 다른 파일이나 디렉토리로 복사하기 위한 명령어이다. 

$ cp [옵션] [원본파일명] [목적파일명 : 디렉토리명]

> option
>
>  -r >> 하위 디렉토리를 포함한 모든 디렉토리를 복사함.
>
> -p >> 사용자의 소유권, 권한을 그대로 복사함.
>
> -f >> 덮어쓰기 제한 등이 걸려있을 때 강제로 복사함.
>
> -a >> 원본 파일의 속성, 링크 정보를 그대로 유지하면서 복사함

<br>

**mv**

mv 명령어는 move의 준말로 특정 파일이나 디렉토리의 위치를 옮기거나 이름을 변경하는데 사용하는 명령어이다. 

$ mv [옵션] [원본파일/디렉토리] [사본파일/디렉토리]

> option
>
>  -b 백업파일 생성
>
>  -f 강제 삭제
>
>  -l 대상파일이 있을 경우 덮어쓸 것인지 물어봄.
>
>  -u 대상파일이 있을 경우 덮어쓸 것인지 물어봄.
>
>  -S 자동으로 접미사를 붙임.

<br>

**mkdir**

mkdir 명령어는 make directory 준말로 새로운 디렉토리를 만들 때 사용하는 명령어이다.

$ mkdir [옵션] 디렉토리명

> option
>
>  -p >> 하위디렉토리 생성
>
>  -m >> 디렉토리 생성시 권한도 함께 설정 (기본값: 755)
>
>  -v >> 디렉토리 생성 후 메시지 출력

<br>

**rm**

rm 명령어는 remove의 준말이며 파일이나 디렉토리를 삭제할 때 사용한다. (rmdir 명령어는 디렉토리를 삭제할 때 사용한다.)

> option
>
>  -r 디렉터리를 삭제할 때 하위의 내용을 먼저 삭제함.
>
>  -l 삭제할때 매번 여부를 물어봄
>
>  -f 존재하지 않는 파일은 무시하고 어떠한 확인 메시지도 출력하지 않음.
>
>  -v 삭제되는 내용을 보여줌.

<br>

**cat / more / less / head / tail**

cat, more, less, head, tail 명령어는 파일 내용을 확인하는 명령어이다. 로그나 문서들의 내용을 확인할 때 사용한다.

<br>

### 절대 경로와 상대경로

절대경로는 파일이 가지고 있는 고유한 경로이고, 상대경로는 현재위치를 기준으로 한 파일의 위치를 말한다.

![AWS로 서버를 시작하기 위해 필요한 Linux 지식 2-리눅스 기본조작 ...](https://miro.medium.com/max/5896/1*d3baBDQIu3hLU2V5HKAVHA.png)

<br>

## 5강. 파일과 프로세스

### 파일 다루기와 표준 입출력

### 파일 다루기 1: head, tail

head 명령어는 파일의 앞 부분을 출력하는데 사용한다. 아무런 옵션을 주지 않으면 처음 10줄이 출력된다.

tail 명령어는 파일의 뒷부분을 출력하는데 사용한다. 아무런 옵션을 주지 않으면 마지막 10줄이 출력된다.

<br>

### 파일 다루기 2: diff

diff 명령어는 파일 두개를 비교하여 이 둘의 차이를 나타낸다. 

<br>

파일 다루기 3: more, less

more 명령어는 파일을 부분별로 출력할 때 사용하는 명령어이다. (다음 페이지로 이동할 때는 스페이스 혹은 "n")

less 명령어는 more 명령어처럼 파일을 보여주는 용도로 사용한다. more보다 빠른데 그 이유는 파일 전체를 한번에 읽지 않기 때문이다. (한줄 아래 "j", 한줄 위 "k", 한페이지 아래 "Ctrl-F", 한페이지 위 "Ctrl-B")

<br>

### Redirection

redirection은 출력된 값들을 파일에 담을 때 사용한다.

$ [command] > [filename]

command에 화면에 출력할 명령어를 작성하고 > 뒤에는 이를 담을 파일이름을 작성하면 된다.

아래의 예제는 II명령어로 출력되는 값들을 another.txt에 저장한다.

> ls -l > another.txt

<br>

### dmesg

dmesg | more

<br>

### 파일 소유권 파일 압축과 해제

### 파일 실행 권한과 소유권이란?

![파일 접근 권한 설정](https://t1.daumcdn.net/thumb/R720x0/?fname=http://t1.daumcdn.net/brunch/service/user/797z/image/lFEVSk_VTHHecqo-k8qElvZf6MQ.png)

<img src="https://lh3.googleusercontent.com/proxy/N8T2y_8nd2lpILKoxcrl7_eiyINTC5p8FLdaQcRp_3uukrd4KDe9iEY6qUqyxBM65OxjrpPT7NQ8l-DTWFkAnthDN8AtRi8hQqCSmGZ86W5o3O2VvpyhwvU-SIWZV2NCe2fh3cyaJfe6L_gdoftsk81TWw" alt="리눅스 사용자와 그룹의 권한에 대하여.. - 순간적인 백업 - 한스월드" style="zoom:67%;" />

![그림4.png](http://hansworld.co.kr/files/attach/images/264/261/001/de208c27caeda8c9fc1f84a80bb02a72.png)

![리눅스(Linux) #5 - 파일과 프로세스[LEE'Today]](https://media.vlpt.us/images/ieed0205/post/679ffd71-914d-43c4-b907-0b1dbca00206/2.PNG)

<br>

### 권한을 변경할때 사용하는 chmod

chmod 명령어는 파일이나 디렉토리의 권한을 바꿔주는 명령어이다.

$ chmod [options] [permissions] [filename]

ex) $ chmod u=rwx, g=rx, o=r myfile **or** chmod 764 myfile

=> 권한을 보려면 ls -l file.txt

> option
>
>  -c >> like-verbose, but gives verbose output only when a change is actually made
>
>  -f >> Quiet mode
>
>  -v >> verbose mode
>
>  --no-preserver-root >> do not treat root directory in any special way
>
>  --preserve-root >> do not operate recursively on '/'
>
>  -R >> change files and directories recursively
>
>  --reference=RFILE >> set permissions to match those of file RFILE, ignoring any specified MODE.

<br>

### tar를 이용한 파일 묶기 방법

tar 명령어는 디렉토리나 여러개의 파일을 하나의 파일로 묶는 명령어이다.

$ tar -cvf [filename] [path]

$ tar -xvf [tar filename] [path]

> $ tar -cvf test.tar /home/test
>
> $ tar -xvf test.tar /home/test



> option
>
>  -c >> tar파일 생성할 때 사용 (풀때는 -x 옵션)
>
>  -t >> tar 파일의 내용을 확인할 때 사용
>
>  -f >> tar파일을 사용할 때 사용 (기본 사용)
>
>  -p >> tar파일을 생성/풀때 원본 파일 속성 (permission) 유지
>
>  -v >> 묶거나 풀 때 과정보기

<br>

### gzip 를 이용한 파일 압축 방법

gzip 명령어는 파일을 압축하는 명령어이다. tar 명령과 함께 쓰이기도 한다.

$ gzip [filename]

$ gzip -d [compressed filename]

> $ gzip test.tar
>
> $ gzip -d test.tar.gz
>
> \# tar 명령에 'z' 옵션주기
>
> ​	$ tar -czvf test.tar.gz /home/test



> option
>
>  -c 표준 출력 이용 (파이프와 사용)
>
>  -d 압축 풀기
>
>  -f 강제로 쓰기
>
>  -q 경고 메시지 출력 없이 하기



> 예제
>
> $ tar -zcvf comp.tgz *.txt
>
> : 모든 txt 파일을 하나로 묶어서 zip 파일로 압축 (tar + gzip = tgz)
>
> $ tar -zxvf comp.tgz
>
> : 위에서 만들었던 압축 파일을 풀기



<br>

### 원격 파일 전송, 프로세스 관리하기

### 원격 파일 전송: ftp (sftp), wget

ftp 명령어는 URL을 이용하여 인터넷에서 받을 수 있는 파일을 내려받을 때 사용하는 명령어이다. sftp도 ftp와 사용법은 같다.

$ ftp [URL]

$ ftp [ip]

$ ftp [user id]@[URL]

<br>

weget 명령어는 URL을 이용하여 인터넷에서 받을 수 있는 파일을 내려받을 때 사용하는 명령어이다.

<br>

### 프로세스 관리하기: ps, kill, top

ps 명령어는 현재 실행되고 있는 프로세스들을 보여주는 명령어이다.

$ ps [options]

$ ps -aux | grep 'emacs'

> ps -aux 를 사용하면 어떤 사용자가 어떤 프로그램을 사용하는 지 나와있다.
>
> 뒤에 | grep python 하면 python에 관련된 파일이 출력됨

<br>

kill 명령어는 실행되는 프로그램을 끌 때 사용한다.

$ kill [options] [process id]

> $ kill 564 
>
> : process id는 ps -aux를 통해 출력된 숫자를 보고 파악할 수 있다.

<Br>

top 명령어는 실행되는 프로그램들과 이들의 리소스 사용량을 보여준다.

$ top

> 현재 구동되고 있는 프로그램

<br>

## 6강. 리눅스용 문서편집기와 환경변수

### 리눅스용 문서 편집기

### 텍스트 파일과 바이너리 파일

컴퓨터에 있는 모든 파일은 텍스트(text) 파일 혹인 바이너리(binary) 파일

텍스트(text) 파일: 글자정보를 저장 (ASCII, UniCode, UTF-8)

바이너리(binary) 파일: 영상 정보 등 다양한 정보를 각각의 방식으로 기계어 저장

<br>

### CUI 환경(리눅스)에서의 텍스트 파일

텍스트 파일은 운영체제 환경에 기본 구성 요소임

환경설정: 윈도우즈 제어판 (GUI) 대신 환경설정 파일 (CUI)

프로그램 소스코드

<br>

### 텍스트 파일 읽기만 한다면, more, less

<br>

### 편집해야 한다면 리눅스용 문서 편집기

리눅스에서 사용할 수 있는 텍스트 에디터튼 vi(vim), emacs, nano 등이 있다.

<br>

### 리눅스용 문서 편집기의 종류

리눅스에서 사용할 수 있는 텍스트 에디터는 vi(vim), emacs, nano 등이 있다.

**VI (Vim) 에디터**

: 가장 대표적인 텍스트 에디터로 리눅스에 기본 설치되어 있음. 처음 접하면 어려우나 익숙해지면 타 에디터보다 빠르고 효율적인 작업이 가능함.

**Nano 에디터**

: 쉬운 사용법이 장점인 에디터로 대부분의 리눅스에 기본 설치되어 있음. 단축키를 몰라도 사용할 수 있음.

<br>

### Vi (Vim) 에디터

![리눅스(Linux) #6 - 문서편집기, 환경변수[LEE'Today]](https://media.vlpt.us/images/ieed0205/post/cabaabe0-4fbf-41ad-9592-f18ee9565f9d/1.PNG)

![img](https://t1.daumcdn.net/cfile/tistory/202B7E124C1E5A7A09)

<br>

### Inset mode

vi를 실행한 후 i, I, a, A 키를 눌러 Command mode에서 Insert mode로 진입할 수 있다. insert mode로 들어간 후에 원하는 텍스트를 추가하고 편집할 수 있다. 편집을 마친 후 ESC 키를 눌러 다시 Command mode로 돌아갈 수 있다.

<br>

### Command mode

vi가 시작할 때 실행되는 기본모드로 insert나 execution mode로 이동할 수 있다. Command mode에서는 단축키를 통해 커서를 이동하거나 복사, 붙여넣기를 할 수 있다.

<br>

### Execution mode

Command mode에서 ':' 키를 눌러 진입할 수 있다. ': 명령어'를 통해 파일을 저장하거나, 나가기 등을 할 수 있다.

> key
>
>  :. >> 현재 줄 처음으로 이동
>
> :$ 마지막 줄로 이동
>
> :1,3d >> 1~3번째 라인 삭제
>
> 
>
> :(range) s/pattern/string/flags
>
> \>>특정 range에서 pattern을 찾아서 string으로 ㅊ환
>
>  \* flags
>
>  -g: pattern이 여러개 나오면 모두 치환
>
>  -i: 대소문자 구분 안함
>
>  -c: 모든 pattern에 대해 바꿀지 물어봄
>
> 
>
> ex) 1,5s/aa/bb/gi >> 1~5줄에서 대소문자 구분하지 않고 모든 aa를 bb로 치환
>
> .%s/aa/nn/g >> 문서 전체에서 aa를 bb로 치환

<br>

### nano 에디터

nano는 사용법이 쉬워서 처음 사용하는 사람들도 어렵지 않게 사용할 수 있는 텍스트 에디터이다. 터미널에서 'nano [문서명]'을 입력하여 사용할 수 있다.

<br>

### 시스템 환경변수

### 환경변수

환경변수란 프로세스가 컴퓨터에서 동작하는 방식에 영향을 미치는 동적인 값들의 모임으로 쉘에서 정의되고 실행하는 동안 프로그램에 필요한 변수를 말한다.

아래는 환경변수 중 하나인 PATH를 출력한 것이다. PATH에 'ls'와 같은 실행파일들이 있는 경로(/bin)를 저장하고 있기 때문에 /bin/ls 형태로 사용하지 않고 ls로만 사용 가능 하다.

\# 환경변수 'PATH' 출력

$ echo $PATH

> echo는 무언가를 출력하라는 의미
>
> PATH에 들어 있는 경로들 안 파일에 대해서는 앞의 경로를 적지 않아도 실행 되도록 한다는 의미이다. 

\# 시스템의 모든 환경변수 출력

$ env

<br>

### 주요환경변수 목록

HOME : 사용자의 홈 디렉토리

PATH: 실행파일을 찾는 경로

LANG: 프로그램 사용시 기본 지원되는 언어

PWD: 사용자의 현재 작업하는 디렉토리

TERM: 로긴 터미널 타입

SHELL: 로그인해서 사용하는 쉘

USER: 사용자의 이름

EDITOR: 기본 편집기의 이름

HOSTNAME: 호스트의 이름

USERNAME: 사용자이름

<br>

### 환경변수 설정(export)

환경변수는 'export' 명령을 통해 추가할 수 있다.

\# 환경변수 num 추가

$ export num =1000

\# 환경변수 num 변경

$ export num = 2000

\# 환경변수 해제

$unset num

\# 기존 path에 새 경로 추가

export PATH=$PATH: /new/path/

$ echo $PATH

<Br>

### 환경변수 영구적으로 설정

export 명령을 통해 설정한 환경변수는 로그아웃하면 설정이 사라지므로 영구적으로 설정하기 위해서는 ~/.bashrc에 설정할 수 있다.

\# ~/.bashrc 열기

$ vi ~/. bashrc

export PATH=$PATH: /new/path/  # export 추가

\# 수정한 .bashrc 파일 적용

$ source ~/.bashrc

\# 수정한 환경변수 확인

$echo $PATH

<br>

### 환경변수 영구적으로 설정

~/.bashrc 이외에도 용도에 따라 아래 파일들을 편집하여 환경변수를 영구적으로 설정할 수 있다.

\# 모든 사용자용 (글로벌)

/etc/profile

\# 개별 사용자용 (로컬)

~/ .bash_profile

~/ .bash_login

~/ .profile

<br>

## 7강. 리눅스 각종 서버 프로그램 이해

### 리눅스 패키지 관리 시스템의 이해

### 패키지 관리 시스템을 사용하는 이유

* 리눅스의 두가지 의미. 커널과 배포판.
* 수많은 공개 소프트웨어의 조합으로 구성됨 -> 리눅스 디렉토리 구조에 다양한 프로그램을 적절히 배치
* 새 버전의 프로그램 출시 -> 편리하게 반영하고 싶음.
* 프로그램 버전간 "의존성" 존재
* 알려진 프로그램간 의존성을 반영하여, 쉽게 프로그램을 설치하고, 삭제할 수 있는 시스템 필요

==> 패키지 관리 시스템: 프로그램 설치, 삭제, 업데이트 수행

<br>

### 리눅스에서 프로그램을 설치하는 다양한 방법

1. 소스코드 -> 실행파일로 컴파일 (리눅스에는 컴파일러 프로그램이 내장되어 있음 (GCC))
   * 소스코드 다운로드 후 README 파일을 읽는다.
   * ./configure -> make -> make install
2. 실행파일 복사 (CPU 종류에 따라 다름)
3. 패키지 관리 시스템 이용
   * 잘 알려진 대부분의 공개 소프트웨어 이용 가능
   * 명령어 한줄로 프로그램 다운로드 및 설치
   * 자동으로 최신버전 업데이트

<br>

### 패키지 관리 시스템

**레드햇 계열**

* RPM(Red Hat Package Manager)
  * 원래 레드햇에서 사용되었던 패키지 파일이지만 현재는 많은 리눅스 배포판에서 사용됨.
  * .rpm 파일의 설치, 삭제, 정보 제공을 위해 사용되는 프로그램
* yum
  * RPM 설치를 개선하기 위해 개발한 패키지 관리자
  * 인터넷상의 저장소에서 패키지를 검색해 설치
  * 패캐지들의 의존성을 고려하여 설치

**데비안 계열**

* dpkg
  * .deb 파일의 설치, 삭제, 정보 제공을 위해 사용되는 프로그램
* apt
  * 인터넷상의 저장소에서 패키지를 검색해 설치
  * 패키지들의 의존성을 고려하여 설치

<br>

### rpm 사용법

![Computer Communications LAB., Kawangwoon University - ppt download](https://slidesplayer.org/slide/14627935/90/images/66/RPM+%EB%AA%85%EB%A0%B9+RPM+%EB%AA%85%EB%A0%B9+%EC%82%AC%EC%9A%A9%EB%B2%95+RPM+%EB%AA%85%EB%A0%B9+%EC%82%AC%EC%9A%A9%EB%B2%95+%EC%84%A4%EC%B9%98+%EB%B0%A9%EB%B2%95+rpm+%E2%80%93i+%ED%8C%A8%ED%82%A4%EC%A7%80%EB%AA%85+%EC%97%85%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%93%9C+%EB%B0%A9%EB%B2%95+rpm+%E2%80%93U+%ED%8C%A8%ED%82%A4%EC%A7%80%EB%AA%85.jpg)

![PPT - 서버 관리 및 실습 PowerPoint Presentation, free download ...](https://image2.slideserve.com/3724888/slide16-l.jpg)

<br>

### YUM 사용법 (레드햇 계열)

![PPT - 서버 관리 및 실습 PowerPoint Presentation, free download ...](https://image2.slideserve.com/3724888/slide20-l.jpg)

<br>

### APT 사용법 (데비안 계열)

![apt-get : 데비안 기반 리눅스 패키지 관리 명령어](https://3.bp.blogspot.com/-Od3V6hvmJ7M/WFpbtYKCAKI/AAAAAAAAmWk/nWgPGzQC3o0o7shSi22UeNYqHh8XAOIqwCLcB/w1200-h630-p-k-no-nu/apt-get.jpg)

<br>

### MySQL 설치 및 사용

### MySQL

* MySQL(마이에스큐엘)은 세계에서 가장 많이 쓰이는 오픈소스 관계형 데이터베이스 관리 시스템 (RDBMS, Relational Database Management System) 임.
* 자유 라이센스 GPL과 상용 라이센스, 두개의 적용을 받음.
* 썬 마이크로시스템즈에서 인수하였고, 이어 오라클에서 인수함.
* 오라클의 불확실한 라이센스 상태에 반발하여, MariaDB가 별도로 만들어짐 (같은 구성으로 별도로 만듦).
* CentOS 7에서는 MariaDB를 MySQL 대신 사용함.

<br>

### 패키지 관리 시스템을 통한 MySQL (MariaDB) 설치

### 데이터베이스 생성 및 사용자 추가

"test_db"를 만들고, 이 데이터베이스를 사용할 수 있는 사용자 'db_user' 를 만듦.

\# mysql -u root

### MySQL 사용자 비밀번호 변경

비밀번호를 변경하는 밥법은 여러 가지가 있다. 참고로 쿼리는 대소문자 상관없이 작동한다. 아래 쿼리는 강조하기 위해 다문자로 적었다. 소문자로 사용해도 된다.

<br>

### 테이블 생성

데이터베이스는 기본적으로 "테이블" 이라는 구성안에 데이터를 입력한다. 

"testtable" 라는 이름의 테이블을 만들고, 테이블 화확인하기

<br>

### 데이터 검색하기 (SEARCH)

만들어진 테이블에 저장된 데이터를 검색할 때는 "SEARCH" 구문을 이용한다.

<br>

### 그 밖에 yum으로 설치할 수 있는 패키지들

### yum패키지 관리 시스템 사용

잘 알려진 오픈소스 프로그램은 대부분 사용 가능하다. "yum search" 명령으로 해당 패키지 이름 확인한다.

<br>

## 8강. 리눅스 별도 프로그램 설치

### 리눅스 디렉토리 구조의 이해

<br>

### 리눅스용 프로그램 설치

### 일반적인 프로그램 설치 방법들

일반적으로 프로그램 배포 시 아래와 같은 방법들이 사용된다.

1. 즉시 실행 가능한 바이너리 형태

   -> 다운로드 하여 압축해제 후 바로 실행, 설치 불 필요

2. 각 리눅스 배포판에 맞춘 패키지 프로그램 (.rpm, .deb 등)

3. 직접 컴파일하여 설치 할 수 있는 프로그램 소스

   -> 설치 위치 및 옵션을 지정하여 사용자가 직접 컴파일 하여 설치

<br>

### 프로그램 설치 시 특정 디렉토리에 파일 복사됨

* 소스코드 -> 실행파일로 컴파일 

  > ./configure -> make -> make install

* 패키지 관리 시스템 이용

  > 자동으로 실행 된다.
  >
  > 실행파일은 /bin
  >
  > 헤더파일은 /include
  >
  > 라이브러리파일 /lib
  >
  > Root 실행 파일 /sbin

<br>

### 생물정보 프로그램 설치

### 설치 프로그램 소개 -BLAST

1. 용도

   Basic Local ALignment Search Tool의 약자로 생물정보학(bioinformatics)의 연구에서 가장 많이 사용되고 있는 분석 프로그램이다. 관심 있거나 내가 알고자 하는 핵산(DNA or RNA) 서열 또는 단백질 서열인 Query Sequence 대한 정보를 알고 싶을 때 서열 데이터베이스 (sequence database)에 비교하여 유사한 것들을 찾아주는 알고리즘이 이용된다. BLAST를 이용하여 알고자 하는 염기서열과 유사성이 높은 유전자 또는 유전체 구간을 검색할 수 있다.

2. 설치 방법

   * NCBI에서 바이너리와 소스 두 가지로 제공
   * 일반적으로 배포판에 맞춰 바이너리 파일을 받아 바로 실행 가능
   * 리눅스용 받아 압축 해제하여 설치

<br>

### FTP 이용하기 (GUI)

위치 -> 홈 -> (+) 다른 위치 -> 서버에 연결

<br>

### FTP 이용하기 (CUI)

yum으로 ncftp 프로그램 설치하고, NCBI FTP 사이트 접속

> \# yum search ncftp
>
> \# yum install ncftp
>
> \# ncftp ftp.ncbi.nih.gov

<br>

### 프로그램 설치

BLAST 프로그램은 별도의 설치 프로그램을 제공하지 않음.

다운받고 ./blastn 하면 실행가능 (존재하는 디렉토리로 cd로 들어간 후)

<br>

### 프로그램 설치 2

bin 디렉토리를 PATH 환경변수에 추가하면 어느 디렉토리에서도 실행할 수 있음.

\# cd ..

\# export PATH=$PATH: /(dir)/ncbi-blast-2.7.~linux/bin

\#  cd $HOME

\# blastn

<br>

### 설치 프로그램 소개 - ClustalW

1. 용도

   ClustalW는 Alignment를 위한 알고리즘 중 하나로 DNA 서열 혹은 protein서열의 다중정렬을 위해 사용한다. 다중정렬을 통하여 변이 없이 유지되고 있는 부분 (conserved)과 그렇지 않은 부분을 확인할 수 있다. 또한 진화적 연관관계를 확인하고, 진화 계통도를 그리는데 활용될 수 있다.

2. 설치 방법

   * 원하는 버전의 소스를 다운받아 컴파일하여 설치

<br>

### 프로그램 설치 1

Clustalw2 프로그램도 별도의 설치 프로그램을 제공하지 않음. ncftp 프로그램으로 FTP 서버에 접속하여, 소스 코드 다운로드

<br>

### 프로그램 설치2

Clustalw2 프로그램은 c ++ 소스코드로 되어 있으며, 이를 설치하기 위해서는 컴파일러 프로그램 (GCC, GCC-C++)이 필요함

> 컴파일러 프로그램은 yum install을 통해 다운 받을 수 있다.

<br>

### 설치 프로그램 소개 - PHYLIP

1. 용도

   PHYLIP은 the PHYLogeny Inference Package의 약자로 DNA나 Protein 서열을 이용한 계통분류 분석 프로그램을 모아놓은 패키지이다. Parsimony의 계산, 서열간 distance matrix 작성, tree construction과 같은 계통분류분석 과정에서 사용할 수 있는 30여가지의 프로그램을 모아 놓았다. 각 프로그램은 command line(shell, terminal, cmd)에서 실행된다.

2. 설치 방법

   * 원하는 버전의 소스를 다운받아 컴파일하여 설치

<br>

### 프로그램 설치 1

직접 다운로드 받을 수 있는 URL을 알고 있으면, wget 프로그램을 이용하여 다운받을 수 있음.

<br>

### 프로그램 설치 2

홈페이지 설명대로 Makefile.unx를 Makefile로 변경하고, make install

<br>

## 9강. 다중 터미널 환경 사용하기

### Background processing 이란?

foreground vs background processing

* Foreground processing

  터미널에서 작업할 때 일반적으로 사용자가 명령행을 실행하면 셀은 사용자가 입력한 명령을 실행, 결과를 출력한다. 이때 사용자가 입력한 명령이 실행되어 결과가 출력될 때까지 기다려야하는 방식을 일컬어 Foreground processing이라고 한다.

* Background processing

  Foreground processing은 실행하는 동안 대기를 해야하므로 다른 작업을 할 수 없다. 반면에 작업 제어가 제공하는 백그라운드 기능을 사용하면 동시에 여러 프로세스를 실행할 수 있다.

<br>

### Running process as background

### 장시간 동작하는 프로세스 예제

아래의 예제는 모든 조합의 로또 번호(6/45)를 lotto.txt 파일에 저장하는 파이썬 코드 (longtime_work.py)를 실행한 결과이다.

<br>

### 백그라운드 실행 방법1: &

& 명령어를 실행하려는 명령어 마지막에 추가하면 해당 명령어를 백그라운드로 실행한다.

$ [command] &

실행할 command에 &를 추가하는 간단한 방식으로 실행할 수 있다.

실행 뒤에 나오는 수는 PID (process identification number)라는 프로세스 고유의 ID number 이다.

<br>

### 작업제어1: 프로세스 목록보기

jobs명령어를 실행하면 현재 백그라운드로 실행되고 있는 프로세스의 목록을 볼 수 있다.

$ jobs

이를 실행하면 다음과 같은 프로세스 실행 리스트를 볼 수 있다.

리스트는 번호, 상태, 명령어 순으로 나타난다.

<br>

### 작업제어2: 중지하거나, 재시작하기

포그라운드로 실행되는 프로세스를 중지(stop) 하려면, Ctrl-z키를 입력한다.

$ fg >> 중단됐던 프로세스를 포그라운드로 재실행

$ bg >> 혹은 중단됐던 프로세스를 백그라운드로 재실행

> Ctrl + z >> 포그라운드 프로세스 멈추기 (멈춰있음)
>
> Ctrl + c >> 포그라운드 프로세스 강제 끝내기 (종료)

<br>

### 작업제어3: 작업 종료하기

백그라운드를 종료하는 방식은 foreground처럼 ctrl+c로 끌 수 없다.

kill명령어를 실행하면 현재 백그라운드로 실행되고 있는 프로세스를 끌 수 있다.

$ kill [PID]

PID는 ps 명령어를 통해 확인할 수 있다.

> ps -ef | grep python

<br>

### 백그라운드 사용방법 2: nohup

nohup명령어는 no hang up의 준말로 putty연결이 끊어지더라도 프로세스를 끝내지 않을 때 쓰는 명령어이다. nohup은 &와 함께 쓴다.

$ nohup [command] &

<br>

> $ nobup python longtime_work.py > lotto.txt &

<br>

### Screen 명령어 활용하기

### GNU Screen

**GNU Screen**는 여러 [가상 콘솔](https://ko.wikipedia.org/wiki/가상_콘솔)을 [다중화](https://ko.wikipedia.org/wiki/다중화_(통신))하는데 쓰이는 [응용 소프트웨어](https://ko.wikipedia.org/wiki/응용_소프트웨어)이자 [터미널 다중화기](https://ko.wikipedia.org/w/index.php?title=터미널_다중화기&action=edit&redlink=1)이다. 사용자가 하나의 [터미널](https://ko.wikipedia.org/wiki/컴퓨터_터미널) 창 안에서 별도의 [로그인 세션](https://ko.wikipedia.org/w/index.php?title=로그인_세션&action=edit&redlink=1) 여러 개에 접근할 수 있고, 또 터미널로부터 세션을 제거하거나 재부착할 수 있다. [명령 줄 인터페이스](https://ko.wikipedia.org/wiki/명령_줄_인터페이스)로부터 여러 개의 프로그램들을 다룰 때, 프로그램을 실행한 [유닉스 셸](https://ko.wikipedia.org/wiki/유닉스_셸)의 세션으로부터 프로그램을 분리시킬 때, 특히 사용자가 접속이 끊어져 있는 상태에서도 원격 [프로세스](https://ko.wikipedia.org/wiki/프로세스)의 실행을 지속시키고자 할 때 유용하다.

[GNU GPL](https://ko.wikipedia.org/wiki/GNU_GPL) 버전 3 이상의 조항에 의거하여 출시된 GNU Screen은 [자유 소프트웨어](https://ko.wikipedia.org/wiki/자유_소프트웨어)이다.

![GNU Screen - 위키백과, 우리 모두의 백과사전](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b5/Gnuscreen.png/1200px-Gnuscreen.png)

<Br>

### 백그라운드 실행방법3: screen

screen 명령어는 하나의 터미널에서 여러개의 세션을 쓸 수 있도록 한다. 각 세션은 백그라운드로 유지시킬 수 있으며, 접속이 끊어지더라도 유지된다. 원격에서 장시간 프로세스를 구동할 때 유용하다.

$ screen [options] [command]

<br>

**기본적인 command 옵션들**

![img](https://t1.daumcdn.net/cfile/tistory/1211B23A501785E93F)

**Ctrl+a를 이용한 옵션들**

![img](https://t1.daumcdn.net/cfile/tistory/182B403A501785EA1D)

<br>
![Linux & UnixScreen 사용하기](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F215ECC4C5833CE342F61E5)

<Br>

### screen 예제

yum으로 screen 설치하고, 세션 만들고, 윈도우 만들고, 실행 후 detach 시킴 (백그라운드 실행)

$ yum install screen

$ screen - S test		 \# test 이름이 세션 생성

(Ctrl-a c) 		\# 새 윈도우 생성

$ls

(Ctrl-a space) 			\# 윈도우 이동

$python longtime_work.py > lotto.txt

(Ctrl-a d) 		\# 세션 detach -> 백그라운드로 실행 중 상태로 screen 종료

$ screen -r test 		\# test 이름의 세션 재진입

<br>

### screen 활용

**screen을 써서 할 수 있는 것들**

* 하나의 터미널에서 여러개의 쉘(세션과 윈도우)를 띄워서 돌아가며 작업할 수 있다.
* 하던 작업(세션)을 그대로 두고, 다른 곳에서 접속해서 사용할 수 있다.
* 다른 사람이 다른 터미널에서 세션을 공유해서 쓸 수 있다.

**screen 팁**

* 화면 분할 기능: Ctrl-a Ctrl-S 하면 가로로 화면이 분할된다. 쉘을 가로로 나눠 쓸 수 있다.
* 복사 붙여넣기 기능: Ctrl-a Ctrl-[로 vi 모드 진입하여 복사 후, Ctrl-a Ctrl-]로 붙여넣기 할 수 있다.

<br>

## 10강. 로그 관리와 반복작업 자동화

### 리눅스 로그 시스템

### 로그(log)란?

컴퓨터 운영체제 및 각종 프로그램이 수행하는 내역에 대한 이력을 기록해 놓은 것을 로그라고 함.

> tail -f /var/log/messages

**로그의 활용**

* 컴퓨터 시스템의 모든 사용 내역 확인
* 프로그램 오류에 대한 정보 호가인
* 해킹사고 발생시 사고의 원인파악, 해커 추적

<br>

### 주요 리눅스 로그들


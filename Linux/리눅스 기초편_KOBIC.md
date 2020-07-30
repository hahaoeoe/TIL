# 리눅스 기초편

> KOBIC 차세대 생명정보 교육

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
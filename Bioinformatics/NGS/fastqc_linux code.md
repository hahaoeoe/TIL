# Ubuntu fastqc 파일 vi 편집기 활용

마운트

sudo mount 192.168.24.10:/fstorage_data/ /fstorage_data

ls

cd /fstorage_data

> 마운트: 연결





cd cylee

ls

cp -r JK_2020_05 ../dhgu/JK_2020_05



ll ../ 

> 폴더 권한을 볼 수 있음



.. 

> 상위 디렉토리로 올라간다.



ls ../

> 상위 디렉토리의 list가 보여준다.



ls ../../../

> 한번 쓸 때 마다 상위 디렉토리 한번 씩 올라간다.



du -sh

> 디렉토리 용량 확인



pwd

> 현재 디렉토리의 경로
>
> pass working 



cd  /

> 최상위로 올라감





sh

> shell script



Vi 편집기(ubuntu)

> google / naver에서 검색해보기



vi fastqc.sh

>output~~ => 변수 선언

>for 문
>
>cat 파일을 catch g한다.



sratoolkit

> export 해줘야 함



fastqc

> usr/bin에 대부분 들어있음
>
> input_~ 이 실행 코드



ls control tap 

> 모든 명령어에 tab 자동완성



gz

> 압축 파일(alzip 처럼)



sh

> 코드 실행시키는 shell script



i

> 눌러야 입력, 변경 가능



:q

> 종료

:wq

> 저장하고 종료

:q!

> 강제 종료

:wq!

> 강제로 저장하고 종료



vi trim.sh

> trim 하는 방법



dd

> 한 줄 지우는 법



delete 

> 한 단어 지우는 법



ls -alF



sudo chmod 775 *

> rwx 다 되는 게 7
>
> rwx 7
>
> r-x 5



sudo chmod 773 *

> wx : writing 과 실행





bam

sam

> 검색해보기





sudo rm -rf control*

> rm이 지우는 것
>
> rf : 모든 파일과 하위 디렉토리 모두 



-rw-r--r--1

> 의미 알아보기



sh fastqc.sh

./fastqc.sh

> 실행



cd -

> 이전으로 돌아간다.



./

> cp /fstorage_data/cylee~/align/* ./
>
> ./ 현재 디렉토리로 복사



<20.06.16>

fastqc 설치 후 실행
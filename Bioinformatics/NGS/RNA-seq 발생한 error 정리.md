# RNA-seq 발생한 error 정리

## Alignment index_path error

index_path 할 때 내 컴퓨터에 새로 다운 받아서 tar 하고 ./ make_grcm38_tran.sh 한 후 alignment 돌렸지만 error => 그래서 그냥 충렬 선생님 index path로 두고 진행했더니 alignment 가능

**error message**

1. sh make_grcm38_tran.sh 다운 시

   > Killed
   >
   > Index building failed; see error message

\>> 구글 검색시 파악해보니 

Cause: Linux "oom-killer" process stopped Progress index rebuild because the server has ran out of memory.

아마 컴퓨터가 32G라 메모리 부족으로 인해 다운 받지 못해 alignment도 build가 안되서 error가 난 듯 하다.

<br>

2. sudo sh align_dhgu.sh 실행 시

   > Error reading_rstarts[] array: 7876, 7896
   >
   > Error: Encountered internal HISAT2 exception (#1)
   >
   > (ERR): hisat2-align exited with value 1

\>> reference genome이 준비가 안되니 alignment도 진행 불가

<br>

**출처**

[make_grcm38 다운 시] https://knowledgebase.progress.com/articles/Article/index-build-fails-with-a-killed-message-on-screen


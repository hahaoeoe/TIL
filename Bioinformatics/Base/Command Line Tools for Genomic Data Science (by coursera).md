# Command Line Tools for Genomic Data Science

> * by 존스홉킨스대학교 (coursera 강의)
>
> - **강사:**[Liliana Florea, PhD](https://www.coursera.org/instructor/~12152931), Assistant Professor
>
>   McKusick-Nathans Institute of Genetic Medicine

<br>

## Week1. Basic Unix Commands

1. cd

2. cd .. 

3. cd .

4. ls

5. ls -l

6. ls -lf

7. more 

   > spacebar 누르면 정보가 넘어간다.
   >
   > /> 하면 chromosome 1에서 2로 넘어간다.

8. less

9. rm

10. rm -r

11. pwd

12.  cp

13. mv

14. head 

    > head - 50 peach.genome
    >
    > : line 50으로 가서 정보 보여줌

15. tail peach.genome

16. tail -15 peach.genome

17. cat peach.genes

    > concatenate

18. cat \*/*.genome

19. wc apple/apple.genome

20. wc -l apple/apple.genome

21. wc -l apple/apple.genome > nlines

22. wc -l < apple/apple.genes

23. ls | wc -l 

24. cat \*/*.genome | more

25. more months

26. sort months

    > 순서대로

27. sort -r months

    > 반대로

28. vi months

    > editort로 달력 내용 바꿈

29. sort -k2 months

    > 내용이 january 1 winter 
    >
    > october 10 fall
    >
    > 이렇게 있는데 1의 자리 숫자로 sort 했다는 의미이다.

30. sort -k 2n months

    > 1부터 순차적으로 sort

31. sort -k 2nr months

    > 30번 결과의 반대

32. sort -k 3 months

33. sort -k 3 -k 2n months

34. sort -k 3  -k 2rn

35. vi months

36. cut -f1 months

37. cut -f1,2 months

38. cut -f1-3 months

39. cut -d ' ' -f1 months

40. cut -d '  ' -f1-3 months

    > 앞에 -d 를 붙이면 내용이 f1만 나오게 하는 것
    >
    > f2까지 썼으면 f2까지 나온다.

41. cut -d ' ' -f3 months > seasons

42. more seasons

43. sort -u seasons

    > -u는 unique

44. uniq seasons

    > 이렇게 하면 winter가 두 번 나오게 됨
    >
    > hen we had the three spring lines replace with just one spring, summer, 
    >
    > fall and then we had December the winter might correspond to December at the end. 
    >
    > So you can see that winter now appears twice because it was stated two 
    >
    > distinct places within the file.

45. uniq seaons | sort -u

46. uniq -c seasons

    > c는 counts 즉, number of ouucrrences

47. grep root \*/*.samples

48. grep " 12 winter" months

49. grep "7 winter" months

50. grep -n "12 winter" months

51. more orchard

    > 내용 보여줌

52. cp orchard orchard.1

53. vi orchard.1

    > 에디터를 통해 두번째 줄 지움

54. more orchard.1

55. diff orchard orcahrd.1

    > 2d1이 화면에 뜸
    >
    > < the smell of cool, ~~ 두번 째줄 내용 나옴

56. vi orchard.1

    > evening을 bright night로 바꿈

57. diff orchard orchard.1

    > 2c1

58. more apple/apple.samples

59. comm apple/apple.samples pear/pear.samples

    > - 2개의 파일을 비교하는 리눅스 명령어
    > - (diff 와는 달리) 동일 행끼리만 단순 비교
    >
    > <br>
    >
    > - 첫번째 파일에만 있으면 첫번째 칸(탭 없음)
    > - 두번째 파일에만 있으면 두번째 칸(탭 1개)
    > - 둘 다 있으면 세번째 칸(탭 2개)

60. sort apple/apple.samples > apple/apple.samples.sorted

61. sort pear/pear.samples > pear/pear.samples.sorted

62. comm apple/apple.samples.sorted pear/pear.samples.sorted

63. comm  -1 -2 apple/apple.samples.sorted pear/pear.samples.sorted

    > 이렇게 하면 62번 결과에서 둘 다 있는 세번째 칸만 결과로 나오게 된다.

64. gzip apple.genome

65. ls -l apple.*

66. gunzip apple.genome.gz 

67. ls 

68. ls -l

69. bizp2 apple.genome

70. ls -l apple.genome.bz2

71. bunzip2 apple.genome.bz2

72. ls -l 

73. tar -cvf Apple.tar apple.genes apple.genome apple.samples

    > cvf 알아보기
    
74. gzip Apple.tar

75. tar -xvf Apple.tar

76. 

77. rm * 

    > everything remove in the local directory

78. tar -cvf AppleD.tar apple/

79. bzip2 AppleD.tar

80. mv AppleD.tar.bz2 sandbox

81. bunzip2 AppleD.tar.bv2

82. tar -xvf AppleD.tar

<br>

**Practical Expercises I**



## Week2. Sequences and Genomic Features

 








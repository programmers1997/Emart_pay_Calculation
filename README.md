급여계산기 프로그램
====================
우리 편의점은 파트타이머가 굉장히 많고(7~8명) 근무시간대도 아주 불규칙적이라 매달 급여를 계산하기 번거로웠다.!

그래서 만들어본 급여계산기 !

년, 월 ,시작 요일만 입력하면 자동으로 급여를 계산해주는 프로그램을 만들고 싶었다.

수정해야할 사항은 
1. 근무자가 바뀌었을 때
2. 최저시급이 올랐을 때
3. 근무 요일이 바뀌었을 때
4. 근무 시작요일 (모든 사람이 1일부터 시작이 아니니깐,)
만 수정해주면 된다.!

앞으로 추가할 기능
-------------------
대타자가 있을 경우 
빠진 사람의 시간만큼을 빼고 을 대타자에게 그 시간을 더해주는 것!


VERSION 1.0
--------------------
월급을 줘야 하는 년도,월, 해당월의 시작요일을 입력하면 모든 근무자의 당월 급여가 나오게 하였다.

우리 편의점 근무자들은 근무요일과 시간이 불규칙적이다 . 누구는 월(5시간),화(4시간), 목(5시간)  누구는 수(4시간),일(10시간) 
그리고 해당월이 31일일땐 어떤요일은 5번이고, 어떤 요일은 4번이다.

그렇기 때문에 생각한 건
예를들어 준석이의 근무요일이 수 일요일 이면
해당월의 수요일과 일요일의 개수를 COUNTING하고 급여로 환산하는 방법이었다.

VERSION 1.1
------------------------
아르바이트 특성상 한 근무자가 6개월 이상 근무하는 경우가 별로 없다. 짧으면 1~3달만에 그만둔다.
새로운 알바생이 들어왔을 때 , 그 근무자는 X일부터 시작할 것이다.
그럼 그 날부터 31일까지의 급여를 계산해주는 기능을 추가하였다.

앞에서 이미 요일을 counting하는 방법으로 코드를 만들었기 때문에
이 점을 고려하여 1일부터 x일까지의 요일 개수를 총 요일 개수에서 빼는 방법을 사용하였다.

오류 1 - > 15일부터 근무시작 이라하면 15일에 근무한 것을 포함시켜야하는데 이게 포함이 안됨.
해결 ---> startDayInput 을 받고 거기에 -1을 하여 해결
오류 2 -> 오류 1 해결방식으로 하면 문제가 또 발생함, 그리고 Start_day메소드 가 전반적으로 오류를 포함하고 있었음.
이를테면, input값 반영, remainder의 범위에 따른 배열 값 오류 등.. 
해결 ---> 수요일 것을 일단 바꿔놨음. 이걸 기반으로 월~일요일까지 다시 바꿔야함.
해결 ---> 알고보니 전역변수 startdayInput을 설정해놓고 지역변수에 또 같은 이름의 변수를 설정해버려서 .. 값이 이상하게 뜬거임

VERSION 1.2
------------------------------
추가근무량 or 감소근무량도 나타내기!
1차 완료

VERSION 1.25
-----------------------------
SEVEN배열을 나타내는 메소드가 너무 길어져서 이를 하나의 CLASS내의 여러 METHOD로 나눴다.  
원래 버전에는 하나의 METHOD에 월요일부터 금요일까지의 코드가 다 들어있었는데, 이를 요일별로 쪼갰다.  
그리고 중간에 들어온 사람을 가르키는 dayCount메소드도 하나의 Class로 설정하여 여러개의 메소드로 쪼갰다.  
+ 1/1일부터 시급이 올라 이를 반영했다.

VERSION 1.3
-------------------------
이전 버전까지는 중간에 들어온 사람의 급여를 계산하는 기능이었고, 1.3버전에서는 중간에 나간 사람의 급여를 나타내는 기능을 추가할 것이다.  
일단, 생각해둔 방법은 앞서 만들어놨던 기능은 예를들어 13일에 시작했다면 전체 급여에서 1~12일까지의 급여를 뺀 것이었다.  
이 기능을 그대로 이용하여 StartdayInput값에 +1을 더해줘 하루 더 늘려 계산하면 될 것이라 생각된다.  
그래서, PatCalculation클래스에 cutInPayCalculate() 메소드를 추가하였다. 

VERSION 1.31 
----------------------------
모든 클래스가 상속받기 때문에 한 기능을 추가하거나, 코드를 수정하게 되면 다른 클래스들 모두 수정해야 하는 불편함이 있다.  
또한, 굳이 안쓰는 기능들 또한 상속받기 때문에 이를 수정해보려고 한다.  
Option에 있는 기능은 거의 사용이 worker에서만 사용되기 때문에 상속받지 않고 객체를 생성해서 해결했다.  
각 worker들의 근무day빼곤 코드가 중복되어 이를 하나로 통합하여 메소드를 만들었다.  
그 결과 700줄이 넘던 코드가 430줄이 되었다.  
생각해보니까 근무자마다 근무요일이 달라서, 값이 다르게 나온다. RETURN형식으로 메소드를 바꿔야할듯  
return형식으로 바꾸어서 만들었고 매개변수는 각 요일로 설정했다. 이렇게 실행했을때 기존 PayCalculation 클래스에 있던 인스턴트변수들이 사용되지 않고 int타입의 메소드 매개변수들이 사용되어 day_result값이 0으로 나왔다.  그래서 인스턴트변수를 없애고 기존의 메소드에 있던 day_result의 변수들을 int타입의 메소드 안으로 가져왔다.  
결과적으로 700줄이었던 코드가 300줄이 되었다. !

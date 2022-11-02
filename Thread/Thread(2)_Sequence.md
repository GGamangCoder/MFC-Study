## 설비에서 시퀀스(Sequence) 진행시키는 주된 방법

이전에 쓰레드를 선언하고 실행시키는 부분을 봤다.
그럼 이제 Sequence를 구성하는 방법을 정리해보고자 한다.
첫째로 다음은 헤더파일에 선언해줘야 하는 변수들이다.

```cpp
// MyClass.h
enum ThreadWork
{
  Thread_RUN = 0,
  Thread_Stop
};
enum STEP
{
  STEP0 = 0,
  STEP1,
  STEP2,
  STEP3,
  ...
  STEPN,
  STEP_Count
};

ThreadWork	e_Thread;
STEP		e_Step;

void MyFunc();
```

여기서 enum 은 열거형으로 ThreadWork는 Thread상태를 보여주고 Working은 시퀀스 진행 스텝을 보여준다.

이제 cpp파일에서 쓰레드를 실행시키고 다시 시퀀스 함수(MyFunc())를 호출해준다.

+추가(22.11.02)
enum 을 대표하는 변수와 내부 변수는 보통 대문자를 사용하며 위와 같이 STEP_(Name) 으로 명명한다.
또한 각 스텝 혹은 변수들이 0부터 시작하므로 마지막 변수를 COUNT (갯수)로 사용하기도 한다.

```cpp
UINT MyClass::MyThread(LPVOID _mothod)
{
	MyClass *pThread = (MyClass*)_mothod;
	while (pThread->e_Thread == THREAD_RUN)
	{
    		pThread->MyFunc();    // Sequence를 구성할 함수
  
		Sleep(100);
	}

	return 0;
}
```

이때 Thread_Run 부분을 통해 쓰레드를 종료시킬 수 있다.

```cpp
void MyClass::MyFunc()
{
  switch (e_STEP)
  {
  case STEP0:
    // 준비 단계
    if(완료)  e_WorkStep = Step1;   // 다음 스탭 진행
    break;
  case STEP1:
    // 첫 번째 스탭 진행
    if(완료)  e_WorkStep = Step2;   // 다음 스탭 진행
    break;
  case STEP2:
    // 두 번째 스탭 진행
    if(완료)  e_WorkStep = Step3;   // 다음 스탭 진행
    break;
    
    ...
  }
}
```

다음과 같이 하면 쓰레드를 계속 돌릴 수 있고 순차적으로 Step을 진행한다.
이때 쓰레드를 중간에 멈추려면 e_WorkStep을 Thread_Stop으로 바꾸어 반복문을 빠져나가게 해주면 된다.
그럴 경우 쓰레드를 다시 실행시켜주면 되고 쓰레드는 계속 돌리되 내부 변수나 외부 함수를 통해 다룰 수도 있다.

### MFC Thread 사용
AfxbeginThread 에 등록

[MyClass 에서 MyThread 사용]
* 헤더파일

1. 포인터 선언
```cpp
CWinThread *pThread;
```

2. 함수 선언
```cpp
static UNIT (MyThread)(LPVOID _mothod );    // 반드시 static 선언
```

* cpp 파일
```cpp
0. 시작 시에 Thread 설정
 ex - OnInitDialog() 에서
 if(pThread == NULL)
 {
   AfxBeginThread(MyThread, this);
 }
```

1. 함수 구현(생성 함수) 
```
UINT MyClass::MyThread(LPVOID _mothod)
{
  MyClass *pThread = (MyClass*)_mothod;
   while(1)    // THread_RUNNING 일 때
   {
     pThread->Func();   // Func이라는 특정 함수에서 Thread 돌리기
     
     Sleep(1);
   }
   return 0;
}
```

2. 함수 소멸(Thread 소멸)
```cpp
void MyClass::Ondestroy()
{
  CDialog::OnDestroy();
   
  if(pThread != NULL)
   pThread = NULL;
}
```


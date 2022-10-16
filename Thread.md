## MFC Thread 사용
### AfxbeginThread 에 등록


### [MyClass 에서 MyThread 사용]
* 헤더파일

1. 포인터 선언
####  CWinThread *pThread;

2. 함수 선언
####  static UNIT (MyThread)(LPVOID _mothod );    // 반드시 static 선언


* cpp 파일
0. 시작 시에 Thread 설정
#### ex - OnInitDialog() 에서
#### if(pThread == NULL)
#### {
####   AfxBeginThread(MyThread, this);
#### }

1. 함수 구현(생성 함수) 
```
UINT CSequence::MyThread(LPVOID _mothod)
{
  MyClass *p1 = (MyClass*)_mothod;
   while(1)    // THread_RUNNING 일 때
   {
     p1->Func();   // Func이라는 특정 함수에서 Thread 돌리기
     
     Sleep(1);
   }
   return 0;
}
```

```cpp
// 이진 탐색 소스코드 구현(재귀 함수)
int binarySearch(vector<int>& arr, int target, int start, int end) {
    if (start > end) return -1;
    int mid = (start + end) / 2;
    // 찾은 경우 중간점 인덱스 반환
    if (arr[mid] == target) return mid;
    // 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
    else if (arr[mid] > target) return binarySearch(arr, target, start, mid - 1);
    // 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
    else return binarySearch(arr, target, mid + 1, end);
}
```

2. 함수 소멸(Thread 소멸)

#### void MyClass::Ondestroy()
#### {
####   CDialog::OnDestroy();
####   
####   if(pThread != NULL)
####     pThrad = NULL;
#### }



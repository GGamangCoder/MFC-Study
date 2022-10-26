## 현재 시간(날짜) 받아오는 방법

0. SYSTEMTIME
-> Log/ActualEx.md 를 참고하자

1. CTime

제일 기본적인 시간 클래스, 하지만 초기 단계 구현된 클래스이므로 1970년 이전과 2038년 이후 시간을 처리하지 못한다.
그래서 CTime.SetTime() 함수가 사라졌고 초기화할 때 한 번 시간을 저장하면 이후로 값 수정이 불가하다.

현재 시간을 받아올 때는 다음 함수를 이용한다.
```cpp
CTime CurTime = CTime::GetCurrentTime()
```

2. COleDateTime

CTime을 대체하기 위해 0~9999 년까지의 시간을 저장하기 위한 클래스이다.
CTime 클래스 대부분 기능을 포함하며 시간 값 수정이 자유롭다. 단 SetDate() 함수보단 SetDateTime() 함수를 지향한다.
참고로 GetTime() 함수는 제공하지 않으므로 각 연/월/일 등을 구분하여 얻어낸다.

```cpp
// 예제
COleDateTime COleDateTime;

CString strSeq_Year, strSeq_Month, strSeq_Day;
strSeq_Year.Format(_T("%d"), COleDateTime.GetYear());
strSeq_Month.Format(_T("%d"), COleDateTime.GetMonth());
strSeq_Day.Format(_T("%d"), COleDateTime.GetDay());

COleDateTime.SetDateTime(2022, 10, 26, 12, 00, 00);
```

3. CTimeSpan

CTimeSpan 클래스의 경우 시간을 저장하기 보단 기간을 저장하는 개념이다.
CTime 이나 COleDateTime 객체의 시간에 특정 기간을 더하거나 뺄 때 이용한다.
```cpp
CTimeSpan(day, hour, min, sec);
```

cf) 각 클래스는 서로 변환도 가능하다.

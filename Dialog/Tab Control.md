## Tab Control 이용

#### : 유사한 정보들을 제공하는 컨트롤들을 한 화면에 구성시켜준다. Page1, Page2, ... 과 같다고 생각하면 된다.

메인 프로젝트(다이얼로그)에 Tab Control 을 추가하고 원하는 Tab 갯수만큼 대화상자(다이얼로그)를 만들어준다.
새로 만든 대화상자 속성에서 'Style - Child, System Menu - False, Title Bar - False' 로 설정한다.
이 때, Tab Control의 변수를 추가하여 **m_Tab** 이라 선언하자.

참고: https://github.com/HeokseokMJ/MFC-Study/blob/main/Dialog/FormView(1).md 

FormView 때와 마찬가지로 메인.h 에 클래스와 객체 포인터 변수를 선언해주고 .cpp 에는 헤더를 include 해준다.
Tab Control 은 OnInitDialog 에서 초기화 작업을 해줘야 한다.

```cpp
	m_Tab.InsertItem(0, _T("/Tab1 Name/"));
	m_Tab.InsertItem(1, _T("/Tab2 Name/"));
	m_Tab.InsertItem(2, _T("/Tab3 Name/"));

	m_Tab.SetCurSel(0);

	CRect rect;
	m_Tab.GetWindowRect(&rect);

	pDlg1 = new Main;   // Main is Main Project(Class) Name
	pDlg1->Create(IDD_DIALOG1, &m_Tab);
	pDlg1->MoveWindow(0, 25, rect.Width(), rect.Height());
	pDlg1->ShowWindow(SW_SHOW);
	
	...
```
다이얼로그 2, 3도 마찬가지로 추가해주면 된다.

다음으로 탭 전환을 보여주기 위해서는 FormView 때와 비슷하게 만들어주면 된다.

참고 : https://github.com/HeokseokMJ/MFC-Study/blob/main/Dialog/FormView(2).md

탭에 대한 이벤트 처리기를 추가하여 SEL_CHANGE 형식의 메시지로 받고 탭이 눌릴경우 Show & Hide 로 위 참고 코드 내부와 동일하게 구현해주면 된다.
```cpp
void LogResult::OnTcnSelchangeTab2(NMHDR *pNMHDR, LRESULT *pResult)
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
  
	*pResult = 0;
}

```

끝.

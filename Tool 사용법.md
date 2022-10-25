## MFC 도구(TOOL) 사용 법


* Button
: 버튼을 추가한 뒤 더블 클릭을 할 경우 자동으로 이벤트처리기가 생성된다.
```cpp
"MyProject.h"
	afx_msg void OnBnClickedBtnA();
```

```cpp
"MyProject.cpp"

BEGIN_MESSAGE_MAP(Sequence, CDialog)
	...
	ON_BN_CLICKED(IDC_BTN_APPLY, &Sequence::OnBnClickedBtnA)
	...
END_MESSAGE_MAP()

void MyProject::OnBnClickedBtnApply()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
}
```

* MFC Button Control
: MFC Button Control 은 특히 색상을 입히기 용이하다.
```cpp
"MyProject.h"
	CMFCButton		m_btnInit;
```

다음으로 DoDataExchange에서 버튼에 대한 데이터 변환을 선언해주고 시작 다이얼로그(OnInitDialog)에서 초기화를 시켜준다.
```cpp
"MyProject.cpp"

void MyProject::DoDataExchange(CDataExchange* pDX)
{
	CDialog::DoDataExchange(pDX);
	DDX_Control(pDX, IDC_MFCBUTTON0, m_btn);
}

BOOL Sequence::OnInitDialog()
{
	CDialog::OnInitDialog();
  // 초기화
  m_btnInit.EnableWindowsTheming(FALSE);
  // 색상 지정
  m_btnInit.SetFaceColor(RGB(255, 0, 0));
}
```

* Check Box
: 체크박스를 생성 후 더블클릭하여 이벤트를 생성하거나 직접 MESSAGE_MAP 에 추가해준다.
그리고 다음과 같은 함수를 이용하여 체크 여부를 확인할 수 있다.(이 때 헤더파일에 체크박스에 대한 변수를 추가해줘야 한다.)
```cpp
	// 헤더파일 - 변수 선언
	CButton m_Chk;

	// MyProject.cpp
	// 체크 확인
	bState = m_Chk.GetCheck();
	// 강제 체크
	m_Chk.SetCheck();
```

* Edit Control


* Combo Box


* List Box
1. AddString
```cpp
	int AddString(LPCTSTR lpszItem);
```
 매개 변수 'lpszItem' 은 null 로 끝나는 문자열을 가리킨다.
 마지막 위치로부터 문자열을 추가한다.

2. InsertString
```cpp
	int InsertString(int nIndex, LPCTSTR lpszItem);
```
 매개 변수 'nIndex' 은 문자열을 삽입할 위치의 인덱스를 지정. 매개 변수가 -1 이면 문자열이 목록의 끝에 추가된다.
 'lpszItem' 은 마찬가지이며 반환값은 문자열이 삽입된 위치의 0부터 시작되는 인덱스이다.
 (가장 위에서부터 0, 1, 2, .. 순이다.)

3. DeleteString
```cpp
	int DeleteString(UINT nIndex);
```
 매개 변수 'nIndex' 은 삭제할 문자열의 인덱스를 지정하여 해당 라인을 모두 삭제한다. 이 때 반환값은 남아있는 문자열의 행의 수이다.

4. GetText
```cpp
	void GetText(int nIndex, CString& rString) const;
```
 'nIndex' 행에서 'rString' 에 문자열을 반아온다.

* Progress Control


* Tab Control


* Date Time Picker


* Month Calendar Control

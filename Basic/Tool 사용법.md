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
  CMFCButton m_btnInit;
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
: Edit Box를 생성한 후 원하는 값을 집어넣거나 불러올 수 있다. 일반적으로는 변수 선언을 따로 하지 않고 사용하는 경우가 많다.
```cpp
// 헤더 파일
CEdit m_edit;
```
```cpp
// cpp 파일
void MyProject::DoDataExchange(CDataExchange* pDX)
{
  CDialog::DoDataExchange(pDX);
  
  DDX_Control(pDX, IDC_EDIT0, m_Edit);
}
```
사용 시에는 필요한 부분에서 Get 이나 Set을 이용하여 값을 얻거나 설정할 수 있다.
```cpp
  // Edit 값을 불러와 변수 temp 에 저장
  CString temp;
  GetDlgItemText(IDD_EDIT0, temp);
  // Edit 값을 설정
  SetDlgItemText(IDD_EDIT0, _T(" 내용 "));
```

* Combo Box


* List Box
1. AddString
: 매개 변수 'lpszItem' 은 null 로 끝나는 문자열을 가리킨다.
 마지막 위치로부터 문자열을 추가한다.
```cpp
int AddString(LPCTSTR lpszItem);
```


2. InsertString
: 매개 변수 'nIndex' 은 문자열을 삽입할 위치의 인덱스를 지정. 매개 변수가 -1 이면 문자열이 목록의 끝에 추가된다.
 'lpszItem' 은 마찬가지이며 반환값은 문자열이 삽입된 위치의 0부터 시작되는 인덱스이다.
 (가장 위에서부터 0, 1, 2, .. 순이다.)

```cpp
int InsertString(int nIndex, LPCTSTR lpszItem);
```

3. DeleteString
: 매개 변수 'nIndex' 은 삭제할 문자열의 인덱스를 지정하여 해당 라인을 모두 삭제한다. 이 때 반환값은 남아있는 문자열의 행의 수이다.
```cpp
int DeleteString(UINT nIndex);
```

4. GetText
: 'nIndex' 행에서 'rString' 에 문자열을 반아온다.
```cpp
void GetText(int nIndex, CString& rString) const;
```
 
5. ResetContent
: 리스트에 대한 멤버 변수(mList)를 통해 
```cpp
mList.ResetContent();
```

+ 22.10.26 추가)
6. Find()
: CString 개체의 문자열 기준 좌측에서부터 문자 혹은 문자열을 검색(cf- ReverseFind() 는 우측에서부터 검색)

함수 원형 및 설명
```cpp
int Find( LPCTSTR lpszSub ) const;
// lpszSub : NULL로 종결되는 검색할 문자열
// 반환값 : 검색된 위치, 실패 시 -1

int Find( LPCTSTR lpszSub, int nStart ) const;
// lpszSub : NULL로 종결되는 검색할 문자열
// nStart : 검색을 시작할 위치. 생략 시 디폴트 값 0
// 반환값 : 검색된 위치, 실패 시 -1
```

7. GetText()
: 리스트 상자 내 원하는 행의 위치에서 CString 개체의 문자열을 받아온다.

함수 원형 및 설명
```cpp
int GetText(int nIndex, LPTSTR lpszBuffer) const;
// nIndex : 가져오려는 행의 인덱스
// lpszBuffer : 문자열을 받아올 CString 개체의 변수

ex)
mList.GetText(nIndex, strTemp);
```


* Progress Control


* Date Time Picker

참고: https://github.com/HeokseokMJ/MFC-Study/blob/main/Log/2.Log%20Load.md

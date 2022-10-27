## 다이얼로그 내에 다이얼로그 추가(FormView)


참고: https://github.com/HeokseokMJ/MFC-Study/blob/main/Dialog/Dialog%20%EC%B6%94%EA%B0%80.md

위 글에서 다이얼로그를 팝업 형태로 추가하여 버튼 누를 경우 팝업 창이 띄우지게끔 추가했다.
이번에는 화면(메인 다이얼로그)에 새로운 다이얼로그를 추가하는 방법에 대해 정리한다.

cf- 한 UI 내에서 끝내면 좋겠지만 모듈화가 되는지 따져보고 (또 다른 Form으로) 구분하는 것이 좋다.

1. 메인 UI에서 "Picture Control"을 생성한다.
- 이 공간에 새로운 다이얼로그가 추가되며 크기가 맞지 않는 경우 자동으로 스크롤을 생성한다.

2. 새로운 다이얼로그(Form)를 추가한다.
- 이 때 Dialog의 설정은 'Boarder: None', 'Style: Child" (Dialog 명은 IDD_FORM)
- Dialog의 이름은 MyForm으로 하고 클래스를 추가하되 기본 클래스를 CFormView를 상속받도록(기본 클래스로) 지정한다.
- 그리고 가상 함수로 Create 와 OnInitialUpdate 를 추가한다.

```cpp
// MyForm.h

#pragma once
  
// CMyForm 대화 상자
 
class CMyForm : public CFormView
{
	DECLARE_DYNAMIC(CMyForm)
 
public:
	CMyForm();
	CMyForm(UINT nIDTemplate);
	virtual ~CMyForm();
 
// 대화 상자 데이터입니다.
#ifdef AFX_DESIGN_TIME
	enum { IDD = IDD_FORM };
#endif
 
protected:
	virtual void DoDataExchange(CDataExchange* pDX);    // DDX/DDV 지원입니다.
 
	DECLARE_MESSAGE_MAP()
public:
	virtual BOOL Create(LPCTSTR lpszClassName, LPCTSTR lpszWindowName, DWORD dwStyle, const RECT& rect, CWnd* pParentWnd, UINT nID, CCreateContext* pContext = NULL);
	virtual void OnInitialUpdate();
};
```
2-1. 가상함수 Create & OnInitialUpdate
```cpp
// MyForm.cpp
BOOL CMyForm::Create(LPCTSTR lpszClassName, LPCTSTR lpszWindowName, DWORD dwStyle, const RECT& rect, CWnd* pParentWnd, UINT nID, CCreateContext* pContext)
{
	return CFormView::Create(lpszClassName, lpszWindowName, dwStyle, rect, pParentWnd, nID, pContext);
}

void CMyForm::OnInitialUpdate()
{
	CFormView::OnInitialUpdate();
}
```

3. 메인 다이얼로그
- 헤더파일에 새로 추가한 클래스의 헤더파일을 include 해주고 아래 변수들을 추가한다.
- 실제 화면에 새로운 다이얼로그를 포함시켜줄 AllocForm() 함수를 정의해준다.
- 이 때 Ondestroy() 에 view 포인터를 소멸해준다.

```cpp
// 메인.h
#include "CMyForm.h"  // 제일 위에 추가해주기

public:
	CMyForm *m_pForm; // 새로 추가한 Dialog 객체
	void AllocForm(); // Picture Control에 Dialog를 추가하는 함수

```
```cpp
// 메인.cpp

void CExamFormAttachDialogDlg::AllocForm()
{
	CCreateContext context;
	ZeroMemory(&context, sizeof(context));
 
	CRect rectOfPanelArea;
 
	GetDlgItem(IDC_STATIC_RECT)->GetWindowRect(&rectOfPanelArea);
	ScreenToClient(&rectOfPanelArea);
	m_pForm = new CMyForm();
	m_pForm->Create(NULL, NULL, WS_CHILD | WS_VSCROLL | WS_HSCROLL, rectOfPanelArea, this, IDD_FORM_MYFORM, &context);
	m_pForm->OnInitialUpdate();
	m_pForm->ShowWindow(SW_SHOW);
	GetDlgItem(IDC_STATIC_RECT)->DestroyWindow();
}

void CExamFormAttachDialogDlg::OnDestroy()
{
	CDialogEx::OnDestroy();

	if (m_pForm != NULL)
	{
		m_pForm->DestroyWindow();
	}
}
```



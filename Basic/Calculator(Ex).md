## 계산기 만들기
MFC 학습을 하기 위한 ..

연습하며 작성했던 전문을 첨부한다.
```cpp

// Cal_ExDlg.h : 헤더 파일
//

#pragma once


// CCal_ExDlg 대화 상자
class CCal_ExDlg : public CDialogEx
{
// 생성입니다.
public:
	CCal_ExDlg(CWnd* pParent = NULL);	// 표준 생성자입니다.

// 대화 상자 데이터입니다.
	enum { IDD = IDD_CAL_EX_DIALOG };

	protected:
	virtual void DoDataExchange(CDataExchange* pDX);	// DDX/DDV 지원입니다.


// 구현입니다.
protected:
	HICON m_hIcon;

	// 생성된 메시지 맵 함수
	virtual BOOL OnInitDialog();
	afx_msg void OnSysCommand(UINT nID, LPARAM lParam);
	afx_msg void OnPaint();
	afx_msg HCURSOR OnQueryDragIcon();
	DECLARE_MESSAGE_MAP()
public:
	void PSStart();

	int BeforeData;
	int AfterData;
	int Sign;	//plus = 1, minus = 2, multi = 3, divid = 4
	int ResultData;
	bool SignCheck;

	CString ViewData;

public:
	void ResultViewFn(int NumberData, int Mode = 1);
	void Reset();

public:
	afx_msg void OnBnClickedButton1();
	afx_msg void OnBnClickedButton2();
	afx_msg void OnBnClickedButton3();
	afx_msg void OnBnClickedButton4();
	afx_msg void OnBnClickedButton5();
	afx_msg void OnBnClickedButton6();
	afx_msg void OnBnClickedButton7();
	afx_msg void OnBnClickedButton8();
	afx_msg void OnBnClickedButton9();
	afx_msg void OnBnClickedButton0();
	afx_msg void OnBnClickedButtonPlus();
	afx_msg void OnBnClickedButtonMinus();
	afx_msg void OnBnClickedButtonMulti();
	afx_msg void OnBnClickedButtonDivid();
	afx_msg void OnBnClickedButtonEqual();
};

```

```cpp

// Cal_ExDlg.cpp : 구현 파일
//

#include "stdafx.h"
#include "Cal_Ex.h"
#include "Cal_ExDlg.h"
#include "afxdialogex.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// 응용 프로그램 정보에 사용되는 CAboutDlg 대화 상자입니다.

class CAboutDlg : public CDialogEx
{
public:
	CAboutDlg();

// 대화 상자 데이터입니다.
	enum { IDD = IDD_ABOUTBOX };

	protected:
	virtual void DoDataExchange(CDataExchange* pDX);    // DDX/DDV 지원입니다.

// 구현입니다.
protected:
	DECLARE_MESSAGE_MAP()
};

CAboutDlg::CAboutDlg() : CDialogEx(CAboutDlg::IDD)
{
}

void CAboutDlg::DoDataExchange(CDataExchange* pDX)
{
	CDialogEx::DoDataExchange(pDX);
}

BEGIN_MESSAGE_MAP(CAboutDlg, CDialogEx)
END_MESSAGE_MAP()


// CCal_ExDlg 대화 상자



CCal_ExDlg::CCal_ExDlg(CWnd* pParent /*=NULL*/)
	: CDialogEx(CCal_ExDlg::IDD, pParent)
{
	m_hIcon = AfxGetApp()->LoadIcon(IDR_MAINFRAME);
}

void CCal_ExDlg::DoDataExchange(CDataExchange* pDX)
{
	CDialogEx::DoDataExchange(pDX);
}

BEGIN_MESSAGE_MAP(CCal_ExDlg, CDialogEx)
	ON_WM_SYSCOMMAND()
	ON_WM_PAINT()
	ON_WM_QUERYDRAGICON()
	ON_BN_CLICKED(IDC_BUTTON_1, &CCal_ExDlg::OnBnClickedButton1)
	ON_BN_CLICKED(IDC_BUTTON_2, &CCal_ExDlg::OnBnClickedButton2)
	ON_BN_CLICKED(IDC_BUTTON_3, &CCal_ExDlg::OnBnClickedButton3)
	ON_BN_CLICKED(IDC_BUTTON_4, &CCal_ExDlg::OnBnClickedButton4)
	ON_BN_CLICKED(IDC_BUTTON_5, &CCal_ExDlg::OnBnClickedButton5)
	ON_BN_CLICKED(IDC_BUTTON_6, &CCal_ExDlg::OnBnClickedButton6)
	ON_BN_CLICKED(IDC_BUTTON_7, &CCal_ExDlg::OnBnClickedButton7)
	ON_BN_CLICKED(IDC_BUTTON_8, &CCal_ExDlg::OnBnClickedButton8)
	ON_BN_CLICKED(IDC_BUTTON_9, &CCal_ExDlg::OnBnClickedButton9)
	ON_BN_CLICKED(IDC_BUTTON_0, &CCal_ExDlg::OnBnClickedButton0)
	ON_BN_CLICKED(IDC_BUTTON_PLUS, &CCal_ExDlg::OnBnClickedButtonPlus)
	ON_BN_CLICKED(IDC_BUTTON_MINUS, &CCal_ExDlg::OnBnClickedButtonMinus)
	ON_BN_CLICKED(IDC_BUTTON_MULTI, &CCal_ExDlg::OnBnClickedButtonMulti)
	ON_BN_CLICKED(IDC_BUTTON_DIVID, &CCal_ExDlg::OnBnClickedButtonDivid)
	ON_BN_CLICKED(IDC_BUTTON_Equal, &CCal_ExDlg::OnBnClickedButtonEqual)
END_MESSAGE_MAP()


// CCal_ExDlg 메시지 처리기

BOOL CCal_ExDlg::OnInitDialog()
{
	CDialogEx::OnInitDialog();

	// 시스템 메뉴에 "정보..." 메뉴 항목을 추가합니다.

	// IDM_ABOUTBOX는 시스템 명령 범위에 있어야 합니다.
	ASSERT((IDM_ABOUTBOX & 0xFFF0) == IDM_ABOUTBOX);
	ASSERT(IDM_ABOUTBOX < 0xF000);

	CMenu* pSysMenu = GetSystemMenu(FALSE);
	if (pSysMenu != NULL)
	{
		BOOL bNameValid;
		CString strAboutMenu;
		bNameValid = strAboutMenu.LoadString(IDS_ABOUTBOX);
		ASSERT(bNameValid);
		if (!strAboutMenu.IsEmpty())
		{
			pSysMenu->AppendMenu(MF_SEPARATOR);
			pSysMenu->AppendMenu(MF_STRING, IDM_ABOUTBOX, strAboutMenu);
		}
	}

	// 이 대화 상자의 아이콘을 설정합니다.  응용 프로그램의 주 창이 대화 상자가 아닐 경우에는
	//  프레임워크가 이 작업을 자동으로 수행합니다.
	SetIcon(m_hIcon, TRUE);			// 큰 아이콘을 설정합니다.
	SetIcon(m_hIcon, FALSE);		// 작은 아이콘을 설정합니다.

	// TODO: 여기에 추가 초기화 작업을 추가합니다.
	GetDlgItem(IDC_STATIC_View)->SetWindowTextA(_T(""));


	return TRUE;  // 포커스를 컨트롤에 설정하지 않으면 TRUE를 반환합니다.
}

void CCal_ExDlg::OnSysCommand(UINT nID, LPARAM lParam)
{
	if ((nID & 0xFFF0) == IDM_ABOUTBOX)
	{
		CAboutDlg dlgAbout;
		dlgAbout.DoModal();
	}
	else
	{
		CDialogEx::OnSysCommand(nID, lParam);
	}
}

// 대화 상자에 최소화 단추를 추가할 경우 아이콘을 그리려면
//  아래 코드가 필요합니다.  문서/뷰 모델을 사용하는 MFC 응용 프로그램의 경우에는
//  프레임워크에서 이 작업을 자동으로 수행합니다.

void CCal_ExDlg::OnPaint()
{
	if (IsIconic())
	{
		CPaintDC dc(this); // 그리기를 위한 디바이스 컨텍스트입니다.

		SendMessage(WM_ICONERASEBKGND, reinterpret_cast<WPARAM>(dc.GetSafeHdc()), 0);

		// 클라이언트 사각형에서 아이콘을 가운데에 맞춥니다.
		int cxIcon = GetSystemMetrics(SM_CXICON);
		int cyIcon = GetSystemMetrics(SM_CYICON);
		CRect rect;
		GetClientRect(&rect);
		int x = (rect.Width() - cxIcon + 1) / 2;
		int y = (rect.Height() - cyIcon + 1) / 2;

		// 아이콘을 그립니다.
		dc.DrawIcon(x, y, m_hIcon);
	}
	else
	{
		CDialogEx::OnPaint();
	}
}

// 사용자가 최소화된 창을 끄는 동안에 커서가 표시되도록 시스템에서
//  이 함수를 호출합니다.
HCURSOR CCal_ExDlg::OnQueryDragIcon()
{
	return static_cast<HCURSOR>(m_hIcon);
}


void CCal_ExDlg::PSStart()
{
	Reset();
}

void CCal_ExDlg::Reset()
{
	BeforeData = 0;
	AfterData = 0;
	Sign = 0;
	ResultData = 0;
	ViewData = "0";
	SignCheck = false;

	ResultViewFn(0, 0);
}

void CCal_ExDlg::OnBnClickedButton1()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(1);
}


void CCal_ExDlg::OnBnClickedButton2()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(2);
}


void CCal_ExDlg::OnBnClickedButton3()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(3);
}


void CCal_ExDlg::OnBnClickedButton4()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(4);
}


void CCal_ExDlg::OnBnClickedButton5()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(5);
}


void CCal_ExDlg::OnBnClickedButton6()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(6);
}


void CCal_ExDlg::OnBnClickedButton7()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(7);
}


void CCal_ExDlg::OnBnClickedButton8()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(8);
}


void CCal_ExDlg::OnBnClickedButton9()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(9);
}


void CCal_ExDlg::OnBnClickedButton0()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	ResultViewFn(0);
}


void CCal_ExDlg::ResultViewFn(int NumberData, int Mode)
{
	CString Temp;

	if (Mode == 0)
	{
		Temp.Format("%d", NumberData);
	}
	else
	{
		GetDlgItem(IDC_STATIC_View)->GetWindowTextA(Temp);
		if (Temp == "0" || SignCheck == true)
		{
			SignCheck = false;
			Temp.Format("%d", NumberData);
		}
		else
		{
			Temp.AppendFormat("%d", NumberData);
		}
	}

	if (Sign == 0)
	{
		BeforeData = atoi(Temp);
	}
	else
	{
		AfterData = atoi(Temp);
	}

	GetDlgItem(IDC_STATIC_View)->SetWindowTextA(Temp);
}


void CCal_ExDlg::OnBnClickedButtonPlus()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	// Plus
	Sign = 1;
	SignCheck = true;
	GetDlgItem(IDC_STATIC_View)->GetWindowTextA(ViewData);
}


void CCal_ExDlg::OnBnClickedButtonMinus()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	// Minus
	Sign = 2;
	SignCheck = true;
	GetDlgItem(IDC_STATIC_View)->GetWindowTextA(ViewData);
}


void CCal_ExDlg::OnBnClickedButtonMulti()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	// Multiply
	Sign = 3;
	SignCheck = true;
	GetDlgItem(IDC_STATIC_View)->GetWindowTextA(ViewData);
}


void CCal_ExDlg::OnBnClickedButtonDivid()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	// Divide
	Sign = 4;
	SignCheck = true;
	GetDlgItem(IDC_STATIC_View)->GetWindowTextA(ViewData);
}


void CCal_ExDlg::OnBnClickedButtonEqual()
{
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	// Equals
	CString Temp;

	GetDlgItem(IDC_STATIC_View)->GetWindowTextA(Temp);
	BeforeData = atoi(ViewData);
	AfterData = atoi(Temp);

	switch (Sign)
	{
	case 0:
		break;
	case 1:
		ResultData = BeforeData + AfterData;
		break;
	case 2:
		ResultData = BeforeData - AfterData;
		break;
	case 3:
		ResultData = BeforeData * AfterData;
		break;
	case 4:
		ResultData = BeforeData / AfterData;
		break;
	}
	Temp.Format("%d", ResultData);
	GetDlgItem(IDC_STATIC_View)->SetWindowTextA(Temp);
}

```

(참고 사진 - 실행 화면)

![캡처2](https://user-images.githubusercontent.com/94775103/197983534-4eef96f7-7448-411d-9698-0ed18df1bb62.JPG)

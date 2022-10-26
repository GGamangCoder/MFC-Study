## 다이얼로그 추가하는 방법

* 팝업 창으로 띄우는 방법

메인 다이얼로그(혹은 부모 다이얼로그)를 가진 MFC 프로젝트를 실행시킨다. - 부모(메인) Main
// IDD_MAIN
생성된 프로젝트 리소스 뷰에서 다이얼로그를 추가 해준다.(dialog 삽입) - 자식 Child
// IDD_SUB
생성된 다이얼로그를 우클릭하여 클래스를 생성해준다.
( 사진 )

이제, 메인 프로젝트(다이얼로그 cpp파일)에 추가된 다이얼로그 헤더파일을 추가시켜준다.
```cpp
// Main.cpp
#include "Child.h"

	// MESSAGE_MAP
	ON_BN_CLICKED((버튼 ID), &Main::OnBnClickedBTN)

```
다시 메인 프로젝트의 헤더파일에 들어가 0식 다이얼로그 **클래스**와 이에 대한 **객체**를 멤버 변수로 선언해준다.
```cpp
// Main.h
class Child;

// (중략)
	Child *pChildDlg;
	afx_msg void OnBnClickedBTN();
```

위 코드에 포함된 것처럼 버튼을 하나 만들어 이벤트 처리기를 추가한다.
그리고 버튼 이벤트 함수에 다음과 같은 코드를 추가하면 정상적으로 팝업이 띄워지는 것을 확인할 수 있다.
```cpp
void Main::OnBnClickedBTN()
{
	pChildDlg = new Child();
	pChildDlg->Create(IDD_SUB, this);
	pChildDlg->ShowWindow(SW_SHOW);
}
```

## FormView 전환 - 여러 화면 스위칭

### Tab Control과 유사한 FormView 이용하기

이번 내용은 간단히만 정리하겠다.
바로 전 내용 FormView(1)을 참고(https://github.com/HeokseokMJ/MFC-Study/blob/main/Dialog/FormView(1).md)하여
다이얼로그 3개를 생성했다고 하자. (MyForm1, MyForm2, MyForm3)

그리고 메인 프로젝트에(정확히는 헤더파일에) ShowForm(int nIndex) 를 선언한다.
cf- 다른 내용은 똑같으며 AlloForm 등 반복되는 부분을 함수화 혹은 클래스화시켜주면 더 편리하다.

ShowForm을 선언하여 정의하는 이유는 화면이 전환되는 효과를 나타내기 위함이다.
```cpp
void CExamFormSwitchingDlg::ShowForm(int idx)
{
	switch (idx)
	{
	case 0:
		m_pForm1->ShowWindow(SW_SHOW);
		m_pForm2->ShowWindow(SW_HIDE);
		m_pForm3->ShowWindow(SW_HIDE);
		break;
	case 1:
		m_pForm1->ShowWindow(SW_HIDE);
		m_pForm2->ShowWindow(SW_SHOW);
		m_pForm3->ShowWindow(SW_HIDE);
		break;
	case 2:
		m_pForm1->ShowWindow(SW_HIDE);
		m_pForm2->ShowWindow(SW_HIDE);
		m_pForm3->ShowWindow(SW_SHOW);
		break;
	}
}
```

** Tab Control과 기능은 유사하나 UI를 사용자 요구에 맞게 커스터마이징 할 수 있다는 장점이 있다.


## Log 생성 후 날짜 별로 폴더 생성하여 텍스트 파일에 저장

이 경우 헤더파일에는 열거형과 함수 선언을 시켜준다.
```cpp
	enum CodeType
	{
		System = 1001,
		Work,
		Teaching
	};
	enum CodeType
	{
		A,
		B,
		...
	};

	...

	void LogEvent(int CodeType, int LogType);
```
열거형을 선언해주는 이유는 각 타입별로 명시되어 해당 코드를 봤을 때 직관적으로 파악하기 위함이다.

```cpp
void Sequence::LogEvent(int CodeType, int LogType)
{
  	// System 시간을 불러온다
	SYSTEMTIME Cur_Time;
	GetLocalTime(&Cur_Time);
	
	CString strYear, strMonth, strDay, strHour, strMin, strSec;
	CString DirPath;
	strYear.Format(_T("%04d"), Cur_Time.wYear);
	strMonth.Format(_T("%02d"), Cur_Time.wMonth);
	strDay.Format(_T("%02d"), Cur_Time.wDay);
	strHour.Format(_T("%02d"), Cur_Time.wHour);
	strMin.Format(_T("%02d"), Cur_Time.wMinute);
	strSec.Format(_T("%02d"), Cur_Time.wSecond);

  	// 실행 파일 기준으로 년/월/일 순으로 폴더를 생성한다.
	DirPath = _T(".\\Log\\") + strYear;   // 현재 폴더의 Log 폴더 참조
	CreateDirectory(DirPath, NULL);
	DirPath += _T("\\") + strMonth;
	CreateDirectory(DirPath, NULL);
	DirPath += _T("\\") + strDay;
	CreateDirectory(DirPath, NULL);

	CStdioFile	file;
	CString		LogData;
	LogData = strYear + _T(".") + strMonth + _T(".") + strDay + _T(".") + strHour + _T(":")
		+ strMin + _T(":") + strSec + _T(": ");

	if (file.Open(DirPath + _T("\\Log_Event.txt"), CFile::modeNoTruncate | CFile::modeCreate | CFile::modeReadWrite))
	{
		// 시간 - Code 번호
		file.SeekToEnd();

		CString temp;
		temp.Format(_T("%d"), CodeType);
		file.WriteString(LogData + _T("#") + temp + _T(" "));

    		// 사용자 임의의 코드 타입을 지정한다.
		switch (CodeType)
		{
		case System:
			file.WriteString(_T("System: "));
			break;

		case Work:
			file.WriteString(_T("Work: "));
			break;

		case Teaching:
			file.WriteString(_T("Teaching: "));
			break;
		}

    		// 각 상황별로 로그를 파일에 저장한다.
		switch (LogType)
		{
		case A:
			file.WriteString(_T("A에 대한 상황 설명\n"));
			break;

		case B:
			file.WriteString(_T("B에 대한 상황 설명\n"));
			break;
      
		default:
			break;
		}
	}
	file.Close();

}
```

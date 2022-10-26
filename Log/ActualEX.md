## 프로젝트에서 사용한 Log 남겨 요일별 파일 저장

```cpp
void MyProject::Log(int CodeType, int LogType)
{
  // 실시간 - 연/월/일/시/분/초 받아옴 - 반환형은 정수형이기 때문에 문자열로 변환
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

  // 파일 경로를 지정해 준다.
	DirPath = _T(".\\Log\\") + strYear;
	CreateDirectory(DirPath, NULL);
	DirPath += _T("\\") + strMonth;
	CreateDirectory(DirPath, NULL);
	DirPath += _T("\\") + strDay;
	CreateDirectory(DirPath, NULL);
  
	CStdioFile	file;
	CString		LogData;
	LogData = strYear + _T(".") + strMonth + _T(".") + strDay + _T(".") + strHour + _T(":") + strMin + _T(":") + strSec + _T(". ");

  // 해당 날짜 파일이 있을 경우 폴더 및 파일을 생성하지 않고, 없을 경우 새로 생성한다.
	if (file.Open(DirPath + _T("\\Log_Event.txt"), CFile::modeNoTruncate | CFile::modeCreate | CFile::modeReadWrite))
	{
		// 시간 - Code 번호
		file.SeekToEnd();

		CString temp;
		CString strCodeType;
		temp.Format(_T("%d"), CodeType);
		file.WriteString(LogData + _T("(#") + temp + _T(") "));

		switch (CodeType)
		{
		case System:
			file.WriteString(_T("System: "));
			strCodeType.Format(_T("System: "));
			break;

		case Work:
			file.WriteString(_T("Move: "));
			strCodeType.Format(_T("Move: "));
			break;

		case Teaching:
			file.WriteString(_T("Teaching: "));
			strCodeType.Format(_T("Teaching: "));
			break;
		}

		switch (LogType)
		{
		case Home:
			file.WriteString(_T("Home Button Clicked!\n"));
			mL_Log_Event.InsertString(0, LogData + _T("(#") + temp + _T(") ") + strCodeType + _T("Home Button Clicked!"));
			break;

		case AutoRun:
			file.WriteString(_T("AutoRun Button Clicked!\n"));
			mL_Log_Event.InsertString(0, LogData + _T("(#") + temp + _T(") ") + strCodeType + _T("AutoRun Button Clicked!"));
			break;

		case Stop:
			file.WriteString(_T("Stop Button Clicked!\n"));
			mL_Log_Event.InsertString(0, LogData + _T("(#") + temp + _T(") ") + strCodeType + _T("Stop Button Clicked!"));
			break;
		}
	}
	file.Close();
}

```

프로젝트 실습 시 이용했던 코드를 그대로 인용했다. LogEvent라는 함수는 어떻게 만들던 본인의 취향이다.
일반적으로 사용자 임의의 CodeType과 기록을 위한(혹은 디버깅) LogType을 지정하고 헤더파일에 열거형(enum)으로 선언해준다.

- 참고(헤더 파일)
```cpp
	enum CodeType
	{
		System = 1001,
		Work,
		Teaching,
    ...
	};
  enum LogType
  {
		Home = 0,
		AutoRun,
		Stop,
    ...
  };
```

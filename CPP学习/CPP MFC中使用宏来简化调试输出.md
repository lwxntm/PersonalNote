# CPP MFC中使用宏来简化调试输出

```c++
#define log(fmt, ...)\
CString cstr;\
cstr.Format(CString(fmt), __VA_ARGS__);\
AfxMessageBox(cstr)

```


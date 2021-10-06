# unlocker库的使用

首先有两个工具函数：作用是支持中文

```c++
#include <locale>
#include <codecvt>
#include <string>

inline std::wstring to_wide_string(const std::string& input) {
	std::wstring_convert<std::codecvt_utf8<wchar_t>> converter;
	return converter.from_bytes(input);
}

// convert wstring to string
inline std::string to_byte_string(const std::wstring& input) {
	// std::wstring_convert<std::codecvt_utf8_utf16<wchar_t>> converter;
	std::wstring_convert<std::codecvt_utf8<wchar_t>> converter;
	return converter.to_bytes(input);
}
```

然后是主题：

```c++
int main(int argc, char* argv[]) {
	if (argc < 2) {
		return -1;
	}
	unlocker::File* d = unlocker::Path::Exists(
		to_wide_string(argv[1]));
	if (d) {
		d->Delete();
		delete d;
	}
}
```


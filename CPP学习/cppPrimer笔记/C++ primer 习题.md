# C++ primer 习题

## 引用和指针的理解

#### 引用: &ri

```c++
#include <iostream>
using namespace std;
auto main() -> int
{
	int i = 19;
	//先说引用，&符号出现在等号左边时，说明一个变量是另一个变量的引用
	int& ri = i; //ri是一个对i的引用，ri不是一个内存里的对象，他没有自己的地址。
	//  int &ri = i;
	//	int & ri = i; //这两句都和上一句完全等价， &符号写在三个位置都是允许的。
	//int& rn;    这一句会编译错误，引用必须初始化。
	ri = 10; //修改引用的值，就相当于修改引用所指向的对象的值
	//ri = &i;  //错误，ri是一个int对象的引用，只能给他赋int类型的值
	cout << i << endl << ri << endl;  //这两个输出完全一致
	cout << &i << endl << &ri << endl;  //这两个输出也完全一致，都是i的地址，引用没有自己的内存地址
	const int* const pi = &i;
}
```

#### 指针: *pi

```c++
#include <iostream>
using namespace std;
auto main() -> int
{
	int i = 19;
	int* pi = &i; //pi是一个指针，指向int类型的数据
	int* pi2;
	pi2 = &i;   /*这两行的作用和定义pi时一样，也就是说指针是一个内存里真正存在的对象，可以先定义，再初始化。
				&i中， &是取地址符号，代表把i的地址交给等号左边的指针*/
	
	cout << pi<<endl<<pi2<<endl; //输出的是i的地址。
	cout << *pi<<endl<<*pi2<<endl; //输出的是i的值，*是解引用操作符，*pi意思是pi所指向的对象的值。
	pi = nullptr;//相当于把指针pi改变成空指针，等价于pi = 0或者pi = NULL(推荐使用nullptr)
	cout << *pi << endl << pi2 << endl; //这一句能编译通过，但是运行会报错，因为尝试对空指针进行解引用。
}
```

#### const的理解

```c++
#include <iostream>
using namespace std;
auto main() -> int
{
	int i = 19;
	int* const cpi = &i;
						   /*等号左边的式子可以从右往左读，cpi首先是const的，说明cpi是常量，
						    * 然后cpi是一个int类型的指针，它的内容是i的地址。
						    */
	//cpi = nullptr;       //报错，因为cpi是常量，初始化后就不能改变。

	const int * const cpi2 = &i;   /* cpi2首先是const的，说明cpi是常量，
									* 然后cpi是一个const int 类型的指针，它的内容是i的地址。
									* 虽然i其实并不是const，但是并不影响，因为我们可以运行cpi2
									* 自以为是的认为i是一个const的值，这不会有坏处，只是他的自以为是而已。
									*/

	const int ci = 10;
	//int* pci = &ci;               //报错，因为ci的类型是const int，而不是int
	const int* pci = &ci;          //合法，pci是指向 const int 类型的指针
	const int* cpci = &ci;          //合法，pci是指向 const int 类型的常量指针
}
```



### 3 string 

```c++
#include <string>
#include <iostream>
using namespace std;
auto main() -> int
{
    //练习3.2：编写一段程序从标准输入中一次读入一整行，然后修改该程序使其一次读入一个词。
	string s1;
	getline(cin, s1);
	cout << s1;
    
    //然后修改该程序使其一次读入一个词。
	string s2;
	cin >> s2;
	cout << s2;
}
```

```
#include <string>
#include <iostream>
using namespace std;

int main()
{
	//练习3.4：编写一段程序读入两个字符串，比较其是否相等并输出结果。
	//如果不相等，输出较大的那个字符串。
	//改写上述程序，比较输入的两个字符串是否等长，如果不等长，输出长度较大的那个字符串

	string s1, s2;
	cin >> s1 >> s2;
	if (s1 == s2)
	{
		cout << "两个字符串相等！" << endl;
	}
	else
	{
		if (s1.size() > s2.size())
		{
			cout << s1 << endl;
		}
		else cout << "更长的一个是" << s2 << endl;
	}
}
```

```c++
#include <string>
#include <iostream>
using namespace std;

int main()
{
	// 练习3.5：编写一段程序从标准输入中读入多个字符串并将它们连接在一起，输出连接成的大字符串。
	// 然后修改上述程序，用空格把输入的多个字符串分隔开来。
	cout << "输入字符串，输入quit时结束输入"<<endl;
	string s1,temps;
	while (cin>> temps)
	{
		if (temps=="quit")
		{
			break;
		}
		if (!s1.empty()) s1 += " ";
		s1 += temps;
	}
	cout << s1 << endl;
}

```



### 3 vector 

```c++
#include <iostream>
#include <string>
#include <vector>
using namespace std;
/*
练习3.17：从cin读入一组词并把它们存入一个vector对象，然后设法把所有词都改写为大写形式。输出改变后的结果，每个词占一行。
*/
int main(int argc, char* argv[])
{
	vector<string> s_vec;
	string temps;
	cout << "` is stop input" << endl;
	while (cin >> temps)
	{
		if (temps == "`")
		{
			break;
		}
		s_vec.push_back(temps);
	}

	for (auto& item : s_vec)
		for (auto& c : item)
			c = toupper(c);
	for (const auto& item : s_vec)
		cout << item << endl;
}

```

```c++
#include <iostream>
#include <string>
#include <vector>
using namespace std;

/*
练习3.19：如果想定义一个含有10个元素的vector对象，所有元素的值都是42，请列举出三种不同的实现方法。哪种方法更好呢？为什么？
*/

template <typename T>
void PrintVector(vector<T> v)
{
	for (T item : v)
		cout << item << endl;
}

int main(int argc, char* argv[])
{
	vector<int> v1(10, 42);
	PrintVector(v1);

	vector<int> v2{42, 42, 42, 42, 42, 42, 42, 42, 42, 42};
	PrintVector(v2);

	vector<int> v3(10);
	for (vector<int>::size_type i = 0; i < v3.size(); ++i)
	{
		v3[i] = 42;
	}
	PrintVector(v3);
}

```



### 3.5 数组

```c++
#include<iostream>
using namespace std;

int main()
{
	/*
	 * 练习3.40：编写一段程序，定义两个字符数组并用字符串字面值初始化它们；
	 * 接着再定义一个字符数组存放前两个数组连接后的结果。
	 * 使用strcpy和strcat把前两个数组的内容拷贝到第三个数组中。
	 */

	const char chs1[] = "hello, ";
	const char chs2[] = "world!";
	const size_t mix_size = size(chs1) + size(chs2);
	char mix_chars[mix_size];
	strcpy_s(mix_chars, chs1);
	strcat_s(mix_chars, chs2);
	cout << mix_chars << endl;
}

```

### 6.2 传参

```c++
#include<iostream>
#include <vector>
using namespace std;

/*
练习6.21：编写一个函数，令其接受两个参数：一个是int型的数，另一个是int指针。函数比较int的值和指针所指的值，返回较大的那个。在该函数中指针的类型应该是什么？
*/
int Greater2(int i1, int* i2)
{
	if (i1 > *i2)
	{
		return i1;
	}
	else return *i2;
}

int main(int argc, char* argv[])
{
	int i2 = 13;
	int* pi2 = &i2;
	cout << Greater2(22, pi2);
}

```

### 6.3 递归

```c++
#include<iostream>
#include <vector>
using namespace std;
/*
 * 编写一个递归函数，输出vector对象的内容。
 */
void print_vec(vector<int>& iv, decltype(iv.begin()) pbeg)
{
	if (pbeg == iv.end())
	{
		return;
	}

	cout << *pbeg << " ";
	print_vec(iv, ++pbeg);
}

int main(int argc, char* argv[])
{
	vector<int> iv = {1, 2, 3, 4, 5, 6};
	print_vec(iv, iv.begin());
}

```

### 6.7 函数指针

```c++
#include <iostream>
#include <vector>

/*
 * 练习6.54：编写函数的声明，令其接受两个int形参并且返回类型也是int；
 * 然后声明一个vector对象，令其元素是指向该函数的指针。
 */

int func(int a, int b);

typedef int (*func_p)(int a, int b);

int main(int argc, char* argv[])
{
	std::vector<func_p> fpv;
}
```

```c++
/*
 * 练习6.55：编写4个函数，分别对两个int值执行加、减、乘、除运算；
 * 在上一题创建的vector对象中保存指向这些函数的指针。
 */
#include <iostream>
#include <vector>

int add(int a, int b)
{
	return a + b;
}

int reduce(int a, int b)
{
	return a - b;
}

int multiply(int a, int b)
{
	return a * b;
}

int divide(int a, int b)
{
	return a / b;
}

using iiri = int(*)(int, int);

int main(int argc, char* argv[])
{
	std::vector<iiri> funcs;
	funcs.push_back(add);
	funcs.push_back(reduce);
	funcs.push_back(multiply);
	funcs.push_back(divide);

	//练习6.56：调用上述vector对象中的每个元素并输出其结果。
	for (auto item:funcs)
	{
		std::cout << item(9, 3) << std::endl;
	}
}

```

### 8.2 文件输入输出

```c++
//练习8.4：编写函数，以读模式打开一个文件，
//将其内容读入到一个string的vector中，将每一行作为一个独立的元素存于vector中。
int main(int argc, char* argv[]) {
	std::string path("C:\\workspace\\temp\\a.txt");
	vector<string> words;
	std::ifstream ifs(path);
	std::string temp_string;
	while (getline(ifs, temp_string)) {
        
		words.push_back(UTF8ToGB(temp_string.c_str()));
	}
	for (auto item : words) cout << item << endl;
}
//练习8.5：重写上面的程序，将每个单词作为一个独立的元素进行存储。
	//while (ifs>> temp_string) {...
```

```c++
//最简单的写入文件
int main(int argc, char* argv[]) {
  ofstream out("C:\\workspace\\temp\\b.txt");
  out << "测试写入ofstream";
}
```

### 10.1 泛型算法

```c++
/*
 * 练习10.1：头文件algorithm中定义了一个名为count的函数，
 * 它类似find，接受一对迭代器和一个值作为参数。
 * count返回给定值在序列中出现的次数。
 * 编写程序，读取int序列存入vector中，打印有多少个元素的值等于给定值
 */
int main(int argc, char* argv[])
{
	cout << "请输入一串数字，以空格隔开" << endl;
	string tempString;
	getline(cin, tempString);

	istringstream iss(tempString);
	list<int> iv;
	while (iss >> tempString)
	{
		iv.push_back(stoi(tempString));
	}
	cout << "请输入目标数字" << endl;
	int desiredInt;
	cin >> desiredInt;
	int countResult = count(iv.begin(), iv.end(), desiredInt);
	cout << desiredInt << " 在给定的序列中出现了：" << countResult << " 次"
		<< endl;
}

```

```c++
/*
 * 练习10.9：实现你自己的elimDups。测试你的程序，
 * 分别在读取输入后、调用unique后以及调用erase后打印vector的内容。
 */
#include <vector>
#include <algorithm>
#include <fstream>
#include <iostream>
#include <sstream>

using namespace std;

void 生成随机分数();
vector<int> 读取分数并返回();
template <typename T>
void 打印向量(vector<T> iv);

int main()
{
	//生成随机分数();
	auto r = 读取分数并返回();
	打印向量(r);
	cout << endl << endl;
	sort(r.begin(), r.end());
	auto pe = unique(r.begin(), r.end());
	打印向量(r);
	r.erase(pe, r.end());
	cout << endl << endl;
	打印向量(r);
}

template <typename T>
void 打印向量(vector<T> iv)
{
	for (auto item : iv)
	{
		cout << item << "  ";
	}
}

vector<int> 读取分数并返回()
{
	ifstream ifs("C:\\workspace\\temps\\outputScores.txt");
	stringstream ss;
	ss << ifs.rdbuf();
	string tempString = ss.str();
	vector<int> iv;
	istringstream iss(tempString);
	while (iss >> tempString)
	{
		iv.push_back(stoi(tempString));
	}
	return iv;
}


void 生成随机分数()
{
	srand((unsigned int)time(NULL));
	std::ostringstream oss;
	for (int i = 0; i < 60; ++i)
	{
		int n = rand() % 50 + 51;
		oss << n << " ";
	}
	std::ofstream ofs("C:\\workspace\\temps\\outputScores.txt");
	ofs << oss.str();
}

```

### 10.4 iostream迭代器

```c++
#include <fstream>
#include <iostream>
#include <sstream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

int main(int argc, char* argv[])
{
	//从文件创建输入文件流
	ifstream ifs(R"(E:\workspace\temps\a.txt)");
	//从文件流中创建迭代器，eof没提供构造函数参数，相当于尾后迭代器。
	istream_iterator<int> in_iter(ifs), eof;
	//从迭代器创建vector
	vector<int> iv(in_iter, eof);
	//用泛型算法和lambda表达式迭代vector
	for_each(iv.begin(), iv.end(),
	         [](int n) { cout << n << "      "; });
}
```

### 12.1 动态内存与智能指针

```c++

/*
 * 练习12.6：编写函数，返回一个动态分配的int的vector。
 * 将此vector传递给另一个函数，这个函数读取标准输入，将读入的值保存在vector元素中。
 * 再将vector传递给另一个函数，打印读入的值。记得在恰当的时刻delete vector。
 */
#include <vector>
#include <sstream>

vector<int>* factory_iv()
{
	auto p = new vector<int>;
	return p;
}

void read_from_cin_and_save_to_vector(vector<int>* iv)
{
	string temp_string;
	getline(cin, temp_string);

	istringstream iss(temp_string);

	while (iss >> temp_string)
	{
		iv->push_back(stoi(temp_string));
	}
}

void read_from_iv_and_print(vector<int>* iv)
{
	for (auto item : *iv)
	{
		printf("%d \n", item);
	}
	delete iv;
}

int main(int argc, char* argv[])
{
	auto p = factory_iv();
	read_from_cin_and_save_to_vector(p);
	read_from_iv_and_print(p);
}
```

```c++

/*
 * 练习12.7：重做上一题，这次使用shared_ptr而不是内置指针。
 */
#include <vector>
#include <sstream>

shared_ptr<vector<int>> factory_iv()
{
	auto sp = make_shared<vector<int>>();
	return sp;
}

void read_from_cin_and_save_to_vector(shared_ptr<vector<int>> iv)
{
	string temp_string;
	getline(cin, temp_string);

	istringstream iss(temp_string);

	while (iss >> temp_string)
	{
		iv->push_back(stoi(temp_string));
	}
}

void read_from_iv_and_print(shared_ptr<vector<int>> iv)
{
	for (auto item : *iv)
	{
		printf("%d \n", item);
	}
}

int main(int argc, char* argv[])
{
	auto p = factory_iv();
	read_from_cin_and_save_to_vector(p);
	read_from_iv_and_print(p);
}

```

### 13.1 拷贝构造函数

```c++
//练习13.5：给定下面的类框架，编写一个拷贝构造函数，拷贝所有成员。你的构造函数应该动态分配一个新的string（参见12.1.2节，第407页），并将对象拷贝到ps指向的位置，而不是ps本身的位置

#include <iostream>
using namespace std;

class HasPtr {
public:
    HasPtr(const string &s = string()) : ps(new string(s)), i(0) {}
    HasPtr(const HasPtr &org) : i(org.i) { ps = new string(*(org.ps)); }
    ~HasPtr() { delete ps; }
    string &GetS() { return *ps; }
    void SetS(const string &org) {
        delete ps;
        ps = new string(org);
    }

private:
    string *ps;
    int i;
};

int main(int argc, char *argv[]) {
    HasPtr hp("haha");
    cout << hp.GetS() << endl;

    HasPtr hp2(hp);
    cout << hp2.GetS() << endl;

    hp.SetS("new string inner");
    cout << hp.GetS() << endl;
    cout << hp2.GetS() << endl;
}

```

### 13.2 拷贝赋值运算符的安全性

我们定义的拷贝赋值运算符要是安全的，即使我们将自身赋值给只身，也要合理，如下代码运行时会报错：

```c++
#include <iostream>
using namespace std;

class HasPtr {
public:
    HasPtr(const string &s = string()) : ps(new string(s)), i(0) {}
    HasPtr(const HasPtr &org) : i(org.i) { ps = new string(*(org.ps)); }
    HasPtr &operator=(const HasPtr &org) {
        i = org.i;
        delete ps;
        ps = new string(*org.ps);
        //        auto tempPtr = new string(*org.ps);
        //        delete ps;
        //        ps = tempPtr;
        return *this;
    }
    ~HasPtr() { delete ps; }
    string &GetS() { return *ps; }
    void SetS(const string &org) {
        delete ps;
        ps = new string(org);
    }

private:
    string *ps;
    int i;
};

int main(int argc, char *argv[]) {
    HasPtr hp("haha");
    cout << hp.GetS() << endl;

    HasPtr hp2(hp);
    cout << hp2.GetS() << endl;

    hp.SetS("new string inner");
    cout << hp.GetS() << endl;
    cout << hp2.GetS() << endl;

    hp2 = hp2;
    cout << hp2.GetS() << endl;
    hp = hp;
    cout << hp.GetS() << endl;
}

```

需要定义一个临时指针先指向新创建的堆空间：

```c++

    HasPtr &operator=(const HasPtr &org) {
        i = org.i;
        auto tempPtr = new string(*org.ps);
        delete ps;
        ps = tempPtr;
        return *this;
    }
  
```


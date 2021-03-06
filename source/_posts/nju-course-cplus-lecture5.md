---
title: nju-course-cplus-lecture5
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:53:37
tags:
- nju
- course
- cplus
categories:
- [course, nju, cplus]
---
# 重点

## 9. 输入/输出

输入、输出具体的实现会音操作系统和计算机硬件的不同而有所不同，与平台相关。
在C++中，一种基于字节流的操作。

分为：
1. 面向控制台的I/O  iostream - istream, ostream
2. 面向文件的I/O  fstream, ifstream, ofstream
3. 面向字符串变量的I/O  strstream, istrstream, ostrstream

在c++中，ios_base基类，ios派生。 I/O类都是用类模板来实现的

> 因为输入输出通常会设计到外设，即经常考虑把程序所需要运行的数据输出到外设和从外设输入。
> C++STL中的输入输出被C++编译程序所接受。但是，因为具体平台相关性，还可以采用其他方式进行输入输出。例如：Visual C++提供的MFC基础类中包含了具有输入、输出功能的类。 
> 但是，指定注意的一点就是以非C++标准库方式进行输入输出不利于C++程序的移植

### 9.1 面向控制台的I/O
#### 9.1.1 基于函数库的控制台输入、输出

C语言标准库的功能，C++保留了这些功能
- 输出
putchar(int ch);

int puts(const char *p); 

> 什么是标准输入平台？这里应该就是控制台输入输出的意思。

printf() // 返回输出 字符个数或返回负数
注意printf("")这里如果是%s, 不接受空格和回车的，如果需要空格%[0-9a-zA-Z ]这样显式指定。
> 记得字符又被分为了好几种，比如打印字符，什么字符之类。

- printf常用格式控制字符及其含义

| 控制字符 | 类型 | 输出格式 |
|--- |----|---|
| %c | int |字符 |
| %d | int | 有符号十进制整数 |
| %u | unsigned int | 无符号十进制整数 |
| %x, %X| unsigned int | 无符号16进制 |
| %o | unsigned int | 无符号8进制 |
| %s | char* | 字符串 |
| %e,%E| double | 科学计数法的形式 |
| %f | double| 一般形式 |
| %% | | 字符%| 
- 输入
// 操作失败返回EOF
int getchar();

// 成功返回p，否则null
char *gets(char *p);

> 平台怎么处理空格和回车？是否会纳入考虑
scanf() // 返回实际输入并保存的数据个数或返回EOF
> %5f, 好像是指定位数来着？？测试

#### 9.1.2 基于类库的控制台I/O
C++的改变，用类来做。
问题：用库函数scanf和printf需要实现基本数据类型数据的输入和输出，实际操作的是格式串。
printf,scanf等于说是多参数，即输入的参数个数时可变的。这样冲某种意义上破坏了C++强类型的特点
> C++强类型有什么特点？
> 从某种意义上是不是有安全的概念？
另外，scanf和printf还有一个特点就是不能对用户自定义，只能操作基础的数据类型
。
而I/O类库提供了更为方便和安全的输入、输出操作，并且这些操作可以很容易地扩充到用户自定义类型的数据。

##### 1). 预定义的控制台对象
在I/O类库中预定义了4个I/O对象，cin,cout,cerr以及clog，利用这些对象可以直接进行控制台的输入输出。
cin->istream.  cout,cerr,clog->ostream
cout敌营这计算机系统的用于输出程序正常运行结果的标准输出设备，而cerr和clog则对应着计算即系统的用于输出程序错误信息的设备，通常都对应着显式器，但是cerr和clog不受输出重定向的影响。
> linux的输出命令的感觉吗？
> 重定向，也就是原本默认的标准输入，从新定义输出到文件。不受是不可操作的意思吗？？？？
并且cerr不对输出信息进行缓存，因此它有较快的响应效果。


###### 输出

对于指针的输出有一个特例，即输出指向字符串的指针时，不输出字符串的首地址，，而是输出字符串。

进一步，为了对输出格式进行进一步的控制，可以通过输出一些操纵符来实现。
例如：
```
#include<iostream>
#include<iomanip>
using namespace std;
int x =10;
cout<<hex<<x<<endl; // 以十六进制输出x的值，然后换行。
```
其中，对于浮点数float,double和long double,当输出格式为ios::scientific或ios::fixed时，精度设置操纵符setprecision用于设置小数点后的位数。
当什么没有时，用于设置浮点数有效数字的个数。

!!! 只对浮点数有效
> setiosflags(ios::scientific) // 设置输出格式
> resetiosflags(ios::scientific) // 取消输出格式

| 操作符| 含义|
| ---|---|
| endl | 输出换行符，并执行flush操作 |
| flush | 使输出缓存的内容立即输出？？？|
| dec | 十进制输出 |
| oct | 八 |
|hex | 16 |
|setprecision(int n)| 设置浮点数的精度 |
| setiosflags(long flage) |  设置/取消输出格式，flags的取值可以是:ios:scientific(指数形式), ios:fixed(小数形式) |

> 注意: 设置一遍后均有效，然后应该是就近原则吧！
> 初始状态下，输出格式为自动方式，输出精度为6位有效数字。

> 很好奇cout是怎么解析<<符的？按照空格回车然后进行分隔吗？还是怎么的？

> 还存在其他类型吗？这样只对double类型进行了处理，一般来说输出应该不需要其他情况的输出了吧，对！
> 一般只有两种类型，double, str 还有其他需要控制输出的吗？？？

> 突然想起，输出扩展需要的有比如输出的数据对齐指定？待安排
###### 输入
任何基本数据类型都可以通过cin对象和抽取操作符>>来进行输入。
在输入每个数据之间用空白符分开，最后输入一个回车符。另外也可以用一些操作控制符来控制输入的行为。
> 对于输入来说，感觉上也就字符串数据需要进行控制了吧，setw()用于指定输入字符的最大个数。

> 对，还需要清空输出吧？感觉上
cin >> setw(10) >> str; // 把输入的前9个字符和一个'\0'放入str中

!!!!果然不出所料，果然对于输入流来说，当输入流没有用完时是接着用输入流的

get(), getline(*p, cout,delim) 
read()
cin.get(ch);
read();

> 感觉这些输入地方太容易出错了，我之前就是这里老出错。不要有只取多少位数据的情况。

特别地，cin的输入操作是无法读入空白符的，这时才可用上面的成员函数实现空白符的读入。
> 所以此时一般扩展用getline

cin的输入操作是带输入缓存的，只有当用户输入“回车”时，输入的数据才会放入程序的变量中。
需要注意的是，基于类库的输入、输出操作中可能遇到问题（如没有正确输入数据等）。 因此，在操作后可通过下面的函数来判断操作是否成功。
bool ios::fail(); //true表示操作失败
怎么用？ 
> (cin>>ch2)返回的不是基本类型int,bool
> cin.fail() 那什么操作失败呢？
> 比如int, 输入字符串

#### 9.1.3 抽取、插入操作符>>和<<的重载。

```
class A{
    int x,y;
    friend ostream& operator <<(ostream& out, const A &a);
public:
    ...
}

ostream& operator <<(ostream& out, const A &a)
```

// 从某种意义上理解，ostream是stream的子类，所以可行。这里是类为啥又可以？

一般来说，只做输出。

有几种方法，如何在继承下使有效？
1. 如果在基类定义，而没有在派生类定义，派生类只能输出一部分值。
2. 如果在派生类定义，不能解决间接调用的情况
特别是用基类的指针，不知道会动态绑定到哪种情况的时候，考虑使用封装。
即在每个类里面重载display()函数，在<<中只需要调用display()函数即可。

### 9.2 面向文件的输入，输出

程序运行结果有时需要永久地保存起来，以供其他程序或本程序下一次运行时使用:程序运行所需要的数据也常常要从其他程序或本程序上衣词运行所保存的数据中获得。用于永久保存数据的设备称为外部存储器，如磁盘、磁带、光盘等。

#### 9.2.1 文件概述
在外部存储器中保存数据的方式通常由两种：文件和数据库。
本程序设计教程只介绍一文件方式永久性地保存数据，数据库由其他专门的课程介绍。在C++中，把文件看成由一系列字节构成的字节串，称为流式文件，对文件中数据的操作通常是逐个字节顺序进行。在对文件数据进行读写前，首先要打开文件，打开文件的目的是：把程序内部一个表示文件的变量与外部的一个具体文件关系起来，并创建内存缓冲区，每个打开的文件都有一个隐式的读写位置指针，它指向文件的当前读写位置。在进行读写操作时，每读入，写出一个字节，文件位置指针会自动往后移动一个字节的位置。
文件读写完毕后，通常需要关闭文件，其目的是把暂存在内存缓冲区中的内容写入文件，并归还打开文件是申请的内存资源。
在文件中，数据的存储方式有两种，文本方式和二进制方式。
文本方式一般用于存储具有行结构的文字数据，如源程序或纯文本格式的数据等。二进制方式一般用于存储无显式结构的数据，数据的格式由应用程序来解释，如目标代码程序以及二进制数据等。
这两种方式的主要区别是，文本文件中只包含可显示字符和有限的几个控制字符（如\r,\n,\t）;而二进制文件可以包含任意的没有显式含义的二进制文件。为了进一步区分两种存储方式，我们用一个例子来说明。
对于一个还只能输123457，可以用来种方式保存到文件中：
1. 文本方式：依次把1,2,3,4,5,6,7的ASCII玛写入文件（共7个字节）
2. 二进制方式：把整数123457的计算机内部表示（如补码）分解为字节写入文件（如果整数内部为32位，则为4个字节）。
> 以二进制方式组织的文件不利于文件在不同计算机平台上使用。
> 因为不同计算机平台的内部表示不一样
> 关于文件的压缩的思路估计也有这样的一份。

#### 9.2.2 基于函数库的文件I/O
C++从C语言标准库中保留下来的输入、输出函数库包含了对文件进行输入、输出操作的函数。
`#include<cstdio>`
##### 1) 文件的输出操作
- 打开文件
打开外部文件输出数据fopen
`FILE *fopen(const char *filename, const char *mode); // 打开文件`
mode表示打开方式：
w:打开一个外部文件用于写操作。如果外部文件已经存在，则首先把它的内容清除；否则先创建该外部文件。
a:打开一个外部文件用于添加（从文件末尾）操作。如果不存在，则首先创建文件
另外，在打开方式的后面还可以加上b,指出以二进制方式打开文件。默认打开方式为文本方式。对一文本方式打开的文件，当输出字符为\n时，在某些平台（如DOS和Windows平台）将自动转换为'\r''\n'两个字符写入外部文件
一般来说，以文本方式组织的文件要用文本方式打开；以二进制方式组织的文件要用二进制方式打开。
以w方式打开文件时，文件位置指针指向文件的头；以a方式打开文件时，文件位置指针指向文件的尾。
文件打开成功后，fopen将返回一个费控的"FILE*"指针，该指针指向与打开文件有关的一些信息（如文件的内存缓冲区等），它将被之后的文件输出操作函数使用。
> 就是实际上内存是按块组织的，这个指针会指向一些节点。当不断输出文件时，需要进行改变指针以扩大文件写的内容，还有更改好多相关的东西。
如果文件打开失败，则fopen返回空指针NULL.
- 输出操作

文件打开成功后就可以往文件中写入数据了。往文件中写入数据的函数主要有以下几个。
```
// 输出一个字符，输出成功时返回输出的字符
int fputc(int c, FILE *stream);

// 输出一个字符串，输出成功时返回一个非负整数。
int fputs(const char *string, FILE*stream);

// 输出基本类型数据，返回输出的字符数
int fprintf(FILE *stream, const char *format[,argument]...)
// 看来这里我搞错了，

// 按字节输出数据。参数size为字节块的尺寸；cout为字节块的个数。返回实际输出的字节块的个数
size_t fwrite(const void *buffer, size_t size, size_t count, FILE *stream);// size_t:unsigned int;
```
> 感觉操作系统，最难的一块就在于要计算好待数据内容的大小。
> 数据存储也是按字节方式的，所以fwrite也是二进制方式，对!

在上面的函数中，都有一个FILE*类型的指针参数，该指针参数是fopen成功打开文件后返回的。
前三个函数主要以文本方式输出数据，而第4个函数则是以二进制方式。

- 关闭文件 `fclose(FILE *stream);`

- 作业
从键盘键入一批学生的成绩信息并把他们以文本格式存入外部文件:scores.txt。
// 注意这里为什么是字符串输入。因为这些字符都是可显式字符，所以可以显式指定，就在文件中看见这些内容

> 扩展版本：使用C++中的类
> 请务必注意循环输入的判断条件
> 还有一些系统目录是没有权限，怎么说吧，除非知道相对目录很清楚的情况外，都用绝对目录来做吧

##### 2) 文件的输入操作
1. 打开文件
打开外部文件输入数据要用下面的函数来实现
`FILE *fp = fopen(const char *filename, char *mode)`;
filename是要打开的外部文件名；mode是打开方式。
它可以是'r', 表示打开一个外部文件用于读操作，这时外部文件必须存在，否则打开文件失败。
另外，可以在后面加上b，表示以二进制方式打开文件，默认打开方式为文本方式，在进行输入操作时，外部文件中连续两个字符\t和\n(Window)自动转换为一个字符\n. 读入字符0x1A(Ctrl+Z)时表示文件结束。
文件打开成功后，fopen将返回一个非空的"FILE *"类型的指针，该指针用于今后的文件输入操作函数。文件打开失败后，fopen返回空指针
文件打开成功后，文件位置指针指向文件头。
2. 输入操作
```
// 输入一个字符，然后返回字符的编码
fgetc(FILE *stream);
> 哇，这个操作完全可以用来做循环条件啊，牛逼

// 输入一个字符串，函数正常结束时返回string的值，否则返回NULL。
char *fgets(char *string, int n, FILE *stream)

// 输入基本类型的数据，返回值表示读入并存储的数据个数
fscanf()

// 按字节输入数据。
size_t fread(const void *buffer, size_t size, size_t count, FILE *stream);

// 判断文件结束。当文件位置指针在文件末尾时，继续进行读操作会使得feof返回非零(true)
int feof(FILE *stream);
```

当从文件中读取数据时，必须知道文件的存储格式，包括数据的类型和存储方式等。
> fscanf和fprintf(文本文件), fread和fwrite(二进制文件)对应
3. 关闭文件 fclose(FILE *stream)

- 小练习
读取上一个中的数据，计算每个学生的平均成绩；统计男生人数。

##### 3) 文件的输入、输出操作

> 在上面注意到一个点，输入和输出都涉及到文件打开方式，那么文件打开方式是否与实际情况相符合了呢？

以w和a方式打开的文件只能对其进行输出操作；以r打开的文件只能进行输入操作。

mode扩展：
- r+ 读写，文件必须存在
- w+ 读写。 文件不存在会创建一个空文件，否则情况一寸照的文件
> 前面这两种不过是+号更多了一种功能，跟本身功能关系很大的
- a+ 打开一个外部文件用于读、添加操作。如果文件不存在，则首先创建一个空文件。以这种方式打开的文件，输出操作总是在文件尾进行。

另外，在这些后面可以加上t,b, 文本，二进制。对！已经测试过！"w+b"

##### 4) 随机输入/输出
一般情况下，文件的读写操作都是顺序进行的，即在进行输入输出操作时，必须按顺序读入，这样降低了文件访问的小。
每个打开的文件都有一个位置指针，指向当前读写文字，每读入或写出一个字符，文件的位置指针都会自动往后移动一个位置。
`fseek(FILE *stream, long offset,int origin)`
其中，origin指出参考位置，它可以是SEEK_CUR, SEEK_END, SEEK_SET;offset是移动的字节偏移量，正值往后移动，负值往前移动。
fseek返回值为0表示移动成功，否则表示移动失败。
当前位置可以通过`ftell(FILE *stream)`
> ? 是返回的基于开头的字节数吗？？？
> 需要练习
**练习**
读入第二个学生的信息，把该学生的专业修改为COMPUTER。
```
#include<cstdio>
using namespace std;
...// 省略了Sex,Date,Major及Student的定义
int main(){
    FILE *fp = fopen("d:\\students.dat", "r+b")
    if( fp==NULL) {
        printf("打开文件失败")
    }
}
Student st;
if (fseek(fp,sizeof(st),SEEK_SET)==0){ // 文件指针指向第二个学生数据
    fread(&st,sizeof(st), 1,fp); //读入第二个学生信息
    st.major=COMPUTER;
    fseek(fp,-sizeof(st),SEEK_CUR);
    fwrite(&st,sizeof(st),1,fp); //修改后的第二个学生数据写入文件
}
fclose(fp);  // 这里student 为结构体，所以不用重载，sizeof直接可以用，如果是类是否需要重载呢？
return 0;
```
#### 9.3.3 基于类库的文件I/O
前面展示了C的部分，下面展示使用I/O类库进行外部文件的输入、输出。
`#incldue<iostream>,  #include<fstream>`
##### 1) 输出
首先创建一个ofstream类(是ostream的派生对象)，使之与某个外部文静建立联系：
1. 直接方式 `ofstream outfile(文件名，打开方式)`
2. 间接方式 `ofstream outfile; outfile.open()`
打开方式ios::out 同w, ios::app同a
打开方式还可以是上面的值与ios::binary按位或|的结果。默认为文本方式。
打开文件是否成功判断
`if(!out_file) 失败;   out_file.fail(), !out_file.is_open()`
文件成功打开后，可以使用插入操作符“<<”,或ofstream的一些成员函数来进行输出操作.
```
out_file<<x;
out_file.write((char *)&x, sizeof(x));// 以二进制方式输出数据。
out_file.close();
```
**练习**
用I/O类库来实现9-1的程序功能。

> 值得注意的是，对ostream和istream重载的插入操作符也能是ofstream和ifstream的对象。

##### 2) 输入
同输出， 打开方式ios::in，同r。
`in_file.read((char*)&x,sizeof(x)); //以二进制形式读入文件`
判断文件是否结束
`ios::eof(); // 返回非0表示上一次读操作中遇到了文件末尾`

**练习**
##### 3) 输入/输出与随机存取
```
istream& istream::seekg(<位置>) // 指定绝对位置
istream& istream::seekg(<位置>, <参照位置>) // 指定相对位置
streampos istream::tellg(); // 获取指针位置

ostream& ostream::seekg(<位置>) // 指定绝对位置
ostream& ostream::seekg(<位置>, <参照位置>) // 指定相对位置
streampos ostream::tellg(); // 获取指针位置

参照位置ios::beg, ios::cur, ios::end
```

### 9.4 面向字符串变量的输入/输出
有时，程序中有些数据并不直接输出到标准输出设备或文件，而是需要保存在程序中的某个字符变量中；程序中有些数据有时也不直接从标准输入设备或文件中获得，而是需要从程序中的某个字符变量中获得。这时，可以采用C++标准库的基于字符串变量的输入输出功能。
基于字符串变量的输入/输出功能的主要是sscanf和sprintf
```
int sprintf(char *buffer, const char *format);
int sscanf(char *buffer, const char *format);
```
与文件输入/输出不同，这里的输入源和输出目标不是文件，而是内存中一个区域:buffer. 比如说把int型变量转换为一个字符串存入字符数组a中.

对于基于I/O类库的字符串变量输入/输出操作，首先需要创建类istream,ostrstream或strstream的一个对象。
> 在新的标准中被istringstream, ostringstream和stringstream（头文件sstream）代替

// 输出
```
#include<iostream>
#include<strstream>
using namespace std;

ostrstream str_buf; // 默认构造采用可扩充的内部缓冲
或
char buf[1000];
ostrstream str_buf(buf,100);

int x,y;
str_buf<<x<<y<<endl;  // 通过该途径使得x和y获得了值
char *p = str_buf.str(); // 可获取str_buf中字符串缓存的首地址
```

// 输入
```
#include<iostream>
#include<strstream>
using namespace std;
char buf[100];
... // 通过某种途径在buf中存放了一些字符串
istrstream str_buf(buf);
或
istrstream str_buf(buf,100);
// 如果str_buf对象的构造没有给出长度，则认为它的缓存中的内容以'\0'结束
```

另外，也可使用抽取操作符或者其他操作符。
> 用处：就是用一个东西来进行收集的感觉。然后可以用，对是这样的感觉。

### 9.5 其它点

> 怎么查看C++的版本？

> Linux输出平台参数一般是怎么设置的？看起来有什么快捷的地方？有没有一本好书？

> 一般对于继承C中的输入，输出元素，返回结果如果为非负数表示正常，具体有什么含义？返回EOF表示出问题。其他有什么函数类似？

> 观察到一个点，如果正常结束的话是Process finished with exit code 0; 非正常结束返回的是一个有问题的数

> wostream 是干什么用的？

> ostream <CharT, class_traits>有什么用？
为什么不能进行ostream a; 使用
```
template <typename charT, type traints=char_traits<charT>
class basic_string;
class basci_istram

// traits 特征，性状。
basic_string<char> string;
basic_string<TCHAR> string;
// 这里TCHAR可以自己指定
```
- https://stackoverflow.com/questions/5319770/what-is-the-point-of-stl-character-traits

- http://www.cplusplus.com/reference/string/char_traits/

解释
官方给的定义是：
Character traits:
Character traits classes specify character properties and provide specific semantics for certain operations on characters and sequences of characters.
换而言之，这里的意思就是对字符串这个操作特别放宽了要求，允许你过滤或者定义特殊化的字符串。
其实，平时我们都是需要什么就进行什么过滤，而这里的感觉就是给你了机会，让你将其定义为类型，然后操作起来更简便。
这里是最初开始设计的内容，后面都是在这基础上的延伸。

> 模式匹配问题，记得刷leetcode,感觉上就有这样类似的问题。即如何进行模式匹配，实际上就是涉及到DFA和NFA，有限自动机和无限自动机。然后可以发现有限自动机方式，是按照回溯的方法解析的，所以会超级耗费内存。这样一想也解释通了。

> 路径名问题，在Windows中为\, /.有两种写法\\,或者/(linux). 其实主要原因在于编译器如果进行解释吧！

## 异常处理
异常概述
C++的异常处理机制
程序调试

- 程序的错误通常包括：
    - 语法错误：程序的书写不符合语言的语法规则。这类错误可由编译程序发现。例如：
        - 使用了未定义或未声明的标识符
        - 左右括号不匹配
    - 逻辑错误：程序设计部当造成程序没有完成预期的功能。这类错误可通过对程序进行静态分析和动态测试发现。
        - 把两个数相加写成了相乘
        - 排序功能未能正确排序
    ```
    bool strlonger(char *str1,char *str2){
        return strlen(str1)-strlen(str2)>0;
    }   
    strlonger("abc","1234") //?
    因为strlen()返回的是unsigned int, -1>0这个被认为是真。 两个unsigned int运算结果不会进行自动转换。
    修改 (int)(strlen(str1)-strlen(str2))>0
    ```
    - 运行异常：程序设计对程序运行环境考虑不周而造成的程序运行错误。
        - 对于x/y操作，y输入了零
        - 由内存空间不足导致的访问空指针：int *p=new int; *p=10;
        - 输入数据的数量超过存放它们的数组的大小导致数组下标越界
        - 多任务环境可能导致的文件操作错误
        - 给一个采用二分法查找的函数提供了一个未排序的数组
        - 数据超出了其类型所允许的范围（溢出）

语法错误，编辑器会编译不通过.
逻辑错误：环境正常，结果不正确。
运行异常：环境正常，结果正确。环境不正常，结果不正确

在程序运行环境正常的情况下，导致运行异常的错误是不会出现的。
程序异常错误往往是由于程序设计者对程序运行环境的一些特殊情况考虑不足所造成的。

导致程序运行异常的情况是可以预料的，但它是无法避免的。

为了保证程序的鲁棒性，必须在程序中对可能的异常进行预见性处理。
> 比如在输入的输入对输入的各种情况进行排查，原本的一个例子就是输入文件，然后文件路径不存在。

### 处理异常的策略

> 思考：有时间异常不是错误，所以没有必要当场处理；而且，有可能当场有重要的事情，那么这样做的话其实是非常草率的一件事情。还有用exit或abort, 这种每次都一下子退出了，完全不知道出现了什么问题，对程序员来说是一种特别懵逼的状态。

就地处理, 在发现错误的地方处理异常。
异地处理，在其他地方（非异常发现地）处理异常。

异常的就地处理

常见做法是调用C++标准库中的函数exit或abort
- abort立即终止程序的执行，不做任何的善后处理工作
- exit在终止程序的运行前，会做关闭被程序打开的文件、调用全局对象和static存储类的局部对象的析构函数等工作。
注意：不要在对象类的析构函数中调用exit，否则该析构函数不能调用。

不管是abort还是exit，都"not user-friendly"

异常的异地处理。

发现异常时，在发现地（如在被调用的函数中）有时不知道如何处理这个异常，或者不能很好地处理这个异常，要由程序的其它地方（如函数的调用者）来处理。
例如，前面的函数f中打开文件失败，这时可以由调用者重新提供一个文件来解决。

### 如何实现异常的异地处理？
1. 通过函数的返回值，或指针、引用类型的参数，或全局变量把异常情况通知函数的调用者，由调用者处理。在计算机网络中很多情况都是这样处理的。
> 该途径的不足：
> 通过函数的返回值返回异常情况会导致正常返回值和异常返回值交叉在一起。
> 通过指针，引用类型的参数返回异常情况，需要引入额外的参数，给设计带来负担
> 通过全局变量返回异常情况会导致使用者忽视这个全局变量的问题，不知道它的存在。
> 程序的可读性差！程序的正常处理与异常处理混杂在一起
2. 通过语言提供的结构化异常处理机制进行处理
C++异常处理机制
把有可能遭遇异常的一系列操作（语句或函数调用）构成一个try语句块
如果try语句块中的某个操作在执行中发现了异常，则通过执行一个throw语句抛出一个异常对象，之后的操作不再进行。
抛置的异常对象将由能够处理这个异常的地方通过catch语句块来捕获并处理之。
```
void f(char *filename){
    ifstream file(filename);
    if (file.fail())  throw filename(); // 产生异常
    int x;
    cin>>x;
    ...
    return 0;
}

 int main(){
     char str[100];
     ...
     try{
         f(str); // 启动异常处理机制
         // 如果在函数f中抛掷了char*类型的异常，则程序转到try后面的catch(char*str)处理
     } catch(char *fn){ // 捕获异常
         ...// 处理异常
     }
     ...// 正常情况
 }
```
> 一种直观的感觉，等于说try语句这里把所有的异常进行了传递，然后所有子的对象都会返回回来。如果从运行的角度来思考也很正常。然后，还有一个点，类也可以实现，即基类的指针一直指回基类？对吗？

### try语句

try语句块的作用是启动异常处理机制。其格式为：
try{
    语句序列
}

上述的语句序列中可以有函数调用。

### throw语句
throw语句用于在发现异常情况时产生异常对象。
throw <表达式>
表达式为任意类型的C++表达式。
例如：
```
void f(char *filename){
    ifstream file(filename);
    if (file.fail())  throw filename(); // 产生异常
    // 注意这里异常的对象为一个字符串指针，这里是与catch中的类型相互对应的。
    int x;
    cin>>x;
    ...
    return 0;
}
```
执行throw后，接在其后的语句将不再继续执行，而是转向异常处理（由某个catch语句给出）
> 这里也好理解，当出错的时候肯定是这样处理了啊
一个问题？
这样做属于异常的异地处理了。怎么说来看的话是封装了一层。即实际上也是用的全局变量，但是不是在返回一层时调用，可能是返回多层时进行调用的。细想一下，震惊了哈哈哈。
挺好，用本身自带的变量不好，重新开辟一个浪费，如果是全局变量则被忽视，所以倒不如由底层处理好了再说，真棒！

### catch语句
catch语句块用于捕获throw产生的异常对象并处理相应的异常。
```
catch(<类型>[<变量>])
{
    ...
}
```
类型用于指出捕获何种异常对象，它与异常对象的类型匹配规则如若函数重载。
变量用于存储异常对象，它可以缺省，缺省时表明catch语句块只关心异常对象的类型，而不考虑具体的异常对象。
> 这里想到了实际上做的时候确实只关心了异常对象的类型，而且是将类型分为了唯一的积累，这样处理起来就方便了很多。

catch语句块要紧接在某个try语句的后面。

一个try语句块可以跟多个catch语句块，用于捕获不同类型的异常对象并进行处理。
> 这里做的是类型其实也还可以，但是很是不行，因为处理的准则应该是对应于哪种错误，关于类型直接反馈不到处理上。

关于try、throw和catch几点注意
- 在try语句块的语句序列执行中如果没有抛出异常对象，则其后的catch语句不执行，而是继续执行try语句块之后的非catch语句。
- 在try语句块的语句序列执行中如果抛掷了异常对象
    - 该try语句块之后有能够捕获该异常对象的catch语句，则执行这个catch语句中的语句序列，然后继续执行这个catch语句之后的非catch语句
    - 该try语句块之后没有能够捕获该异常对象的catch语句，则由函数调用链的上一层函数中的try语句的相应catch来捕获。
    > 这就是用到的向上层抛出异常。

- 如果抛除异常对象的throw语句不是由程序的某个try语句块中的语句序列调用的，则抛掷的异常不会被程序中的catch捕获
> 也就是不在try语句中，所以就要注意了，必须代码在检测范围中才能被检测啊，这是很浅显的道理了啊！

- 异常处理的嵌套
try语句是可以嵌套的
- 当在内层的try语句的执行中产生了异常，则首先在内层try语句块之后的catch语句中找，否则想上处理
- 如果抛掷的异常对象没有，则调用系统的terminate函数进行标准化异常处理。默认情况下，terminate函数将会去调用abort函数。

还有一个很重大的问题，有时候终止程序需要很大的代价，那么是否可以做到程序不终止，一直到获得正确的数据为止呢？
> ?????上层一直while循环 

## 程序调试

- 一个处于开发阶段的程序可能会含有很多错误（逻辑或异常）。
- 发现和找出这些错误的一种手段是在程序中的某些地方家伙少年宫一些输出语句，在程序运行时把一些调试信息（如变量的值）输出到显示器。

> 啊啊，我现在都一直用的是这种笨办法

这种方式存在以下问题：
- 调试者必须对输出的值做一定的分析才能知道程序是否有错。
- 在开发结束后，去掉调试信息有时是一件很繁琐的工作
> 难道是用 Debug, 或者调试工具？哇撒

？断言
实际上，在调试程序时输出程序中的某些变量或表达式的值，其目的是为了确认程序输出的这些值与预期的值是否相符。
上述的目的可以让程序设计者在程序的一些关键或容易出错的点上插入一些相关的断言来表达
    - 断言是一个逻辑表达式，它描述了程序执行到断言处应该满足的条件
    - 如果条件满足则程序继续执行下去，否则程序执行异常终止。
在程序测试阶段，断言可以帮助测试这发现程序的逻辑错误，也可以用来发现一些异常错误。

> 语法错误首先X,然后的话异常只能按自己是否能够预知下得到。然后的断言最大的好处真的在可以发现程序的逻辑错误啊，真牛逼！其实这等同于输出的，就是每次debug这样真的好麻烦好吗？而且没有意思，哇咔咔，这样真有趣！

> 哇，想起了java的测试工具类，python的测试工具包，终于浮出水面了吗？

宏assert

C++标准库提供的一个宏assert(cassert, assert.h),可以实现断言。
宏assert要求一个关系，逻辑表达式作为其参数，当assert执行时，
    - 如果表达式的值为false，则会显示出相应的表达式、它所在的源文件名以及所在的行号等诊断信息，然后调用库函数abort终止
    - 当表达式的值为true时，程序继续执行

例如，下面的宏assert调用表示程序执行到该宏调用处变量x的值应等于1：
assert(x==1)
当程序执行到该调用处，如果x的值不等于1，则它会显示下面的信息并终止：
Assertion failed:x=1,fileXXX,line YYY
可以返现异常错误，如
```
int divide(int x,int y){
    assert(y==0);
    return x/y;
}
```

这是真的是超级好用的一个东西。
宏assert的实现是通过条件编译预处理命令来实现的：
cassert,
```
#ifdef NDEBUG
#define assert(exp) ((void)0)
#else
#define assert(exp) ((exp)?(void)0:<输出诊断信息并调用库函数abort>)
#endif
```
> 解释，也就是说宏assert只有在宏名NDEBUG没有定义时才有效，这时，程序一般处于开发、测试阶段。程序开发结束提交时，应该让宏名NDEBUG有定义。然后重新编译程序，这样，assert就不再有效了。

> 在思考，#DEBUG有啥用？


### 一点有意思的记录
char *p="adad";
*p指向的是字符串。

```
strlen(p); // 传入指针就行，能想清楚。
// 这本身就是其功能规定的，所以这里也不多说什么。觉得就这样吧，是对的。
cout<<p;  //正常输出
printf("%s",p); // 输出
```

// 等下，我好想这里搞错了什么，或者说把一些事情。也就是说这里指针是没错的。因为这里是统一处理输入输出的函数。

// 另一个方面讲

太棒了，刚说自己想多了，结果才发现这里有说明的。

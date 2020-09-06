# C learning

#### 基础
`size_t`是标准C库中定义的，应为unsigned int，在64位系统中为long unsigned int  
`extern` 便于使用外部变量  
`va_list` 可变参数  
	原型： void va_start(va_list arg_ptr, prev_param); 功能：以固定参数的地址为起点确定变参的内存起始地址，获取第一个参数的首地址  
`aux` 一般是指针  
`vsnprintf` 函数功能：将可变参数格式化输出到一个字符数组。  
`memset` 初始化内存  
`memcpy` 是memory copy的缩写，意为内存复制  
```C
void *memset(void *s, int c, unsigned long n);  
```
函数的功能是：将指针变量 s 所指向的前 n 字节的内存单元用一个“整数” c 替换，注意 c 是 int 型。s 是 void* 型的指针变量，所以它可以为任何类型的数据进行初始化。  
```C
int atoi(const char *nptr)
```
用法：将字符串里的数字字符转化为整形数。返回整形值。  
`atof()`函数：将字符串转换为double(双精度浮点数)  
`strtol()`函数：将字符串转换成long(长整型数)  
`trtod` 功能是将字符串转换成浮点数  
`ispunct()` 函数用来检测一个字符是否为标点符号或特殊字符  
`log()` 以e为底  
`size_t`是标准C库中定义的，在64位系统中为long long unsigned int，非64位系统中为long unsigned int。在32位系统中size_t是4字节的，而在64位系统中，size_t是8字节的  
`ssize_t`这个数据类型用来表示可以被执行读写操作的数据块的大小.它和size_t类似,但必需是signed.意即：它表示的是signed size_t类型的。  
`memcmp`函数的原型为 int memcmp(const void * str1, const void * str2, size_t n));其功能是把存储区 str1 和存储区 str2 的前 n 个字节进行比较   
`inline` 用来定义一个类的内联函数

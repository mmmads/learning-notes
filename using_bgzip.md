# BGZIP
[参考链接](https://www.htslib.org/doc/bgzip.html)

## BGZF File
### BGZF FORMAT
BGZF文件是一系列串联的BGZF块，每个块在压缩之前或之后都不大于64Kb。每个BGZF块被压缩成一个gzip文件，在header区包含extra field。gzip文件的header包括一个有`BC`作为标识符的extra sub-field、压缩块的长度（包括所有header）。  
每个BGZF块包含一个标准的gzip文件header，并具有以下符合标准的扩展。  
1. hearder中的F.EXTRA位用来表示extra field的存在
2. BGZF使用的extra field使用了2个subfield ID值66和67（ASCII 'BC'）
3. BGZF extra field payload的长度（gzip规范中的field LEN）为2（2 bytes of payload）
4. BGZF extra field 的payload是 16-bit unsigned integer in little endian format。这个整数给出了包含BGZF块的大小减去1。  
`payload 指有效负载`  
随机读取限制了每个BGZF块的大小不超过2^16 bytes,所以ISIZE(INPUT SIZE-- length of uncompressed data)被限制在[0,65536]。 BSIZE(total Block SIZE - 1)可以表示BGZF范围在[1,65536]之间的block sizes。由于压缩BSIZE通常会比ISIZE小。  

#### 随机读取
BGZF文件通过BAM file index支持随机读取。BAM file index采用了virtual file offsets into BGZF file。每个virtual file offset都是一个unsigned  64-bit  integer,定义为coffset<<16|uoffset，其中coffset是到BGZF块开头的BGZF文件的无符号字节偏移(unsigned byte offset),uoffset是被那个BGZF块所表示的未压缩数据流中的无符号字节偏移。

#### EOF
EOF由28个16进制bytes表示。

### GZI FORMAT  
索引格式是一个二进制文件，列出了BGZF文件压缩和未压缩的offsets值。每个压缩后的offset指向一个BGZF块的开端，未压缩的offset对应未压缩文件流的相应位置。  
所有值都以 little-endian 64-bit unsigned integers值存储。  
uint64_t： typedef unsigned long int  
FILE CONTENTS:  
` uint64_t number_entries `
followed by number_entries pairs of:  
` uint64_t compressed_offset  
  uint64_t uncompressed_offset`

### 大端序与小端序
* Big Endian（大端序）:最高字节在地址最低位，最低字节在地址最高位，依次排列
* Little Endian（小端序）:最低字节在最低位，最高字节在最高位，反序排列
对于机器来说，Little Endian更有利于机器的运算，内存地址由低位到高位，两个数相加运算，直接在高位添加进位数，不必移动内存地址。

#### 解释
[参考链接1](https://zhidao.baidu.com/question/433068663.html)  

0x12345678 是16进制表示方法 转换成二进制  
0001 0010 0011 0100 0101 0110 0111 1000  
   1    2    3    4    5    6    7    8  
数据在内存中存放遵顼高低原则，即高字节内容存放在高地址，低字节内容存放在低地址(小端)  
所以0x12345678在内存中存放如下（假设第一个地址为0x1001）：  
| 地址   | 数据 | Memory Offset |
| ------ | ----| ----- |
| 0x1001 | 78的二进制 | 0 |
| 0x1002 | 56的二进制 | 1 |
| 0x1003 | 34的二进制 | 2 |
| 0x1004 | 12的二进制 | 3 |  

通常这组数据的第一个内存单元的地址（也就是78对应的内存单元编号表示整体的内存地址）。读取时由于int长度为4字节，char为1字节。读取int值为12345678，读取char值只能读取到78

## BAM FORMAT  
BAM是以BGZF格式压缩的，BAM文件中的所有multi-byte numbers都是little-endian。  
Sequence被编码成4-bit值。质量值用ASCII码表示
### Indexing BAM
如果想要对BAM文件进行索引，BAM必须先按照参考序列ID排序然后再按最左边的左边排序
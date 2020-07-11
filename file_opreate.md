# 文件相关

```python
def iterate(infile):
	## infile can be any iterator over lines
	## iterate over pileup formatted file 
```

## 目录
* [Pileup Format](#PileupFormat)
* [生成器与迭代器](#生成器与迭代器)
* [读取大文件](#读取大文件)
* [**gzip**](#gzip)


## Pileup Format  
[参考链接](http://samtools.sourceforge.net/pileup.shtml "samtools文档")  

### Pileup File每一行对应参考基因组一个位置，一共有六列，列之间用Tab分开，每一列表示内容如下：
1. 染色体
2. `1-based` coordinate 1-碱基坐标
3. 参考基因组对应位置碱基
4. reads中覆盖此位置的碱基数量
5. read bases  reads中对应碱基
6. base qualities  碱基质量

对于read base列：
* `'.'`表示read base比对到正链(forward)参考碱基
* `','`表示read base比对到反链(reverse)参考碱基
* `'ACGTN'`表示比对到正链(forward)上的错配
* `'acgtn'`表示比对到反链(reverse)上的错配
* `'>'`表示 正链(forward)上的Reference skip (由于CIGAR 'N')
* `'<'`表示 反链(reverse)上的Reference skip (由于CIGAR 'N')
* `'*'`表示 正链(forward)上的Reference deletion (由于CIGAR 'D')
* `'*/#'`表示 反链(reverse)上的Reference deletion (由于CIGAR 'D')
* `'^'`表示read segment的开始 read segment是CIGAR'N/S/H'操作符处理的read序列 ^后面的ASCII字符减去33后表示比对质量
* `'$'`表示read segment的结束
* 模式`\+[0-9]+[ACGTNacgtn]+`表示这一参考基因位置与下一位置之间有插入 格式为长度+序列
* 模式`-[0-9]+[ACGTNacgtn]+`表示这一参考基因位置与下一位置之间有删除 格式为长度+序列

example of 2bp insertion on 3 reads:  
	
	seq2 156 A 11  .$......+2AG.+2AG.+2AGGG    <975;:<<<<<  

example of 4 bp deletions from ref (support by 2 resds):  

	seq3 200 A 20 ,,,,,..,.-4CACC.-4CACC....,.,,.^~. ==<<<<<<<<<<<::<;2<<  


## 生成器与迭代器

### 迭代器
	所有可以使用for...in...语法的都叫做一个迭代器，如列表、字符串、文件。但是把所有值都存储到内存了。所以不适合于大量数据。
```python
mylist = [x*x for x in range(3)]
```

### 生成器
	生成器是可迭代的，但是`只可以读取它一次`，并不是把所有值存储在内存中，而是实时地生成数据。
```python
mygenerator = (x*x for x in range(3))
```

### yield
	是一个类似return的关键字，但yield返回的是个生成器

## 读取大文件
[参考链接](https://blog.csdn.net/u012733099/article/details/84286626)

### Preliminary
	Python文件对象提供了三个读方法：read() readlline() readlines。每种方法可以接受一个变量以限制每次读取的数据量，但它们通常不使用变量。
* read()每次读取整个文件，将文件内容放到一个字符串变量中。 常用read(size)读取size字节 的内容
- readline()每次读取一行的内容
* readlines()一次读取所有内容并按行返回list

### read in chunks
	将大文件分割成若干小文件处理，处理完每个小文件后释放该部分内存

``` python
def read_in_chunks(filePath, chunk_size=1024*1024):
"""
Lazy function (generator) to read a file piece by piece.
Default chunk size: 1M
You can set your own chunk size
"""
    file_object = open(filePath)
    while True:
        chunk_data = file_object.read(chunk_size) # .read(size) 读取size个字节
        if not chunk_data:
            break
        yield chunk_data
if __name__ == "__main__":
    filePath = './path/filename'
    for chunk in read_in_chunks(filePath):
        process(chunk) # <do something with chunk>
```

### using with open()
   用with语句打开和关闭文件，`for line in file`会将文件对象file视为一个迭代器，会自动采用缓冲IO和内存管理。

``` python
    #If the file is line based
    with open(...) as f:
        for line in f:
            process(line) # <do something with line>
```

### yield
	通过yield 不需要编写读文件的迭代类

``` python
def read_file(fpath): 
   BLOCK_SIZE = 1024 
   with open(fpath, 'rb') as f: 
       while True: 
           block = f.read(BLOCK_SIZE) 
           if block: 
               yield block 
           else: 
               return
```


## gzip
[参考链接](https://zhuanlan.zhihu.com/p/62302150)  

gzip使用zlib对数据进行压缩，zlib使用的是DEFLATE算法，压缩过的数据被称为Deflate Stream，其压缩过程如下：
  1. 用引用来替换重复出现的序列。如果一个序列在32K的范围内重复出现了若干次，那么从第二次开始，压缩过的数据中将会使用23bit来引用第一次出现的位置(15bit偏移量+8bit长度)
  2. 对前面一段流中（称为块）出现的数据进行哈夫曼编码，使用更少的bit来表示更常出现的数据，从而降低块的整体大小

所以压缩之后 对于不同的原始数据，替换重复序列后的结果和哈夫曼编码的结果会不同，因此没有通用的方法能够仅仅根据原始数据中的位置来换算出压缩后的位置。  
Deflate Stream是比特流，原始数据中某个位置在压缩以后可能对应到非字节边界上，这是无法用sekk直接来定位的。

### 暴力算法
zlib官方提供的 gzseek()/gzrewind()方法。从文件头部开始一边解压缩一边计数，直到到达指定位置。

### 空间换时间：zran.c
利用额外的空间储存建立的索引。在seek时先在索引中查到大致的数据范围，然后解压缩指定位置的数据，再进行seek。
### indexed_gzip(Python)
允许用户持久化索引，将生成的索引存储到磁盘上，下次打开压缩文件的时候可以指定索引，避免了重复创建索引。不过索引的数据结构仍然是线性表

### BGZF(Block GZip Format)
BGZF把`每个gzip块的长度信息写入到Extra Field`标识符为0x4243（小端）的自定义额外字段中。可以通过bgzip命令行转换现有的gz文件或创建新的压缩文件。
	$ bgzip uncompressed.txt                      # 压缩  

一个gzip块的结构如下：
```
+===================+=============================+=================+
| Field(s)          | Description                 | Type            |
+===================+=============================+=================+
| GenericHeaders    | gzip header fields          | uint8_t[8]      |
+-------------------+-----------------------------+-----------------+
| * XLEN            | eXtra field length          | uint16_t        |
+-------------------+-----------------------------+-----------------+
| Extra Subfields                             (total size = XLEN)   |
|  +----------------+-----------------------------+---------------+ |
|  | SubfieldHeader | Extra field header fields   | uint8_t[32]   | |
|  | * BSIZE        | total Block SIZE minus 1    | uint16_t      | |
|  +----------------+-----------------------------+---------------+ |
|                               ...                                 |
+-------------------+-----------------------------+-----------------+
| CDATA             | Compressed DATA             | uint8_t         |
|                   | by zlib::deflate()          | [BSIZE-XLEN-19] |
+-------------------+-----------------------------+-----------------+
| * ISIZE           | Input SIZE                  | uint32_t        |
|                   | (size of uncompressed data) |                 |
+-------------------+-----------------------------+-----------------+
```

如果想要随机读取原始数据中位置为P的数据，步骤如下：
1. 打开BGZF文件
2. 读取gzip块的`XLEN`字段
3. 读取XLEN个字节，在这些字节中搜索BGZF的特定字段，读取`BSIZE`
4. 跳过`BSIZE - LEN - 19`个字节（即压缩数据段）后，读取ISIZE
5. 如果指定位置P大于`ISIZE`，说明当前块不包含位置P的数据，则将P减去ISIZE后继续执行步骤2
6. 如果指定位置P小于`ISIZE`，则解压缩当前块的CDATA以后得到的数据位置P就是读取的起点
该方法用seek()跳过了解压缩数据的步骤，但仍是顺序搜索。

### 更高效的索引结果
可以通过构建原始数据偏移量-压缩数据偏移量/长度的树来实现将查找时间复杂度降低到O(log(n))。树的构建过程如下：
1. 打开BGZF文件
2. 创建名字为`INPUT_OFFSET`的变量，保存原始数据偏移量，初始值均为0
3. 读取当前块的`XLEN`、`BSIZE`和`ISIZE`,在跳过CDATA字段前，将当前文件指针保存为`DATA_OFFSET`，将`BSIZE-LEN-19`保存为`DATA_LENGTH`
4. 以`INPUT_OFFSET`为Key，`DATA_OFFSET`和`DATA_LENGTH`为Value保存在索引树中
5. 将`INPUT_OFFSET`加上`ISIZE`，重复步骤3，直至读取完全部块为止。

创建索引需要顺序扫描一次文件，后续则可以通过索引树来查找。在需要多次随机读取的情况下，比每次顺序定位所需块的方法要更高效。


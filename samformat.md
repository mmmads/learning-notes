# SAM Format

## SAM format
[参考链接](http://samtools.github.io/hts-specs/SAMv1.pdf)

BAM以BGZF格式压缩。BAM中的多字节数都是低位字节序（little-endian），与机器字节序无关。下图中描述了SAM格式，其中括号中的值是相应信息不可用时的默认值(default)。带下划线的大写字母表示SAM格式中对应字段。(如POS MAPQ)
![SAM格式](https://pics.images.ac.cn/image/5f0ea961c7c25.html)

Sequence以4-bit值编码。邻近的碱基被打包成同一字节。当l_seq是奇数时，最后一个byte的后四位是没有被定义的，但是建议将他们写成零。不区分大小写的碱基编码`=ACMGRSVTWYHKDBN`被映射到[0,15]，所有其它字符都映射到'N'


## BAM files的BAI index格式
![BAI格式](https://pics.images.ac.cn/image/5f0eb15eb4d66.html)


#### C语言运算符
`<<` 二进制左移运算符 将一个运算对象的各二进制位全部左移若干位（左边的二进制位丢弃，右边补0）
`>>` 二进制右移运算符 将一个运算对象的各二进制位全部右移若干位（正数左补0 负数左补1，右边丢弃）

1<<? 表示1左移?位   表示2^?（2的? 次方 ）
`uint8_t` 实际上是一个char 直接输出会输出其对应的字符
``` C
typedef unsigned char		uint8_t;
```
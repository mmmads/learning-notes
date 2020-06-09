# pysam pileup()

## SAM文件对 pileup信息的解释
```
Information on match, mismatch, indel, strand, mapping
quality and start and end of a read are all encoded at the
read base column. At this column, a dot stands for a match
to the reference base on the forward strand, a comma for a
match on the reverse strand, a '>' or '<' for a reference
skip, 'ACGTN' for a mismatch on the forward strand and
'acgtn' for a mismatch on the reverse strand. A pattern
'\+[0-9]+[ACGTNacgtn]+' indicates there is an insertion
between this reference position and the next reference
position. The length of the insertion is given by the
integer in the pattern, followed by the inserted
sequence. Similarly, a pattern '-[0-9]+[ACGTNacgtn]+'
represents a deletion from the reference. The deleted bases
will be presented as '*' in the following lines. Also at
the read base column, a symbol '^' marks the start of a
read. The ASCII of the character following '^' minus 33
gives the mapping quality. A symbol '$' marks the end of a
read segment
```
```

[参考链接](https://blog.csdn.net/u013553061/article/details/53293302)
 > 在pileup格式中(没有-u或者-g参数)，每一行代表基因组的位置，由染色体名、1个碱基坐标、参考碱基、reads覆盖该位点的数量、reads的碱基、碱基质量和比对质量。有关匹配、错配、插入缺失、链、比对质量和一条reads的开始结束位置都被编码到reads碱基列。在此列上，“.”表示与正链上的参考碱基匹配，“,”表示与负链上的参考碱基匹配，“>”和“<”表示跳过参考基因，“ACGTN”表示正链上的错配，“acgtn”表示负链上的错配。此模式“\\+[0-9]+[ACGTNacgtn]+”表示在此位点至下一个位点之间与参考基因组对应位点相比，多了一段插入碱基，插入长度由模式中的整数表示。与此类似，“\\-[0-9]+[ACGTNacgtn]+”表示缺失，缺失的碱基使用“*”表示。同时，“^”表示reads的开始，“$”表示reads的结束。在“^”后的字符的ASCII码值减去33表示比对质量值。

```

## pysam pileup 的使用
```python
import pysam  
samfile = pysam.AlignmentFile("ex1.bam", "rb" )  
for pileupcolumn in samfile.pileup("chr1", 100, 120):  
    print ("\ncoverage at base %s = %s" %  
           (pileupcolumn.pos, pileupcolumn.n))  
    for pileupread in pileupcolumn.pileups:  
        if not pileupread.is_del and not pileupread.is_refskip:  
            # query position is None if is_del or is_refskip is set.  
            print ('\tbase in read %s = %s' %
                  (pileupread.alignment.query_name,
                   pileupread.alignment.query_sequence[pileupread.query_position]))

samfile.close()
```

```
samfile.pileup("chr1", 100, 120)可以获取基因组一段位置的pileup情况
该region中的每个position(单个碱基)处都有一个pileupcolumn

````
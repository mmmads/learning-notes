## 命令行

`$*`: 以一个单字符串显示所有向脚本传递的参数


## SNAP
1. increase read length   larger seed
2. increase server memories
3. novelalgorithm

### read alignment
SNAP比对的话 必须要有和seed长度相同、序列完全相同的 才能够进行匹配  
所以large seed会降低找到perfect match的概率  
short seed sizes会导致multiple places  
too many hits 直接drop掉  
seed size应该小于1/4 read size  一般100bp的read seed长度为23(error rate)

SNAP对reference长度有限制 因为存储位置的是32-bit的值
Key size：4-8 大小只影响hash表的构建

MaxHits: 影响quality performance  
maxSeeds: For paired-end alignment maxSeeds also has a large effect on performance, 
but with a smaller effect on accuracy than maxHits  

maxHits: 2000-good quality/300 default/10 quick pass

paired-end 在计算任何分数之前，它会将种子命中集合彼此相交，以找到要得分的候选者
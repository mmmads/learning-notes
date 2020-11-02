# Nanopore sequencing 

当使用拥有相似DNA修饰的同一个物种或者足够近缘物种的原始DNA（未经过扩增）进行训练后的basecalling，原始DNA的准确性更高。  
识别的准确性则由read精确度（单条read对参考数据的相似度）或一致性序列准确度（重叠reads构建的一致性序列对基因组同样位点的序列相似度）来评估，一致性准确度通过增加读取深度来提高。】

## Chiron 
raw signal -> nucleotide sequence  
1. raw data divided into segments corresponding to signals obtained from a k-mer
2. translate segment signals into k-mers  

##### DeepNano——BRNN  
basic statistics:  mean signal, standard deviation, length  
use ↑ to predict k-mer  
##### ONT nanonet Albacore——dynamic programming  
k-mers k-1 bases overlap
##### BasecRAWller——RNN
a pair of RNN  
1. 预测分段边界的概率
2. 将离散时间转换为base sequence  

##### Albacore 2.1  无分段的
非开源  
##### Chiron——DNN  
CNN RNN CTC decoder  
直接对原始信号数据建模 无需分割  
可以自己训练神经网络  
2 sets of layers:  
1. convolutional layers 区分原始输入信号中的局部模式
2. recurrent layers 将模式整合到 basecall 概率中 
3. top of NN: CTC decoder 根据base概率提供final DNA sequence   
sliding windows: 300 raw signals -> 10-20 base pairs sequences(slices)  
overlapping slices -> stack together -> consensus sequence 


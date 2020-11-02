# RNA-seq
[参考链接1](https://zhuanlan.zhihu.com/p/88923135) 
[参考链接2](https://www.jianshu.com/p/1b2a9117254d) 

## 三种主要测序技术 

1. Illumina workflow(short read seq)
建库之后，单独的cDNA分子在流动槽中构建测序簇，使用3’阻断的荧光标记的核苷酸进行边合成边测序。在每一轮测序中，高速摄像机拍照捕获当前激发的荧光，来判断当前是哪个核苷酸合成进来，测序长度在50-500 bp  
2. The Pacific Biosciences workflow
建库之后，每个分子与固定在纳米孔底部的聚合酶结合。然后是边合成边测序，测序长度可以高达50 kb  
3. The Oxford Nanopore workflow
建库后，将单个分子加载到流动槽中，在接头连接过程中加上的分子马达会与生物纳米孔结合。马达蛋白控制RNA链穿过生物纳米孔，引起电流变化，从而推测出经过的碱基序列，生成的测序reads大小为1-10 kb。  

### PacBio
边合成边测序。实时记录荧光信号。转化为单碱基信息，获得具有单碱基分辨率的高精度序列。PacBio测序依赖DNA聚合酶活性。
PacBio测序有两种模式，一种是CLR模式，另一种是CCS模式。对于长插入片段文库，产生的序列的一般少于2 passes的（pass即环绕测序的次数），得到的reads称为Continuous Long Reads（CLR）， 当文库插入片段相对较短时，测序后会产生多个passes，来源于同一个孔的多条reads通过一致性校正，得到一个准确度较高的reads，称为Circular Consensus Sequencing（CCS）Reads

### Nanopore
基于电信号检测原理。当DNA分子穿过纳米孔时会产生电流信号，一般以5个碱基为一组检测电流信号，对电流信号进行解码。Nanopore测序不依赖DNA聚合酶活性，理论上只要DNA分子不断开，就一直可以通过纳米孔，得到的序列读长更长，最长可达Mb级别。Nanopore下机的原始电信号文件，以.fast5结尾，包含测序的序列信息和甲基化修饰信息。经过basecalling软件（Guppy，Albacore等）可以将fast5文件转换为fq文件进行后续分析。一般根据Qscore>7对数据进行质控，通过的为pass，没有通过为fail。  
ONT则是基于电信号原理，在DNA分子穿过纳米孔时会输出电流信号的波形图，但因电流速度太快，一次产生的信号包含4-5个碱基, 就需要对信号进行碱基解码（decoding）。由于单个信号可以产生4^5=1024种组合，且噪音信号和随机数据都会干扰碱基读取，解码时容易出现delete和insert的错误。  

### 测序错误
Pacbio的CLR模式下，一般的错误率在10-15%左右，但是这种错误是随机错误，主要类型为Indel和Mismatch，但是此类错误类型及碱基类型均无偏向性，这种缺陷可通过自身纠错获得准确度高达QV50（99.999%）的序列（




图5）  
Nanopore的测序错误除Indel和Mismatch之外，主要是同聚物（homopolymer）和串联重复区域的错误（Wick et al., 2019），特别是同聚物删除（homopolymer deletion） 的错误较高（图6）。另外，有研究表明基因组中反向重复序列序列会使Nanopore的测序质量下降，得到的序列准确度受到影响（Spealman et al., 2019）。因此，基因组重复比较高的物种，使用此技术要小心了，可能在重复区域准确度不一定高，如果该区域Pacbio不能跨越的话，此技术还是比较好，毕竟有总比没有强。  
。但是Nanopore的同聚物错误使得这些错误往往出现在基因组某些特定的序列或区域，造成自身纠错和用二代数据校正无法纠正，序列错误和真实变异难以区分，影响组装基因组的准确性。  

### 优缺点
1. long-read cDNA方法能直接检测全长转录异构体,从而移除或大幅减少检测偏好，提高差异表达转录本分析的准确率。  
2. 依赖于cDNA转换的方法，在cDNA转换这一过程抹去了有关RNA碱基修饰的信息，而且也只能粗略估计多聚腺苷酸（poly（A））尾巴的长度，而direct-RNA-seq可以直接分析全长转录本异构体、度量碱基修饰（比如N6-甲基腺苷（M6A））和检测poly（A）尾巴长度。
3. short-read cDNAllumina, Ion Torrent①高通量，每次运行产生的reads数是long-read平台的100-1000倍之多；②测序偏好和错误模式研究透彻（同聚物homopolymers对于Ion Torrent来说仍然是个问题）；③可使用的方法和计算流程很多；④可用于降解了的RNA的分析样品制备过程如反转录，PCR和片段选择都会引入偏好性；转录异构体的检测和定量受限；新转录本的鉴定基于转录本拼装步骤几乎所有的RNA-seq应用都是基于short-read cDNA测序：DGE (differential gene expression), WTA (whole- transcriptome-analysis),小RNA，单细胞，空间转录组，新生转录本，翻译组，RNA结构组和RNA-蛋白质相互作用分析等等。
4. long-read cDNAPacBio, ONT①1–50kb的长reads可以检测很多全长转录本 ②用于de novo转录组分析的计算方法简化很多①低-中通量，每个run获得0.5 M-10 Million reads②样品制备过程如反转录，PCR和片段选择(部分方法需要)都会引入偏好性③不太适合降解了的RNA尤其适用于转录异构体的发现，无参转录组的de novo分析，融合转录本的发现，HL A (human leukocyte antigen)和MHC (major histocompatibility complex)等复杂转录本分析
5. Long-read RNAONT①1–50kb的长reads可以检测很多全长转录本②用于de novo转录组分析的计算方法简化很多 ③样品制备不需要反转录或PCR，降低了偏好性 ④可以检测RNA碱基修饰 ⑤单分子测序直接估计poly(A)全长①通量低，每个run仅生产0.5 M-1 Million reads②样品准备和测序过程偏好性不明确③不太适合降解了的RNA①尤其适用于转录异构体的发现，无参转录组的de novo分析，融合转录本的发现，MHC和HLA等复杂转录本分析 ②适用于检测核糖核酸修饰  
跟成熟的短读长技术平台相比，长读长测序技术的测序通量低很多，错误率更高。而长读长测序技术的主要优势即能测序更多的独立转录本全长，依赖于高质量的RNA文库。这些局限会影响那些特别依赖长读长测序实验的灵敏性和特异性。

### 相关概念
1. adapter是一段短的序列已知的核酸链，用于链接序列未知的目标测序片段。
2. barcode，也称为index，是一段很短的寡居核酸链，用于在多个样品混合测序时，标记不同的样品。
3. insert是用于测序的目标片段，因为是包括在两个adapter之间，所以被称为“插入”片段。
4. reads：就是我们测序产生的短读序列，通常一代和三代的reads读长在几千到几万bp之间，二代的相对较短，平均是几十到几百bp
5. contig：中文叫做重叠群，就是不同reads之间的overlap交叠区，拼接成的序列就是- contig
6. scaffold: 是比contig还要长的序列，获得contig之后还需要构建paired-end或者mate-pair库，从而获得一定片段的两端序列，这些序列可以确定contig的顺序关系和位置关系，最后contig按照一定顺序和方向组成scaffold，其中形成scaffold过程中还需要填补contig之间的空缺
7. N50和L50是组装reads的概念。[具体参考](https://www.jianshu.com/p/14ccd77f8af4)


# 3'-end & Poly A

PolyA长度与mRNA半衰期有关，但是与翻译效率无关。我们发现在PolyA尾的下游有广泛的尿苷化和鸟苷酸化。U尾通常是连接到短的PolyA尾(<25nt)，G尾更多连接到较长的PolyA尾上(>40nt)。

# Long-read sequencing (LRS)
[Long-Read Sequencing – A Powerful Tool in Viral Transcriptome Research]   

* cDNA  complementary DNA (cDNA)
* DRS  Direct RNA sequencing (DRS) 
* PABs  poly(A)-binding proteins 
* rt  readthrough (rt)
* RT  (reverse-transcription)  
* RTs  reverse transcriptases 逆转录酶
* SRS  Short-read sequencing (SRS)  
* SMRT  Single-molecule real-time (SMRT)  
* SLR-Seq  synthetic long-read sequencing (SLR-Seq)   
* TSS  (transcription start site)
* TES  (transcription end site)
* TOs  (Transcriptional Overlaps)


目前有两种LRS计数：PacBio和ONT。与LRS方法相比，SRS产生更高质量、深度覆盖的数据集，具有成本效益，并且具有更低的单位基数错误率。  
复杂的基因组区域，包括GC含量高的序列，以及重复序列，不能用SRS有效地解析。较短的阅读长度也使得确定外显子连通性和鉴定转录本异构体(如多剪接RNA以及转录起始位点(TSS)和转录结束位点(TES)变体）的计算变得困难或不可能。此外，交替的多顺反子和TOs(Transcriptional Overlaps)，特别是嵌入RNA(ERNA)分子之间的多顺反子和TOs，给SRS平台带来了挑战。LRS技术可以克服这些挑战，因为它能够提供关于转录本的完整重叠群信息。  
ONT 1D sequencing has low accuracy (approximately 85%). The 1D2 technology improves this accuracy by ligating an adapter to the end of the reads, which increases the probability that a strand and its complementary strand pass through a pore consecutively. The base-calling algorithm creates a consensus of the two reads, and it has an average quality of over 95%. ONT can also identify the nucleotide modifications of RNA molecules by native RNA sequencing. The advantages of nanopore sequencing over the PacBio platform are the longer read length, the higher throughput, and the lower costs  
ONT的dRNA测序的优点是没有RT(reverse-transcription)和PCR artifacts.  
LRS可以区分不同的转录子亚型，而SRS可以在单个细胞水平上表征基因表达    
目前的LRS技术产生的测序覆盖率低于SRS方法；因此，小基因组生物是这些第三代测序技术的理想对象。LRS平台能够确定全长转录本；因此，它们在识别长的、多基因和多剪接的转录本以及TSS和TES亚型方面具有优势。使用这些技术学习ToS也更容易。总而言之，LRS方法使每个被检查的病毒物种的转录本数量成倍增加。考虑到许多病毒转录本的注释很差，LRS可能会大大增加我们对大多数转录本复杂的病毒基因表达的理解。RNA直接测序对于检测具有简单转录体的病毒(例如，(+)ssRNA病毒)也很有用，因为它可以检测到其他方法无法检测到的RNA修饰。ToS的极其复杂的网络暗示了相邻基因和远端基因之间通过转录装置之间的物理相互作用而产生的复杂的相互作用。此外，nroRNAs和NRO样转录本的发现增加了复制和转录机制之间相互作用的可能性，以便以合作的方式调节基因表达和DNA合成(参见突出问题)。


## Nanopore Sequencing
测序直接结果是电流。使用ONT神经网络算法将该连续的离子电流序列转换成核苷酸序列。其中ONT神经网络是用已知RNA分子训练的。 

但是在长的同源多聚物序列延伸上也不能很好地表现    
### ONT的优势
1. 单分子技术 
2. 无扩增 避免了扩增伪影
3. 测序是天然的样本 可以直接测量RNA的附加特征
4. 可以直接将转录本亚型对应到单分子 不需要生物信息学后处理 可以针对同型异构体polyA进行测量

### ONT cDNA测序的优势
与本地RNA测序相比，每个文库制备产生的数据大约多10倍，并且由于扩增，可以从微量RNA作为输入进行测序实验  
使用唯一分子标识符(UMI)的未来cDNA应用将使从每个分子获取多个PolyA尾部测量成为可能，这将提高特定于异构体的Poly(A)尾部测量的保真度

### (ONT)flip-flop model
Flip-flop model base-calling screens the raw signal by comparing probabilities to either stay in the same nucleotide state or change to a new state.  
通过比较保持在相同核苷酸状态或改变到新状态的概率来筛选原始信号  原始数据仅通过平均超过两个采样点来读取，而不是在标准模型基调用中平均超过五个采样点。这些改进已被证明导致了更高质量的碱基调用，更重要的是提高了碱基调用相对于均聚物序列的保真度  

## poly(A) tail estimate software 

#### Nanopolish-polyA
将原始信号分割成为与mRNA和polyA尾对应的区域，然后用区域长度估计PolyA尾的长度。  
其对于polyA尾mediain值估计较准确，误差在几个碱基范围内；但是对于单个read测量有很大的误差范围。  

#### EpiNano
另一种最近开发的基于纳米孔DRS的M6A图谱方法，称为EpiNano，也有上下文特定的限制(Liu等人，2019年)；体外转录RNA的纳米孔DRS被用来训练支持向量机模型以检测M6A(Liu等人，2019年)。然而，EpiNano目前不能在含有多于一个腺苷的k-mer中调用m6A，因此我们在拟南芥中发现的92%的m6A位点用这种方法是无法检测到的

#### FLAM-SEQ(PacBio)
全长聚(A)和mRNA测序(FLAM-SEQ)，这是一种快速和简单的方法，用于高质量的整个mRNA测序。我们报道了一种结合单分子测序的互补DNA文库制备方法来进行FLAM-SEQ。使用人类细胞系、脑器官和秀丽线虫，我们发现FLAM-SEQ为每个样本提供了数千个不同基因的高质量全长mRNA序列。我们发现，3‘非翻译区长度与PolyA尾长相关，同一基因的不同多聚腺苷酸位点和不同启动子与不同的尾长相关，尾部含有大量的胞嘧啶。  
FLAM-SEQ可以测得包括每个测序分子的转录起始位点(TSS)、剪接模式、3‘末端和Poly(A)尾部。

#### PAL-seq

#### TAIL-seq
虽然PALseq能够准确地估计广泛的尾长，但Tail-Seq被限制在 约230个核苷酸(Nt)，但可以确定Poly(A)尾巴的末端修饰，这被发现可以控制mRNA的稳定性。大多数mRNA的稳态尾长远远短于250nt，大多数mRNA的中位数在50到100nt之间，这证实了之前对特定mRNA或与其他mrna的观察到的结果。  

#### Hire-PAT
high-resolution poly(A) tail assays 


我们发现m6A位点的分布与典型的Poly(A)信号AAUAAA(以及具有一个错配位置的相关六聚体)紧密重叠，表明AAUAAA信号本身中的腺苷可以甲基化。  
m6A几乎只存在于mRNA 3‘UTR中(在稳态分析中)。因此，m6A的作用可能主要通过mRNA选择性多聚腺苷基化、稳定性、可译性和形成特异的蛋白质复合物来介导  

m6A在3'UTR区域富集  
全基因组聚(A)分析一直很困难，因为存在碱基转换和去相错误28。最近实施的对Illumina策略的修改解决了这些限制27、28；但不能解决远端关系，例如剪接和聚(A)长度之间的关系  

问题：但到目前为止，还没有方法确定内源mRNA的全长序列，包括它们的Poly(A)尾巴。此外，虽然非A核苷酸可以被掺入到聚(A)尾巴中，但也没有方法对它们进行精确测序。  
PolyA尾巴，它在核输出、翻译起始和翻转中起着关键作用。  


#### PAIso−seq
来自Illumina或PacBio平台的ISO-SEQ数据都缺乏PolyA信息。  TAILseq和PAL-seq使用可选的Poly(T)长度调用算法或测序配方来计算PolyT长度，同时牺牲了调用RNA Poly(A)尾部内的非A残基的能力，除了非常3‘端之外


### polyA测序缺点
低通量 高workload 以及由于扩增带来的技术缺陷  
但从技术上讲，它们仅限于特定大小的PolyA尾巴，这取决于样本富集和测序策略。此外，这些技术中的大多数依赖于聚(A)尾区的PCR扩增，这可能导致影响聚(A)长度测量的扩增伪影以及长和短聚(A)尾之间的定量比较  
最后，也是更重要的是，这些技术只能通过比对代表RNA 3‘端的短序列来间接鉴定与聚(A)相连的转录本。因此，要将Poly(A)尾部的测量结果指定给特定的转录体亚型是具有挑战性的   
然而，目前的碱基调用者在长均聚物RNA和DNA延伸上表现不佳，导致聚(A)尾巴的长度没有得到准确的报道  
Tailfindr，这是一个R工具，它直接从ONT Fast5原始数据中估计单个读取的Poly(A)尾部长度。Tail Findr能够从RNA和DNA读取(包括包含Poly(T)延伸的DNA反向互补读取)中估计Poly(A)尾巴。TailFindr使用未经事先比对的原始数据作为输入，并基于与读取特定核苷酸移位率的归一化来估计长度。我们在一组具有定义的Poly(A)尾长的RNA和DNA分子上验证了Tailfindr的性能。TailFindr对广泛使用的以及最新的ONT基础呼叫应用(触发器模型)的输出进行操作。  
与cDNA测序方法相比，ONT原生RNA测序在数量和质量上都较低，并且依赖于大量的起始材料[500 ng聚(A)选择的RNA，牛津纳米孔技术2018a，2019年]。因此，保留全长Poly(A)尾巴的cDNA测序方法将使材料稀缺的研究成为可能，并增加PolyA(A)尾巴估计的统计能力。因此，我们的目标是将Tailfindr扩展到操作ONT DNA测序方法。  

考虑将分析数据从RNA扩展到 cDNA数据 

#### tailfindr
直接从base-called data(Nanopore数据)中 估计polyA长度

#### Nanopolish
利用alignment information for the definition of the polyA tail segment

# PolyA 长度
## NGS
TAILseq和PAL-seq使用可选的Poly(T)长度调用算法或测序配方来计算PolyT长度，同时牺牲了调用RNA Poly(A)尾部内非A残基的能力，除了非常3‘端。此外，他们需要微克水平的RNA输入，这对于罕见的活体或患者样本是不可行的。  
目前开发的PacBio第三代测序技术能够通过实时单分子测序读取均聚物。此外，测序文库中测序模板的循环使得能够对单个模板多次测序，以准确地调用共识序列READ19。因此，PacBio第三代测序平台可能是准确分析RNA Polyy(A)尾巴长度和组成的最佳选择。

## PacBio
\需要富集
### FLAM-seq 
当我们对由该RNA标准产生的文库进行测序时，我们观察到Poly(A)长度的中位数正好为50nt，离中位数10nt以内的尾巴的比例为78%  
对于每个标准，我们观察到中位尾长接近预期长度，偏移量分别为2、3、5和6nt，97、92、75和78%的读数的尾巴距离预期长度在10nt以内（但是观察到的中位数都比实际polyA长度短）

### PAIso-seq
为了减少对长聚(A)尾巴的偏向，我们还希望避免聚(T)富集步骤。  
defined poly(A) tail lengths of 10, 30, 50, 70, and 100 nt. After sequencing, we observed the mean tail length of 10, 28, 48, 67, and 97 nt

## Nanopore -- cDNA
### Nanopolish-Polya
Poly(A)尾长的中位数Nanopolish-Polya估计在几个碱基范围内是准确的，但单个读取测量有很大的误差范围。  

## Nanopore -- dRNA
我们直接使用与每个聚(A)链3‘端相关的低方差离子电流信号测量聚(A)尾长(图1B，III)。我们开发了一种计算方法(‘Nanopolish-polya’，来分割这一信号，并估计从Poly(A)尾部区域提取了多少离子电流样本。通过校正RNA分子通过孔隙的速率，Nanopolish-Polya估计了Poly(A)尾巴的长度。算法详情可在补充说明1中找到。  

### tailfindr
 预测结果(30 nt: 33; 40 nt: 41; 60 nt: 59; 100 nt: 91;150 nt: 136). However, even though the majority of sequences show the expected poly(A) tail length, the standard deviation of poly(A) tail measurements is relativelyhigh (coefficient of variation between 45% and 79%, seeTable 1).  
 但是方差比较高 

## Illumina和dRNA-seq结合
基于Illumina的短读测序实现的高读取深度和碱基调用的准确性，允许检测病毒DNA基因组转录活性区域内的单核苷酸多态性，以及对基于Nanopore的长读直接RNA测序(dRNA-seq)固有的较嘈杂的碱基调用进行纠错。  
此外，通过结合来自短读测序的高精度剪接位点连接和来自长读测序的全长异构体上下文，我们能够重新评估AdV131转录单元的剪接复杂性。  
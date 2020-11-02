# crash course statistics

## 01 what is statistics?
Proxy  
  描述性统计 更容易理解数据  
推理统计学 在不确定的情况下对数据进行决策  

## 02 mathematical thinking
Scientific notation 科学计数法  
law of truly large numbers 巨数法则  
幸存者偏差  

## 03 
average  measures of central tendency 集中趋势测量  
mean(average)  outliers异常值
median  smallest to largest  
mode 众数 bimodal 双峰 multimodal 多峰  
normal distribution: zero skew 零偏差
skewed 偏态

## 04 measures of spread 散度测量
dispersion 散布  
InterQuartile Range 四分位差  Q1 median Q3  IQR = Q3-Q1  
如果中位数更接近IQR范围中的一个端点 意味着其四分位数范围更小 意味着数量点彼此之间的距离更近  
variance  方差  整个数据分布情况  
deviation  差值  
biased -1
squared deviation = (9+9+9+9)/(5-1) = 9   
方差--可变性
standard deviation 方差的平方根  将单位变得有意义
标准差： the average amount we expect a point to differ (or deviated) from the mean  
注意极值的影响  乡下人法国v

## 05 Data Visualizations 1
data type: categorical and quantitative  
quantities: have both order and consistent spacing  

contingency table 列联表  
bar chart  条形图  
pie chart  饼图  
pictograph  象形图  
binning分箱: 使用定量型变量 把它归入h类  
histogram  直方图  
right stew  均值会偏左 有右部拖尾  

## 06 Data Visualizations 2
1. dotplot 点图  
2. stem and leaf plot 茎叶图  
3. boxplots  箱线图：box and whiskers  
Q1--MEDIAN--Q3 距离越大 数据越分散   
边缘值 minimum = Q1-1.5IQR  maximum=Q3+1.5IQR   溢出 outlier  
可以用pre-set cut-off判断数据是否异常  
4. cumulative frequency plot 积累频率图  

## 07 Data Distribution你
1. normal distribution   
mean & standard deviation  smaller closer skinnier  
2. positive skew & negative skew
右拖尾 左拖尾  
尾部倾斜更大 意味着数据分布范围更大 有更大的标准差
3. bimodal  multimodal
其实可能是多个单峰分布组合成整体
4. uniform distribution 
概率相同 

## 08 Correlation doesn't equal causation
相关性不意味着因果性  
1. bivariate data 双变量数据
2. regression coefficient 回归系数
3. correlation 相关性
4. correlation coefficient r 相关系数 [-1,1]
5. squared coorrelation r^2 平方相关系数[0,1] 可以衡量用一个数据估计另一个数据的准确性
6. spurious correlations 伪相关性

## 09 
systematic difference 系统差
1. allocation bias 分配偏差  selection bias 选择偏差
2. randomized block design 随机化区组设计
3. controls 对照组
4. palcebo effects 安慰剂效应
5. single blind study  double blind study 研究者和被研究者都不知道
6. matched-pairs experiments 非常相似的两组实验
7. repeated measures design

## 10 
1. survey 
2. non-response bias
3. voluntary response bias
4. underrepresentation
5. stratified random sampling 分层随机抽样 
6. weight 给不同的值添加不同的weight
7. cluster sampling 整群抽样  从集群上随机抽样
8. snowball sampling      population of interest

## 11
## 12 sampling and regression
## 13 probability
pareidolia 幻想性错觉  
empirical and theoretical 经验型和理论型  
independent 独立  
conditional probabilities  
## 14 Bayes  
bayesian updating  
law of large numbers 

## 15 Binomial Distribution 二项分布
Binomial Coefficient formula  
Bernoulli distribution  0/1  

## 16
Geometric probability formula 几何概率公式  
第一次成功是在你第n次尝试的时候的概率   
cumulative Geometric probability formula   add up  
mean 1/p   
the birthday paradox 生日悖论   70--99.9%两个人同一天生日  

## 17 randomness 
simulations -- 模拟 来了解未发生的事  
best guess   mean  expectation  一阶矩
variance  二阶矩
三次方 偏度 是否一边有更多的极端值 可以为负  
第四阶矩 峰度  对分布尾部厚度的测量

## 18 standardizatin
先减去均值 然后除以相应的标准差  
z score +1 高一个标准差  用于比较不同分布的  
percentile  百分位数 
标准正态分布 z分布 均值是0 标准差是1
(z-score * sd) + mean  

## 19 normal distribution

关心的是均值的分布  比较群体  
抽样分布  几乎总是正态分布 central limit theorem 中心极限定理  
样本均值分布是正态分布的假设  
样本均值分布的标准差与原标准差相关，样本量越大 越接近真实总体均值  
样本标准差=总体标准差/根号n   standard error

## 20 confidence intervals
95% ci：样本中的95%包含真实总体均值 5% 的情况下排除总体均值  
z-score：一个分布的均值和一个数据点在标准差内的距离
小样本 均值分布不总是正态的，用t分布代替z分布  
t分布：连续的单峰概率分布 根据信息多少改变形状  
一般>30 足够大 越大----越接近正态分布  
margin of error 

## 21 statistic inference
NHST 零假设显著性检验   做不同的检验  
假设没有差异或者影响  
reductio ad absurdum 归谬法  proof by contradiction 反证法  
p-value  how rare your data数据多罕见 含义是零假设为真 你观察的数据有多极端  
p-value 0.1 数据在 top 10%最极端的  
one-sided p-value 单边p值    two-sided 双边 
如果显著低于/高于均值 就可以拒绝零假设  
双向p值是样本均值极端程度的度量  
p值越小 零假设是真的 就越不容易在随即情况下得到你的样本  
p-value cutoff 边界可以设为0.05 小于0.05的值足以证明拒绝零假设  可以说是统计上显著的 statistically significant   
统计显著： 不太可能是因为偶然得到这样的数据  

## 22 p value
pvalue并不能够告诉假设检验为真/假的概率  
因为前提是 假设为真/拒绝了零假设    
alternative distribution 备择分布 拒绝零分布之后的另一个分布

## 23 
type Ⅰ errror  拒绝零假设 即使它为真  false positive
type Ⅱ errror  false negative  
trade-offf  
statistical power 统计功效  
零分布和备选分布重叠越多 越难判断  
两个分布的均值之间的距离代表了 effect size 效应量  
减少重叠：拉远/变瘦   数据越多 分布越瘦 重叠越小  

## 24 Bayes
updating belief  
bayes' factor  从数据中了解到的关于假设的信息量  比较两种假设的概率
poterior belief 
posterior odds = prior odds * likelihood ratio  

## 26 test statistic
量化事物与我们的期望或理论有多接近  
z score  
sampling distribution 抽样分布 在一定样本量下，所有可能的组均数的分布  
样本z-score = (样本均值-总体均值)/(样本方差/根号n)         n是样本数目  
z绝对值越大说明样本均值越极端  
critical value  临界值  z=1.96/-1.96  
不知道总体标准差 用t检验  
z-statistic = (X-μ)/σ  
t-statistic = (X均值-μ)/样本标准差(sd/根号n)  
t分布尾部更厚 估计总体标准差 估计不确定性增加 所以尾部更厚  
样本量大 t分布收敛到z  有总体标准差的时候用z  

test statistic : (observed data- what we expect if the null is true)/average variaton  
standard error = 根号(σ1方/N1 + σ2方/N2)  

## 27 t-test  
test statistic : (observed data- what we expect if the null is true)/average variaton   
two sample t-test (independent or unpaired t-test)   
双样本标准误差：根号(σ1方/N1 + σ2方/N2)  
单样本标准误差：方差/根号n  

## 28 自由度和效应量
degrees of freedom 自由度  effect size 效应量   
自由度可以帮助确定精度 自由度越高越接近z分布  
自由度是数据中独立信息的数量 n-1  
几乎所有数据的均值都在两个标准差之内  
effect size 观察到的变化有多大   
不显著可能是因为样本太少了  可以增大样本容量   

## 29 卡方检验   
chi-square model  
X² = ∑(obs - exp)²/exp  
chi-square Goodness of Fit test 卡方拟合优度检验  
5 -- 边界值   
test of independence 独立性测试：看一个类别的成员是否独立于另一个类别  
test of homogeneity  同质性检验：看不同的样本是否可能来自同总体  
自由度公式 (r-1)x(c-1)  
计算期望频率  

## 30 p-hacking
p值操控 指通过操纵数据或分析来人为地获取显著p值   
family wise error rate 错误发现率   
boneferroni correction 校正 使用通常的阈值除以正在进行的测试的数量  0.05/5  

## 31 reproducibility
复现性  
statistic power 统计功效  检测真实效果的能力  
 
## 32 regression 
GLM general linear model  
GML: DATA = MODEL + ERROR  	Y = MX + B  
outlier 逸出值  y-intercept  coefficient  
residual plot 残差图 想要一个均匀分布的   
F-test 量化数据拟合分布的程度   
F-statistic: (observed model - model if the null is true)/average variation  
null: slope = 0   y^ 对结果变量的预测值  y- 样本中的平均值  
total variation 总变差 SStotal= (Yi-Y-)²  
VAR(Y) = SStotal/N        如果是从一个样本中计算 往往除以N-1 来纠正偏差   
SST: 总平方和   ∑(Yi-Y-)² 获得的所有信息
SSR: 回归平方和 ∑(Y^-Y-)² 可以用创建的模型解释的信息的比例  
SSE: 误差平方和 ∑(Yi-Y^)² 剩余信息  
TSS = SSR + SSE  
F-STATISTIC: (SSR/DFR)(SSE/DFE)
SSE的自由度: 样本大小n-2  (1个用来算截距 1个用来算回归系数)  
SSR自由度: 1  用一条独立信息估计斜率  
t² = F  

## 33 ANOVA 方差分析 多组之间的差异  
ANalysis Of Variance 用分类变量预测连续变量  
平方和SST = N x Variance   Variance = SST/N  表示的是数据中变异或信息量的总数  
SST = SSM + SSE  
DATA = MODEL + ERROR   
SSM 每个模型内 数据与均值差的平方和  是模型所解释的变量  
SSE 组均值与总体均值的平方和 组间方差  
F-STATISTIC (SSM/DFM)/(SSE/DFE) = MSEmodel/MSEerror  
f越大解释的越好  
DF SSM  K-1  K为组数      SSE N-K  
Omnibus test 综合测试 检验一组数据中解释的方差是否显著大于未解释的方差                     

## 34 包含多个分组变量的方差分析 
factorial anova 多因素方差分析   
先计算SST  每个值和总体均值的平方和  
DATA = MODEL + ERROR 
F = ... = MSvariable/MSerror     称作均方和  
eta squared 相关比  
η² = SSeffect/SStotal ≈ ERROR  (0,1)  
相关的两个变量 要加入一个互动项 interaction term   
SSB 组间平方和  

## 35 
ANCOVA  协方差分析 analysis of covariance  
总变异只关注结果变量  
repeated measures anova 重复度量方差分析

## 36 supervised machine learning  
logistic regression 逻辑回归  
confusion matrix 混淆矩阵  
LDA linear discriiminant analysis 线性判别分析  
dimensionality reduction 降维  
k-nearest neighbors k近邻  
k-means clustering 
silhouette score 轮廓系数 
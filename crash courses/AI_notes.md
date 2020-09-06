# [CRASH COURSE AI] Learning Notes

## Class 2 
AI: learn from data  
Data Lable  

## Class 3 ---- Supervised Learning
* Reinforcement Learning
* Unsupervised Learning 
* Supervised Learning 

Neurons: 电信号
首先，人工神经元接受的输入与不同的权重(weight)相乘，这些权重对应于每个电信号的强度，阈值由一种叫做bias(偏置)的特殊权重表示，可以通过调整bias来提高或降低神经元的兴奋程度。然后通过一个step function 输出0/1.  
bias: 神经元活动的阈值
Learn: upgrate weights  
upgrade rule -- decision boundary  
sum < bias, 在decision boundary之下 分类

### Confusion matrix
precision & recall  
precision: how much u should trust when it says it's found something  
recall: how much ur program can find the thing u r looking for  

## Class 4 ---- Neural Networks and Deep Learning
Artificial neural network  
AlexNet   
* Input layer:神经网络接受数值数据的地方，一个输入神经元代表了一个特征  
* Hidden layer:会将获得的所有数据进行整合 判断其是否拥有某些组件  
* Output layer:对最后一个隐藏层的输出进行整合，并给出答案  

## Class 5 ---- Training Neural Networks
BACKPROPAGATION (of the error):  信息反向反馈 对更重要的神经元权值调整的更多
architecture & weights   
result: line of best fit  
loss function:  
局部最优解solution 可以随机多个起点 改变步长(learning rate)

## Class 6 ---- LAB
识别手写字母--转换成text格式  
segmentation 分割(单词中的字母)  
1. find or create a labeled dataset to train our neural network  (EMNIST)  
2. create a neural network
3. train test tweak until accurate enough  
pip3 install emnist  
MLP: Multi-layer perceptron neural network  
SKLearn: Sci Kit Learn  
```python
	from emnist import extract_training_samples
	print("imported the emnist libraries we need")

	X, y = extract_training_samples('letters')
	# preprocessing
	X = X / 255

	X_train, X_test = X[:60000], X[60000:70000]
	y_train, y_test = y[:60000], y[60000:70000]

	# 28+28 pixel
	X_train = X_train.reshape(60000,784)
	X_test = X_test.reshape(60000,784)
```
784 input neurons  
26 output  
hidden: 50 neurons  
iteration epoch   
用confusion matrix 检查哪里最容易出问题   

## Class 7 ---- Unsupervised Learning
k-means clustering  
1. predict
2. learn  

#### Representation Learning  表示学习
找到patterns  图像表示
reconstruction  
autoencoder

## Class 8 ---- Natural Language Processing
1. natural language understanding
2. natural language generation

#### Distributional semantics 分布语义
用count vector   但是要存储大量的数据  
#### 表示学习
encoder-decoder model  
#### encoder
recurrent neural network RNN
给每个单词用一个向量表示
#### decoder
给出词汇表中每个可能单词的得分 用反向传播调整参数  
如果两个词有相似的意思，模型会使它们的向量更相似

## Class 9 ---- LAB Make an AI sound like a youtuber
术语词法类型和词法标记  
词法类型(lexical type)： 一个单词  
词法标记(lexical token)：一个单词的特定实例 （可重复）  
Tokenization: 
Morphology: 词法 一个词通过变形来匹配时态  
stemming:词干提取  
1. data preprocessing
2. set up the model  
  embedding matrix and RNN  
3. train model  
4. make prediction

## Class 10 ---- Reinforcement Learning  learning by doing
学习难以解释的复杂任务  反复试验  结果好奖励高  
Credit Assignment: 决定哪些操作最有效  
Agent Enviroment  trail and error exploration
explore & exploit  

## Class 11 ---- Symbolic AI
Knowledge base 知识库  
logic: symbol relations   
inference: 推理             expert systems 专家系统  

## Class 12 ---- Robotics  
Localization planning manipulation
LiDAR  
proprioception & closed loop control

## Class 13 ---- AI Playing games
决策树 极大极小算法  
Monte Carlo Tree Search  
Monte Carlo 随机性， 比较少数可能性 然后做出选择  
Evolutionary neural network: 利用环境作为输入，比如强化学习  

## Class 14 ---- let's make an AI that destroys video games
1. build game
2. Create AI model
3. train model
GA: Genetics  

## Class 15 ---- Humans and AI working together

## Class 16 ---- How youtube knows what you should watch
Recommender systems: supervised learning & unsupervised learning  
### Algorithm: recommend youtube video  
1. content-based recommendations  -- video 
2. social recommendations  -- audience like view watch time
3. personalized recommendations -- preferences
4. collaborative filtering 结合以上三种

判断一个人对新channel是否喜欢，要找到最相似的人（无监督学习） predication  
use known information from users to predict preferences  
### problems
1. sparse data
2. cold start problem  
3. statistically likely predictions

## Class 17 ---- Recommender systems lab  
[参考链接](https://colab.research.google.com/drive/1-v9cw18wTDjaCUlECKHsQnHeisLKyG8U)

## Class 18 ---- Web Search
Search Engine  
1. gather data
2. create organized systems
3. find results 

#### WEB CRWALER
seed link web --> inverted index  
找到的是一个列表 含有关键词的网页列表 所以要对重要性进行打分  
用户的点击就对这个进行了打分 click view给了training data 
#### KNOWLEDGE BASIS  
NELL: facts: 对象关系 plays(Mozart,pinao) 使用已知对象找新的关系 通过已知关系找到新的对象  
wrote(?,book)  通过知识库找到相应的fact  
可参考语音标记系统  

## Class 19 ---- Algorithmic Bias and Fairness
BIAS & DISCRIMINATION  
1. data reflects existing biases (correlated features)
2. unbalanced classes in training data (训练集数量不够)
3. data doesn't capture the right value (complicated  hard to measure)
4. data amplified by a feedback loop 
5. malicious data attack or manipulation  


## Class 20 ---- LAB Cats vs Dogs
1. gather the data
2. build an ai model
3. select a pet
4. audit the model for bias  
which will make me happy?  
1. feature: 4 features [1]/[0]
2. MLP  sklearn 1 hidden layer 4 neurons 
3. 对数据进行判断 bias data collection       
	1 distribution 2 correlated data  

## Class 21 ---- The future of AI  
AGI artificial general intelligence  

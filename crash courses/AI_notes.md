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

## Class 6 ---- Experiment  
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

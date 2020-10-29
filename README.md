# Validator
声明：此文档由DKE编写整理, 部分信息参考于网络，仅用于交流学习目的，资源以及代码请勿用作它途。 内容如有纰漏欢迎指正
## Bson
### string
```javaScript
{"key_name":{"bsonType":"string"}}
//length restriction
{"key_name":{"bsonType":"string", "minLength":5, "maxLength":30}}
//number restriction in string
{"key_name":{"bsonType":"string", "pattern":"[1-9][0-9][0-9]"}} //"100" to "999" 
{"key_name":{"bsonType":"string", "pattern":"[a-z][A-Z][0-9]"}}
```
### Variables
```python
tf.get_variable(name, shape=None, dtype=tf.float32,
initializer=None, regularizer=None,
trainable=True, collections=None)
```
*Variables should be initialized before being used*
### Placeholders
```python
tf.placeholder(tf.float32, shape=[None, 10]
sess.run(c, feed_dict={a: [1, 2, 3]} // feed
```
## Tensor Session
### reset graph
```python
tf.reset_default_graph()
```
# 定义graph
### run session
```python
sess = tf.Session()
outs = sess.run(e)
print("outs = {}".format(outs))
sess.close()

or

with tf.Session() as sess:
outs = sess.run(e)
print("outs = {}".format(outs))
```


![](Media/image-20201028234348284.png)

## 设置input dim， output dim， hidden layer size（neuron number）

```python
n_neurons_h = ？ # number of neurons in a hidden layer
n_neurons_out = 根据target的类型和数量
n_features = 根据input data维度  # number of features
```

![](Media/image-20201028232822839.png)

Output layer: 

target看作数字， neuron = 1

target看作one-hot , neuron = class number

## 初始化placesholer（input layer & output layer

```python
# placeholder
x = tf.placeholder(tf.float32,
                    shape=(None, n_features),
                    name=”x")
y_true = tf.placeholder(tf.float32,
                    shape=(None , n_neurons),
                    name="y")
```



## 初始化  weight bias 

```python
# input layer -> hidden variables 
W1 = tf.get_variable("weights1", dtype=tf.float32,
					initializer=tf.[...]((n_features, n_neurons_h), ...)
                    )
b1 = tf.get_variable("bias1", dtype=tf.float32,
					initializer=tf.[...]((n_neurons_h), ...)
                    )
# hidden variables -> output layer
W2 = tf.get_variable("weights2", dtype=tf.float32,
					initializer=tf.[...]((n_features, n_neurons_out), ...)
                    )
b2 = tf.get_variable("bias2", dtype=tf.float32,
					initializer=tf.[...]((n_neurons_out), ...)
                    )
```

```python
initializer: tf.random_uniform_initializer(...) or tf.random_normal_initializer(...)
```

https://www.tensorflow.org/api_docs/python/tf/random_normal_initializer

## 设置active function， 计算layer output ， y_hat

```python
# hidden layer output
h = tf.nn.[active function](tf.matmul(X, W1)+ b1)


# output layer output
z = tf.matmul(h, W2) + b2
y_hat = tf.nn.[active function](z)

```

![](Media/image-20201028232958493.png)

![](Media/image-20201028232528739.png)

试试不同的activation function， 选效果最好的那个

##  计算L2 Regularisation: 

```python
l2 = tf.reduce_sum.(tf.square(hidden layer weight)) + tf.reduce_sum.(tf.square(output layer weight))
l2parm = ?
```



##  cost after Regularisation = cost + L2

```python
error = y_true - y_hat
cost_r = [cost]
```

cost: tf.reduce_mean(tf.square(error)) + l2parm*l2

## 计算gradient并更新

use GradientDescentOptimizer

```python
learning_rate = 0.1
optimizer = tf.train.GradientDescentOptimizer(learning_rate)
training_op = optimizer.minimize(cost)
```

manuelly update

```python
learning_rate = 0.1
new_W1 = W.assign(W - learning_rate * tf.gradients(cost, W1))
new_b1 = b.assign(b - learning_rate * tf.gradients(cost, b1))
...
```

有些版本中需要将learning_rate * tf.gradients()的结果转换成 arr 再相减

## Training

```python
init = tf.global_variables_initializer()
n_epochs = 100 
with tf.Session() as sess: 
    init.run() 
    for epoch in range(n_epochs): 
        training...
```

use GradientDescentOptimizer

```python
sess.run(training_op, feed_dict={ X:training_X, y_true: training_y })
```

manuelly update

```python
sess.run([new_W1, new_b1, ...], feed_dict={ X:training_X, y_true: training_y })
```

## Mini-Batch

```python
import numpy as np
n_records,_ = training_X.shape
batch_size = ...

with tf.Session() as sess:
init.run()
for epoch in range(n_epochs):
    rand_index = np.random.choice(n_records, size=batch_size)
    x_batch = training_X[rand_index, :]
    y_batch = training_Y[rand_index, :]
    training...(training_op, feed_dict={X: x_batch, y_true: y_batch})
```

## Predict

```python
with tf.Session() as sess:
    sess.run([y_hat], feed_dict={ X:test_X, y_true: test_y })    
```





# Keras

## 定义hidden layer size，output layer size, 循环次数， learning_rate

## 新建keras 模型

```python
n_neurons_h = ? # number of neurons in a hidden layer
n_neurons_out = 根据target的类型和数量

n_epochs = 100
learning_rate = 0.1
model = tf.keras.Sequential()
model.add(layers.Dense(n_neurons_h,activation="..."))
model.add(layers.Dense(n_neurons_out,activation="..."))
model.compile(optimizer=tf.train.GradientDescentOptimizer(learning_rate),
loss="...", metrics=["accuracy"])
model.fit(training_X, training_y, epochs=n_epochs)
```

### activation function

- relu
- sigmoid
- softmax
- tanh
- exponential

https://keras.io/api/layers/activations/

### loss function

- MSE
- MAE

https://www.tensorflow.org/api_docs/python/tf/keras/losses/MSE

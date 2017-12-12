# tensorflow
It's a note of studying Tensorflow.:>(hope i can do this better)
## install Tensorflow
how to install the tensorflow in Windows:

becase scipy is very difficult to install in Windows. It's very hard use:
```Shell Session
pip install tensorflow 
```
if you do have some wrong with it.
Maybe you should use 'Anaconda' to get the python environment.
* From the website
* From Visual stdio 2017, you can click the Python environment.
when you ready: installed the Anaconda. you can use 'Anaconda Prompt'
and typed:

```shell Session
# you should change #Alias# to you own name
conda create -n #Alias#
# activeate the environment
activate #Alias#
# just use pip
pip install tensorflow 
```
then add the environment to visual studio.

# Getting Started With TensorFlow
## Issue
    * 1.how many types dose tensorflow have
    * 
1. When i test, only the float32 can be used.

## NOTE
That is the way how to program the tensorflow:
    * build the graph
    * initialize variables
    * start queues
    * handle exceptions
    * create checkpoint files and recover from failures
    * save summaries for TensorBoard

* first step
```python
import tensorflow as tf
``` 
then we can begin our travel to tensorflow.:>
```python
node1 = tf.constant(3.0, dtype=tf.float32)
node2 = tf.constant(4.0) # also tf.float32 implicitly
print(node1, node2)
```
That means you definite two constants, 'node1' and 'node2'.
At this part, these constants. can not be used in tenorflow, they are not the right types.
The final print statement produces:
```python
Tensor("Const:0", shape=(), dtype=float32) Tensor("Const_1:0", shape=(), dtype=float32)
```
Those are not the right values which we expected. '3.0', '4.0'.
>To actually evaluate the nodes, we must run the computational graph within a **session**. A session encapsulates the control and state of the TensorFlow runtime.

    We can use dtype=tf.float32 means using the type 'float32',
    Actually that is the most commonly used type. I did some test, you can change dtype=tf.int32. (<-wrong)

When you use `sess.run(node)` the constants at that time can change to the real value which you can use in Tensorflow.
Now you can add these code below:
```python
sess = tf.Session()
print(sess.run([node1, node2]))
# output [3.0, 4.0]
```
Now the output is what we expected.

During the programming, how do we use variables to program?
We can use the `tf.placeholder`, as the name, placeholder can be the 
```C
printf("%d", n);
```
just like __%d__, we can replace the variables to the place, when you finished the program, you can just change the variales.
## the first way to use variables
```python 
# definite variables
a = tf.placeholder(tf.float32)
b = tf.placeholder(tf.float32)
adder_node = a + b
# use
print(sess.run(adder_node, {a:3, b:4.5}))
print(sess.run(adder_node, {a:[1,3], b:[2,4]}))
```
We can easy to know the synatx about tensorflow is:

    sess.run(opeartion_name, {parameter})

and the format of parameter is

    variables_name : value

About add or any other operation in linear Algbra, you can click the link [deeplearningbook](https://deeplearningbook.org)

## the second way to use variables
```python
W = tf.Variable([.3], dtype=tf.float32)
b = tf.Variable([-.3], dtype=tf.float32)
x = tf.placeholder(tf.float32)
linear_model = W*x + b
# the code below is wrong, you need initialize variables first
print(sess.run(adder_node, {a:3, b:4.5}))
```
In this way, you definite variables, be care about the different from `W = tf.Variable([.3] ...` to `node1 = tf.constant(3.0, ...`.
>Constants are initialized when you call __tf.constant__, and their value can never change. By contrast, variables are not initialized when you call __tf.Variable__. 

you should add this code below:
```python
# initialized the value
init = tf.global_variables_initializer()
sess.run(init)
# then you use this code below
print(sess.run(linear_model, {x: [1, 2, 3, 4]}))
```

## the third way to use variables
You can use the assign methon to change the value.
```python
# In this way, the value of W will change to -1, but at this moment, hasn't change it yet.
fixW = tf.assign(W, [-1.])
fixb = tf.assign(b, [1.])
# At this moment, the values have been changed
sess.run([fixW, fixb])
print(sess.run(loss, {x: [1, 2, 3, 4], y: [0, -1, -2, -3]}))
```
Using the 

    return_value =  tf.assign(parameter)

and the format of parameter is

    Variables_you_want_to_change, value

the value will change when the program running the `sess.run(return_value)`, before that the values haven't changed.
And use this way, you needn't to initialize the value.


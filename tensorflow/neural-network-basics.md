# Neural Network Basics

## Initialize Parameters

```python
learning_rate = 0.001
training_epochs = 20
batch_size = 128 # Decrease batch size if insufficient memory
display_step = 1

# Store layers weight & bias
weights = {
    'hidden_layer': tf.Variable(tf.random_normal([n_input, n_hidden_layer])),
    'out': tf.Variable(tf.random_normal([n_hidden_layer, n_classes]))
}
biases = {
    'hidden_layer': tf.Variable(tf.random_normal([n_hidden_layer])),
    'out': tf.Variable(tf.random_normal([n_classes]))
}

# tf Graph input
x = tf.placeholder('float', [None, 28, 28, 1])
y = tf.placeholder('float', [None, n_classes])

x_flat = tf.reshape(x, [-1, n_input])  # reshape x into row vectors of 784px
```

## Hidden Layer with ReLU Activation Function

```python
# Hidden layer with ReLU activation
layer_1 = tf.add(tf.matmul(x_flat, weights['hidden_layer']), biases['hidden_layer'])
layer_1 = tf.nn.relu(layer_1)

# Output layer with linear activation
logits = tf.add(tf.matmul(layer_1, weights['out']), biases['out'])
```

A Rectified linear unit \(ReLU\) is a type of activation function that is defined as `​f(x) = max(0, x)`​.  TensorFlow provides the ReLU function as `tf.nn.relu()` as shown above.

## Dropout Regularization

```python
keep_prob = tf.placeholder(tf.float32)  # probability to keep units

hidden_layer = tf.add(tf.matmul(features, weights[0]), biases[0])
hidden_layer = tf.nn.relu(hidden_layer)
hidden_layer = tf.nn.dropout(hidden_layer, keep_prob)

logits = tf.add(tf.matmul(hidden_layer, weights[1]), biases[1])
```

The code above illustrates how to apply dropout to a neural network.

`keep_prob` allows you to adjust the number of units to drop.  In order to compensate for dropped units, `tf.nn.dropout()` multiplies all units that are kept by `1/keep_prob`.

During training, a good starting value for `keep_prob` is 0.5.

During testing, use a `keep_prob` value of 1.0 to keep all units and maximize the power of the model.

## Define Loss and Optimizer

```python
# Define loss and optimizer
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits, y))
optimizer = tf.train.GradientDescentOptimizer(learning_rate).minimize(cost)
sess.run([optimizer, cost], feed_dict={X: minibatch_X, Y: minibatch_Y})
```

## Calculate Accuracy

```python
# Calculate accuracy
correct_prediction = tf.equal(tf.argmax(logits, 1), tf.argmax(labels, 1))
accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
```

## Initialize and Run Graph

```python
init = tf.global_variables_initializer()

# Launch the graph
with tf.Session() as sess:
    sess.run(init)
    # Training cycle
    for epoch in range(training_epochs):
        total_batch = int(mnist.train.num_examples/batch_size)
        # Loop over all batches
        for i in range(total_batch):
            batch_x, batch_y = mnist.train.next_batch(batch_size)
            # Run optimization op (backprop) and cost op (to get loss value)
            sess.run(optimizer, {x: batch_x, y: batch_y})
```


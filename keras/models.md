# Models

## [Layer](https://keras.io/layers/core/)

A Keras layer is like a neural network layer. There are fully connected layers, max pool layers and activation layers.

```python
from keras.layers import Dense, Flatten

# 1st layer - Add a flatten layer
model.add(Flatten(input_shape=(32, 32, 3)))

# 2nd layer - Add a fully connected layer
model.add(Dense(100, activation='relu'))
```

Keras will automatically infer the shape of all layers after the first layer. This means user only needs to set input dimensions for the first layer.

## [Compile Model](https://keras.io/getting-started/sequential-model-guide/#compilation)

Configures the learning process.

```python
model.compile(optimizer='adagrad',
              loss='categorical_crossentropy',
              metrics=['accuracy'])
```

## Train Model

Trains the model for a fixed number of epochs.

```python
model.fit(X_train, y_train,
          batch_size=256,
          epochs=EPOCHS,
          verbose=2,
          validation_data=(X_test, y_test))
```

## Evaluate Using Model

```python
score = model.evaluate(X_test, y_test, verbose=1)
```

## Convolutional Neural Network

```python
from keras.layers import Conv2D
model.add(Conv2D(32, kernel_size=(3, 3), activation='relu', input_shape=input_shape))
```

## Max Pooling

```python
from keras.layers import MaxPooling2D
model.add(MaxPooling2D(pool_size=(2, 2)))
```


# Keras

## Create Model

```python
from keras.layers import MaxPooling2D
model.add(MaxPooling2D(pool_size=(2, 2)))
```

The [`keras.model.Sequential`](https://keras.io/models/sequential/) class is a wrapper for the neural network model.

## Summarize Model

```python
model.summary()
```


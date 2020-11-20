Download MNIST dataset


```python
from keras.datasets import mnist
(train_images, train_labels), (test_images, test_labels) = mnist.load_data()
```

Prepare a network using `Sequential` model with 2 fully connected or `Dense` layers of neurons. The first layer with 512 nodes with `relu` activation function. The second layer with 10 nodes, corresponding to `10` labels and using `softmax` activation. The output of this layer will be an array of ten floats, adding up to 1, each represents the probability of the sample to be classified as the corresponding digit.


```python
from keras import models
from keras import layers

network = models.Sequential()
network.add(layers.Dense(512, activation='relu', input_shape=(28 * 28,)))
network.add(layers.Dense(10, activation='softmax'))
```

Here we compile the neural net by specifying the loss function, the optimizer and the evaluation metric.


```python
network.compile(optimizer='rmsprop',
                loss='categorical_crossentropy',
                metrics=['accuracy'])
```

Normalize the `train` and `test` data. The shade of each pixel is represented by a `float32` from 0 to 1, instead of an `uint_8` from 0 to 255.


```python
train_images = train_images.reshape((60000, 28 * 28))
train_images = train_images.astype('float32') / 255

test_images = test_images.reshape((10000, 28 * 28))
test_images = test_images.astype('float32') / 255
```

Here, we encode the labels categorically.


```python
from keras.utils import to_categorical

train_labels = to_categorical(train_labels)
test_labels = to_categorical(test_labels)
```

Now we train the neural net. We will do this training in 5 `epochs` and a `batch_size` of 128. Meaning we update the weights after every 128 samples, and we will iterate through the whole training set 5 times. Two quantities are displayed during training: the loss of the network over the training data, and the accuracy of the network over the training data. 


```python
network.fit(train_images, train_labels, epochs=5, batch_size=128)
```

    Epoch 1/5
    469/469 [==============================] - 1s 1ms/step - loss: 0.4275 - accuracy: 0.8778
    Epoch 2/5
    469/469 [==============================] - 0s 982us/step - loss: 0.1145 - accuracy: 0.9665
    Epoch 3/5
    469/469 [==============================] - 0s 960us/step - loss: 0.0692 - accuracy: 0.9795
    Epoch 4/5
    469/469 [==============================] - 0s 965us/step - loss: 0.0485 - accuracy: 0.9852
    Epoch 5/5
    469/469 [==============================] - 0s 991us/step - loss: 0.0389 - accuracy: 0.9892





    <tensorflow.python.keras.callbacks.History at 0x7f6543f8f4a8>



After the training finishes, we can evaluate the performance of the network with a `test` dataset.


```python
test_loss, test_acc = network.evaluate(test_images, test_labels)
print(f"test_acc: {test_acc}")
```

    313/313 [==============================] - 0s 676us/step - loss: 0.0667 - accuracy: 0.9790
    test_acc: 0.9789999723434448


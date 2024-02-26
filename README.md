# DigitRecognizer
Digit Recognizer with 0.99892 score (Top 2%) on Kaggle with CNN and augmentation.
![image](https://github.com/KyryloTurchyn/DigitRecognizer/assets/64077721/5345efe8-fb7c-465a-8df3-ec008a36d9c5)
## Description 
In this work, the main advantage over other solution options was obtained by using a sophisticated convolutional neural network architecture, data augmentation which resulted in additional X pictures, and concatenation of the regular Kaggle dataset and the MNIST dataset. Callbacks techniques were also applied, to change the learning rate and save the weights of the neural network for later use.
## Content
The explanation of the code can be divided into 3 main parts:
1) Data Preparation
2) About CNN
3) Сallbeaks and compilation
> Also there will be comments-explanations in the code itself
### Data Preparation
First of all, you will need to merge the core data [from Kaggle competition](/[example](https://www.kaggle.com/competitions/digit-recognizer/data)) and the supplementary data [from MNIST](/https://www.kaggle.com/datasets/oddrationale/mnist-in-csv?select=mnist_train.csv) to get a larger set of training data. The training data from the Kaggle competition, as well as the MNIST training and test data will be concatenated. After concatenation we will get 112,000 images instead of the initial 42,000.

Using this trick, training is also performed on a modified test sample. It is simply modified during training when the `ImageDataGenerator` from `tensorflow.keras` is running. This is the essence of the trick - our model is as close as possible to the test dataset of the competition, so that we can identify them more correctly. After all, the model can find features in the modified test sample and then easily identify them in the target test sample as well.

After that there is the usual preparation of data in the format of photo 28x28 pixels for the neural network, dividing the data into test and training data, standardisation by dividing by 255 and etc.
### About CNN
This diagram describes a neural network
![qweqwe drawio (2)](https://github.com/KyryloTurchyn/DigitRecognizer/assets/64077721/c610d1a3-e2a8-419d-b9a7-528aaa07ce23)
The neural network architecture is a sequential neural network with multiple convolutional and fully connected layers.

The input layer is defined explicitly through the corresponding layer and the input data size (for one image) is 28×28, with one channel. Immediately after the input layer is the `ZeroPadding2D` layer, which adds a certain number of zero rows and columns. After `ZeroPadding2D`, the data goes to the convolution layer with a convolution kernel of 5×5 and a number of 32 filters, followed by batch normalisation to stabilise the neural network and speed up its training. Then all data passes through the relu activation function, followed by a `MaxPooling2D` and `ZeroPadding2D` layer with `Dropout`, which temporarily does not change a certain percentage of neurons.

Next come two similar blocks, only each of them has the convolution kernels and the number of filters changed on convolution layers.

At the end, multidimensional tensors are smoothed into one dimension (in other words, the multidimensional array becomes one-dimensional), after which the data passes through one more layer with the relu activation function, Dropout's and at the output we get the result of the softmax activation function, which determined the class of the digit in the image (from 0 to 9).

In general, this is all that is related to the network architecture.
![image](https://github.com/KyryloTurchyn/DigitRecognizer/assets/64077721/4586133b-1f6b-4328-8ec8-0979de8d1317)
### Сallbeaks and compilation
The first colback is `ModelCheckpoint`, which stores the weights of the neural network matrix whenever the validation result improves after an epoch.I chose `val_accuracy` (lots of correct answers) as the monitoring criteria and specified that we only save the best result to a specific folder.

The second colback is `ReduceLROnPlateau`, which reduces the learning rate by a certain value every time the result does not improve. The patience parameter is responsible for the number of `epochs` (or iterations) to multiply the learning rate by the `factor` parameter, and `min_lr` determines how much we can minimally reduce the learning rate.

After that you can run compilation and get results.

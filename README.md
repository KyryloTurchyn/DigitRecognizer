# DigitRecognizer
Digit Recognizer with 0.99892 score (Top 2%) on Kaggle with CNN and augmentation.
![image](https://github.com/KyryloTurchyn/DigitRecognizer/assets/64077721/5345efe8-fb7c-465a-8df3-ec008a36d9c5)
## Description 
In this work, the main advantage over other solution options was obtained by using a sophisticated convolutional neural network architecture, data augmentation which resulted in additional X pictures, and concatenation of the regular Kaggle dataset and the MNIST dataset. Callbacks techniques were also applied, to change the learning rate and save the weights of the neural network for later use.
## Content
The explanation of the code can be divided into 3 main parts:
1) Data Preparation
2) About CNN
3) Kolbeeks and compilation
> Also there will also be comments-explanations in the code itself
### Data Preparation
First of all, you will need to merge the core data [from Kaggle competition](/[example](https://www.kaggle.com/competitions/digit-recognizer/data)) and the supplementary data [from MNIST](/https://www.kaggle.com/datasets/oddrationale/mnist-in-csv?select=mnist_train.csv) to get a larger set of training data. The training data from the Kaggle competition, as well as the MNIST training and test data will be concatenated. After concatenation we will get 112,000 images instead of the initial 42,000.

Using this trick, training is also performed on a modified test sample. It is simply modified during training when the `ImageDataGenerator` from `tensorflow.keras` is running. This is the essence of the trick - our model is as close as possible to the test dataset of the competition, so that we can identify them more correctly. After all, the model can find features in the modified test sample and then easily identify them in the target test sample as well.

After that there is the usual preparation of data in the format of photo 28x28 pixels for the neural network, dividing the data into test and training data, standardisation by dividing by 255 and etc.

![qweqwe drawio](https://github.com/KyryloTurchyn/DigitRecognizer/assets/64077721/6f968df1-30a0-4912-8712-a039c9e9b21b)

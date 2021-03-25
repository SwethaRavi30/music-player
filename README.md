# music-player
This application can classify a song into genres and detect emotion from an audio and play a song based on the emotion detected. Knn model is used for music genre classification. Multilayer perceptron model is used for emotion detection. merge_music_speech.ipynb file is for detecting emotion from audio files and play music accordingly.


This package contains three scripts.. 

1.Music Genre Classification – Automatically classify different musical genres

We will classify these audio files using their low-level features of frequency and time domain.

The GTZAN genre collection dataset has audio tracks of similar size and frequency.There are 9 genres each containing 100 files . Each audio file is of  30 seconds duration. Each track is in .wav format. The genres are as follows: 
Blues
Classical
Country
Disco
Hiphop
Metal
Pop
Reggae
Rock
 



The music genre recognition is implemented using the K- Nearest Neighbours Algorithm. 
The following libraries are used:

1.python_speech_features  for filtering the useful information from the audio files.
2.scipy.io.wavfile for reading audio files.
3. Numpy for performing statistical operations.
4. Tempfile for creating temporary files and directories.
5. os for interacting with the operating system.
6. pickle for converting the data structure into binary format.
7.random for splitting the dataset into test and training sets.
8.operator for list operations.
9. Math for performing mathematical operations
10.matplotlib.pyplot for plotting graphs.
11.sklearn.metrics for getting performance metrics.
12.playsound to play music.









The .wav audio files are read using wav.read() ,wav.read() returns the sample rate and a numpy array with all the data read from the file.
The features are extracted using Mel Frequency Cepstral Coefficients(Since audio signals are constantly changing ,the signals are divided into smaller frames each of 20-40ms .The different frequencies present in the each frame are identified,then noise is removed using DCT (discrete cosine transform).And finally only specific sequence of frequencies that have high probability of informations are returned.
Mfcc returns a numpy array containing features. Each row holds 1 feature vector.
Each feature vector here has 13 attributes.
Those 13 attributes are:

Signal : the audio signal from which to compute features.
Samplerate : the sample rate of the signal we are working with.
Winlen : the length of the analysis window in seconds . Default is 0.0025 s
Winstep : the step between successive windows in seconds . Default is 0.01 s 
Numcep: the number of cepstrum to return , default is 13.
nfilt:  the number of filters in the filter bank , default is 26 
Nftt : the FFT size . Default is 512. 
Low Freq : lowest band edge of mel filters . In Hz , default is 0.
high freq : highest band edge of mel filters.In Hz,default is 0.
 preemph : apply pre emphasis filter with preemph coefficient.0 is no filter.Default is 0.97.
ceplifter : apply a lifter to final cepstral coefficients.0 is no lifter.Default is 22.
appendEnergy : if this is true,the zeroth cepstral coefficient is replaced with the log of the
           total frame energy.
      13. winfunc : the analysis window to apply to each frame.By default no window is applied.


Covariance matrix and mean matrix are calculated for the feature vectors.
These matrices are used for distance calculation.
The covariance matrix,mean matrix and the class label are put together in a tuple and all this is dumped into a binary file using pickle.







Now the dataset is read from the binary file and is split into test and training sets using random() in the ratio 34:66 respectively.

We can observe that there are a  total of 588 training instances and 900 is the total number of instances(test +training).

Knn is a lazy learning algorithm ,during the training phase the data is just stored .
During test phase classification occurs, the distance between each of the training set instances and the test instance is found, the k nearest instances’s classes (k nearest neighbours )are found and the test instance is classified to the class that is present in the k nearest neighbours.

Using mean matrix and covariance matrix with the help of numpy (linear algebra module)
The distance between two instances is calculated and returned.

The distance between each training instance and the test instance is calculated,the distances are sorted and the k neighbours are returned.

The class that has the highest frequency is returned. 


The confusion matrix is constructed and the accuracy and error rate is calculated.


To find the optimal value of k ,error rate and accuracy are plotted.





For k=5 maximum accuracy and minimum error is observed.



Now for a new audio file we predict the genre.


playmusic() is invoked from another script.
It plays all the songs present in the genre that it gets as its parameter.


Speech Emotion Recognition - Detect the emotion from the given audio files(speech files)

Emotion can be detected from the tone and pitch of our voice.
To analyze  the audio files librosa library is used.

RAVDESS(Audio-Visual Database of Emotional Speech and Song ) dataset is used in this package .It has 7356 files rated by 247 individuals 10 times on emotional validity, intensity, and genuineness.
The emotions available:
Happy
Angry
Neutral
Sad
Calm
Fearful
Disgust
Surprised

Speech emotion recognition is implemented using the Multilayer Perceptron model using different activation functions and Logistic regression separately to find the better model for the dataset.

Speech emotion recognition is implemented using the Multilayer Perceptron model with the help of following libraries.


Librosa for audio and music processing in Python.
SoundFile  for reading and writing sound files.
Glob  is used to retrieve files/path names matching a specified pattern.
Sklearn.model_selection  -for split arrays or matrices into random training and test sets.
Sklearn.neural_network - to use MLPClassifier model




The mfcc, chroma, and mel features from a sound file are extracted. 
mfcc: Mel Frequency Cepstral Coefficient, represents the short-term power spectrum of a sound
chroma: Pertains to the 12 different pitch classes
mel: Mel Spectrogram Frequency
The sound file is read and the features are extracted.The dataset is split into test (25%) and training sets(75%).
 

Learning_rate: 'adaptive’ keeps the learning rate constant to ‘learning_rate_init’ as long as training loss keeps decreasing. Each time two consecutive epochs fail to decrease training loss by at least tol, or fail to increase validation score by at least tol if ‘early_stopping’ is on, the current learning rate is divided by 5.
alpha:L2 penalty (regularization term) parameter
batch_size:Size of mini batches for stochastic optimizers.
max_iter:Maximum number of iterations. 
epsilon:Value for numerical stability in adam. Only used when solver=’adam’
 
Accuracy of each model is found using sklearn. Using the best model the emotions are classified.
MLP
Activation function: ReLu


Using the Rectifier Linear Unit as the Activation function the accuracy for the model is found. 
Activation function: Sigmoid
Activation function: Tanh

Logistic Regression		



 
From comparing the accuracies we can infer that Mlp Model is better for this dataset. 
We can infer that Mlp with Sigmoid has the highest accuracy.
MLPClassifier is initialized with different activation functions(Re-Lu , tanh, logistics)and the model with best accuracy is chosen.This is a Multi-layer Perceptron Classifier; it optimizes the log-loss function using LBFGS or stochastic gradient descent. Unlike SVM or Naive Bayes, the MLPClassifier has an internal neural network for the purpose of classification. This is a feedforward ANN model.

speech_res() is invoked from another script,it returns the predicted emotions.
 
 
 
The final objective of this package is  to integrate these two models.When a speech is given the emotion is recognised and a music is played for that emotion automatically.

The third script is based on the integration of the above two scripts . Now based on the speech inputted by the user , the appropriate music comfortable for the user at that situation is played . This is implemented with the help of the speech_res()  and playmusic(). 
 
 
 
 
 
 

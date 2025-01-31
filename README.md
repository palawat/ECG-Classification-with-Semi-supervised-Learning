# ECG-Classification-with-Semi-supervised-Learning

Electrocardiogram (ECG) analysis is one of the tasks that deep learning may help physicians to diagnotic pateints faster and more accurate. For this repository, I will show how to apply Semi-Supervised Learning (SSL) to improve predictive model on a small dataset of ECG. I used [ECG200](http://www.timeseriesclassification.com/description.php?Dataset=ECG200) dataset formatted by R. Olszewski. The dataset only contains 100 samples for training and 100 samples for testing. Each series traces the electrical activity recorded during one heartbeat. The two classes are a normal heartbeat and a myocardial infarction. The examples of ECG for each class are shown in a figure below.


![](images/exampleECG.png)

The challenge of this dataset is is quite small for a supervised deep learning model to work well. Therefore, **semi-supervised learning** is considered to learn features by Bi-directional LSTM Autoencoder (*Unsupervised learning*). After that, the encoder part (with pre-trained weights) of the autoencoder is used to build a machine learning classifier (*Supervised learning*). 

## SSL with Bi-LSTM model
![](images/SSL_architect.png)

### Methodology 
**Phase 1**: Time-Series (Inputs) Reconstruction (Unsupervised Learning)

Train the autoencoder (architecture on the left side) to reconstruct the input data, i.e., X is both input and output. The objective is to make the encoder learn dense representations of their inputs without requiring their labels. 

**Phase 2**: Classify types of ECG signals (Supervised Learning)

Take the encoder (or codings) from the autoencoder to train a Random Forest classifier on the training data, i.e., X is the input and Y is the output. Nested 5-fold cross-validation is used for selection of best parameters and model evaluation.

### Evaluation on the Test Data

![](images/report_table.JPG)

From the table, the Semi-Supervised Random Forest could achieve **93%** accuracy, which is *higher than the best accuracy score* shown on the [website](http://www.timeseriesclassification.com/description.php?Dataset=ECG200) (89.0%). The confusion matrix of this model predictions on the test data is shown below.

![](images/confusion_matrix.png)


#### References:
- http://www.timeseriesclassification.com/description.php?Dataset=ECG200
- https://www.tensorflow.org/tutorials/generative/autoencoder
- Geron, Aurelien. Hands-on Machine Learning with Scikit-Learn, Keras and TensorFlow: Concepts, Tools, and Techniques to Build Intelligent Systems. 2nd ed., O’Reilly, 2019.

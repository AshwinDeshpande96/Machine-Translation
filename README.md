# Machine-Translation
Machine translation model to translate sentences from English to Hindi.
This is an investigation of attention based neural machine translation project in low-resource environment. A similar study was found here: [A study of attention-based Neural Machine Translation models on Indian
Languages](http://cse.iitkgp.ac.in/~ayand/W16-3717.pdf)

## 1. Dataset
This dataset is obtained from [IIT-Bombay English-Hindi Parallel Corpus](http://www.cfilt.iitb.ac.in/iitb_parallel/). Parallel dataset consists of one file with 1,561,840 segments. However, majority of the corpus is made of segments such as 'IPython Console', 'Hide private attributes', 'The default plugin layout for the bottom panel' etc. As you can see these are Computer Science jargons and do not reflect natural language. Thus, dataset with 3027 sentences was chosen as it consists of natural language sentences.

      The minimum noise level was found to be 72.5 decibels and the maximum 83.6 decibels.
      
      
      जांच न्यूनतम ध्वनि प्रदूषण 72.5 और अधिकतम 83.6 पाया गया।
      
Lack of data incurs huge amount variance error, and hence the predictions are local to training data. Therefore this is not a generalized model, and requires more amount of data. This causes some problems, these problems are shown in the following text.

## 2. Model Architecture

This model has a encoder-decoder type architecture. Encoder and Decoder are Recurrent Neural Networks with an Attention Layer in between. It is based on [Bahdanau Attention](https://arxiv.org/abs/1409.0473). 
Following is a gross summary of the model and is exactly implemented in this project.
* It takes as input a sequence of words: (x<sub>1</sub>, x<sub>2</sub>, ... , x<sub>n</sub>) in English.
* Encoder: is a GRU recurrent neural network.
  * Takes as input:
    * Sentence (Sequence of words in English)
    * Embedding layer encodes the words to a vector.
  * This outputs two vectors:
    * encoder output: shape-(number_of_sample, length_of_rnn, output_size).
    * hidden state: shape-(number_of_sample, output_size).
* Self Attention Layer creates Context-Vector as follows:
  * Run the encoder output through a fully connected layer.
  * Run the hidden state through another fully connected layer.
  * Add the two outputs on fully connected layer.
  * Activate using tanh.
  * Run activated vector through another fully connected layer.
  * Output of the fully connected layer is the Context vector.
* Decoder:
  * Takes as input:
    * If decoder is at word i, input is the words preceding: (z<sub>1</sub>, z<sub>2</sub>, ... , z<sub>i-1</sub>) in Hindi.
    * Hence initially a start token until n <sup>th</sup> word i.e. the end token
    * Embedding layer encodes the words to a vector.
    * Concatenate with context vector
    * Concatenated vector is given to the Recurrent Neural Network
  * This outputs two vectors:
    * The next-word prediction: which is appended to the Decoder input for next iteration. (This repeats until a end token is found.
    * Attention Matrix: this shows the weight(importance) given to each input words for each prediction.
    
By the end of the sentence (i.e. when end token in encountered), we will have predicted the Hindi sentence equivalent of the input sentence in English.

## 3. Applications

This application is most favourable in areas where English is not the most spoken language, and yet some vital information needs to translated in order to carry out necessary task, such as filling an application form, instructions at a bank/public office etc.

## 4. Results
One example is given as follows: 
 
    Input: the doctors were able to remove ganga and chandra kiran from the damaged car with some difficulty and took them to the sundernagar civil home . <end>
    
    
    Predicted translation: पुलिस ने गंगा व चंद्र किरण को कड़ी मशक्कत के बाद क्षतिग्रस्त टैक्सी से बाहर निकाला और नागरिक अस्पताल सुंदरनगर ले गए। <end> 
This input is very similar to training data, only two words: 'police' and 'hospital' were replaced with 'doctors' and 'home' respectively. Therefore the output while it produces excellent results is insensitive to tiny changes, and cannot generalize well. Availability of larger corpus should definitely help alleviate this issue. 
Full implementation of this project is given [here](https://github.com/AshwinDeshpande96/Machine-Translation/blob/master/NMT_Hindi_English.ipynb)

## 5. Future Work

We are working on a model that omits RNN or CNN architecture completely and only used Attention: [Attention is all you need](https://arxiv.org/abs/1706.03762). This part is under progress and will uploaded soon.

# Machine-Translation
Machine translation model to translate sentences from English to Hindi.
This is a low-resource neural machine translation project. 

## 1. Dataset
This dataset is obtained from [IIT-Bombay English-Hindi Parallel Corpus](http://www.cfilt.iitb.ac.in/iitb_parallel/). Parallel dataset consists of one file with 1,561,840 segments. However, majority of the corpus is made of segments such as 'IPython Console', 'Hide private attributes', 'The default plugin layout for the bottom panel' etc. As you can see these are Computer Science jargons and do not reflect natural language. Thus, dataset with 3027 sentences was chosen as it consists of natural language sentences. Lack of data incurs huge amount variance error, and hence the predictions local to training data. Therefore this is not a generalized model, and requires significant amount of data.

## 2. Model Architecture

This model has a encoder-decoder type architecture. Encoder and Decoder are Recurrent Neural Networks with an Attention Layer in between. It is based on [Bahdanau attention](https://arxiv.org/abs/1409.0473). 
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
 
    Input: the police were able to remove ganga and chandra kiran from the damaged car with some difficulty and took them to the sundernagar civil hospital . <end>
    
    
    Predicted translation: पुलिस ने गंगा व चंद्र किरण को कड़ी मशक्कत के बाद क्षतिग्रस्त टैक्सी से बाहर निकाला और नागरिक अस्पताल सुंदरनगर ले गए। <end> 

Full implementation of this project is given [here](https://github.com/AshwinDeshpande96/Machine-Translation/blob/master/NMT_Hindi_English.ipynb)

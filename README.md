# Machine-Translation
Machine translation model to translate sentences from English to Hindi.
This is a low-resource neural machine translation project. With only 3027 sentences: [IIT-Bombay English-Hindi Parallel Corpus](http://www.cfilt.iitb.ac.in/iitb_parallel/). 

This model is based on [Bahdanau attention](https://arxiv.org/abs/1409.0473). 
* It takes as input a sequence of words: (x<sub>1</sub>, x<sub>2</sub>, ... , x<sub>n</sub>) in English.
* Encoder:
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
* Decoder:
  * Takes as input:
    * If decoder is at word i, input is the words preceding: (z<sub>1</sub>, z<sub>2</sub>, ... , z<sub>i-1</sub>) in Hindi.
    * Hence initially a start token until n <sup>th</sup> word i.e. the end token
    * Embedding layer encodes the words to a vector.
    * Concatenate with context vector
  * This outputs two vectors:
    * The next-word prediction: which is appended to the Decoder input for next iteration. (This repeats until a end token is found.
    * Attention Matrix: this shows the weight(importance) given to each input words for each prediction.

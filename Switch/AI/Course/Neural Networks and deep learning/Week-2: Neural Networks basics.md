# Neural network basics:

## Binary classification
- When we organize the computation of a neural network, usually we have what's called a forward pass or forward propagation step, followed by a backward pass or what's called a backward propagation step.
- Logistic regression is an algorithm for binary classification.
- example:

  - let's say, we have an image, if the image is cat, we output 1, and if the image is not-cat, we output 0.
  - To store an image, computer stores three separate matrices corresponding to red, green and blue color channels of this image. So, in order to turn these pixel intensity values, into feature vector, we wil unroll all these pixel values into an input feature vector x.

    <img width="1107" height="505" alt="image" src="https://github.com/user-attachments/assets/77695ec5-21a5-4fdd-bbd7-0c59f814939b" />

  - So, in binary classification, our goal is to learn a classifier that can input an image by this feature vector and predict whether the corresponsing label y is 0 or 1.
  - A single training example is represented by (x,y), where x is am x-dimensional feature vector, and y, the label, is either 0 or 1.
  - Let's say, our training set comprise of m training examples.
  - for m traingin example, we can write: mtraing, and for m testing set, we can write: mtest
  - so, the input matrix, consisting of all the input feature vector x, will be denoted by X.

    <img width="472" height="271" alt="image" src="https://github.com/user-attachments/assets/baf749fb-ba46-4e03-9638-41315787404e" />

  - and the matrix, of all the output of m trainging examples, will be denoted by Y.

    <img width="440" height="97" alt="image" src="https://github.com/user-attachments/assets/381b581d-0627-409c-a017-f872f40b9e99" />

## Logistic Regression:
- Logistic regression is an algorithm that we use when the output labels y in a supervised learning problem are all either 0 or 1, so for binary classification problems.
- 
- 

# Introduction to deep learning

- CNN: Convolutional Neural Network
- CNN are often applied to images.
- Sequence models includes models like recurrent neural network, abbreviated RNNs, and LSTM models (Long short term memory models).

## What is Neural Network
- Deep learning, refers to training neural networks, sometimes very large neural networks.
- example:

  - Let's consider we have data of size of the house and its price. So, let's say, i want to fit this data into a single line. Means, we are applying linear regression here. Price vs size graph. This for housing price prediction. 
  - As we know, price can not be null, so we will be removing the part of line where the price becomes null.

    <img width="618" height="415" alt="image" src="https://github.com/user-attachments/assets/5483922c-e8cb-4eed-bc16-4682799dbaa6" />

  - So, the above sturucture is the simplest possible neural network.
  - Here, we have input (size), which we give it to this cirlce (which is a single neuron in neural network), and then it outputs (price).
  - So, this single neuron in neural network implements this functions that we have defined in the above graph of price vs. size.

    <img width="354" height="173" alt="image" src="https://github.com/user-attachments/assets/c4d6773a-c27d-49e9-8c22-71128ce44e91" />
  -  The above one is the functions. This functions which goes zero sometimes and then it will take of as a straight line.
  -  This function is known as ReLU function which stands for rectified linear units. And rectify just means, taking a max of 0, which is why the functions looks like this.
  -  As this is just a single neuron, a larger neural network is then formed by taking many of single neurons and stacking them together.
  -  Below shows, how the ReLU's are stacked together to form neural network:
 
    <img width="949" height="511" alt="image" src="https://github.com/user-attachments/assets/d959b7e8-32ca-4ab8-bf4c-3c0bf53200d2" />
  -  And, this is how we can implement this:

    <img width="771" height="371" alt="image" src="https://github.com/user-attachments/assets/48c10ab4-f49c-4447-acb5-70947209ecba" />

  - Here, we have neural network with 4 inputs. So, given these input feature, the job of the neural network is to predict the price Y.
  - The layer with all the inputs, is known as input layer.
  - So, given enough data about x and y, given enough training examples with x and y, neural networks are remarkably good at figuring out functions that accurately map from x to y.

## Supervised learning with neural networks:
- In supervised learning, we have some input x, and we want to learn a functions mapping to some output y.
-  

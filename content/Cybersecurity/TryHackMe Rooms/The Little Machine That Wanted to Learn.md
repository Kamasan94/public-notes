#cybersecurity #machine-learning

Machine learning refers to the process used to create a system that can mimic the behavior we see in real life, examples:
- **Genetic algorithm:** This ML structure aims to mimic the process of natural selection and evolution. By using rounds of offspring and mutations based on the criteria provided, the structure aims to create the "strongest children" through "survival of the fittest".
- **Particle swarm:** This ML structure aims to mimic the process of how birds flock and group together at specific points. By creating a swarm of particles, the structure aims to move all the particles to the optimal answer's grouping point.
- **Neural networks:** This ML structure is by far the most popular and aims to mimic the process of how neurons work in the brain. These neurons receive various inputs that are then transformed before being sent to the next neuron. These neurons can then be "trained" to perform the correct transformations to provide the correct final answer.

### **Learning Styles**
- **Supervised learning:** In this learning style, we guide the neural network to the answers we want it to provide. We ask the neural network to give us an answer and then provide it with feedback on how close it was to the correct answer. In this way, we are supervising the neural network as it learns. However, to use this learning style, we need a dataset where we know the correct answers. This is called a labelled dataset, as we have a label for what the correct answer should be, given the input.
- **Unsupervised learning:** In this learning style, we take a bit more of a hands-off approach and let the neural network do its own thing. While this sounds very strange, the main goal is to have the neural network identify "interesting things". Humans are quite good at most classification tasks – for example, simply looking at an image and being able to tell what colour it is. But if someone were to ask you, "Why is it that colour?" you would have a hard time explaining the reason. Humans can see up to three dimensions, whereas neural networks have the ability to work in far greater dimensions to see patterns. Unsupervised learning is often used to allow neural networks to learn interesting features that humans can't comprehend that can be used for classification. A very popular example of this is the restricted Boltzmann machine. Have a look [here](https://scikit-learn.org/stable/modules/neural_networks_unsupervised.html) at the weird features the neural network learned to classify different digits.

### **Basic structure**
The neural network has three main layers:

- **Input layer:** This is the first layer of nodes in the neural network. These nodes each receive a single data input that is then passed on to the hidden layer. This means that the number of nodes in this layer always matches the network's number of inputs (or data parameters). For example, if our network takes the toy's length, width, and height, there will be three nodes in the input layer.
- **Output layer:** This is the last layer of nodes in the neural network. These nodes send the output from the network once it has been received from the hidden layer. Therefore, the number of nodes in this layer will always be the same as the network's number of outputs. For example, if our network outputs whether or not the toy is defective, we will have one node in the output layer for either defective or not defective (we could also do it with two nodes, but we won't go into that here).
- **Hidden layer:** This is the layer of nodes between the neural network's input and output layers. With a simple neural network, this will only be one layer of nodes. However, for additional learning opportunities, we could add more layers to create a deep neural network. This layer is where the neural network's main action takes place. Each node within the neural network's hidden layer receives multiple inputs from the nodes in the previous layer and will then transmit their answers to multiple nodes in the next layer.


### **Hidden layer**
- **Inputs are not directly added.** Instead, they are multiplied by a weight value first. This helps the neural network decide which inputs should contribute more to the output than others.
- **The addition's output is not directly transmitted out.** Instead, the output is first entered into what is called an activation function. In essence, this decides if the neuron (node) will be active or not. It does this by ensuring that the output, no matter the input, will always be a decimal between 0 and 1 (or between −1 and 1).

### **Feed-Forward Loop**
1. **Normalise all of the inputs:** To allow our neural network to decide which inputs are most important in helping it to decide the answer, we need to normalise them. As mentioned before, each node in the network tries to keep its answer between 0 and 1. If we have one input with a range of 0 to 50 and another with a range of 0 to 2, our network won't be able to properly consume the input. Therefore, we normalise the inputs first by adjusting them so that their ranges are all the same. In our example here, we would take the inputs with a 0 to 50 range and divide all of them by 25 to change their ranges to 0 to 2.
2. **Feed the inputs to our nodes in the input layer:** Once normalised, we can provide one data entry for each input node in our network.
3. **Propagate the data through the network:** At each node, we add all the inputs and run them through the activation function to get the node's output. This output then becomes the input for the next layer of nodes. We repeat this process until we get to our network's output layer.
4. **Read the output from the network:** At the output layer of the network, we receive the output from our nodes. The answer will be a decimal between 0 and 1, but, for decision-making, we'll round it to get a binary answer from each output node.


### Back propagation
When we are training our network, the feed-forward loop is only half of the process. Once we receive the answers from our network, we need to tell it how close it was to the correct answer.

1. **Calculate the difference in received outputs vs expected outputs:** As mentioned before, the activation function will provide a decimal answer between 0 and 1. Since we know that the answer has to be either 0 or 1, we can calculate the difference in the answer. This difference tells us how close the neural network was to the correct answer.
2. **Update the weights of the nodes:** Using the difference calculated in the previous step, we can start to update the weights of each input to the nodes in the output layer. We won't dive too deep into this update process, as it often involves a bit of complex maths to decide what update should be made.
3. **Propagate the difference back to the other layers:** This is where the term back-propagation comes from. Once the weights of the nodes in the output layer have been updated, we can calculate what the difference would be for the previous nodes. Once again, this difference is then used to update the weights of the nodes in that layer before being propagated backwards even more. We continue this process of back-propagation until the weights for the input layer have been updated.

Once all the weights have been updated, we can run another sample of data through our network. We repeat this process with all our samples in order to train our network.


### **Dataset Splits**
 We need to validate that our neural network is learning the process and not the answers. This validation also tells us when we need to stop our learning process. To perform this validation, we have to split our dataset into the three datasets below:

- **Training data:** This is our largest dataset. We use it to train the network. Usually, this is about 70–80% of the original dataset.
- **Validation data:** This dataset is used to validate the network's training. After each training round, we send this data through our network to determine its performance. If the performance starts to decline, we know we're starting to overtrain and should stop the process. Usually, this is about 10–15% of the original dataset.
- **Testing data:** This dataset is used to calculate the final performance of the network. The network won't see this data at all until we are done with the training process. Once training is complete, we send through the testing dataset to determine the performance of our network. Usually, this is about 10–15% of the original dataset.


### Create a neural network

```python
#C:\pyton310\python.exe

  

#These are the imports that we need for our Neural Network

#Numpy is a powerful array and matrix library used to format our data

import numpy as np

#Pandas is a data processing library that also allows for reading and formatting data structures

import pandas as pd

#This will be used to split our data

from sklearn.model_selection import train_test_split

#This is used to normalize our data

from sklearn.preprocessing import StandardScaler

#This is used to encode our text data to integers

from sklearn.preprocessing import LabelEncoder

#This is our Multi-Layer Perceptron Neural Network

from sklearn.neural_network import MLPClassifier

  

#These are the colour labels that we will convert to int

colours = ['Red', 'Blue', 'Green', 'Yellow', 'Pink', 'Purple', 'Orange']

  
  

#Read the training and testing data files

training_data = pd.read_csv('training_dataset.csv')

training_data.head()

  

testing_data = pd.read_csv('testing_dataset.csv')

testing_data.head()

  

#The Neural Network cannot take Strings as input, therefore we will encode the strings as integers

encoder = LabelEncoder()

encoder.fit(training_data["Colour Scheme"])

training_data['Colour Scheme'] = encoder.transform(training_data['Colour Scheme'])

testing_data['Colour Scheme'] = encoder.transform(testing_data['Colour Scheme'])

  
  
  

#Read our training data from the CSV file.

#First we read the data we will train on

X = np.asanyarray(training_data[['Height','Width','Length','Colour Scheme','Maker Elf ID','Checker Elf ID']])

#Now we read the labels of our training data

y = np.asanyarray(training_data['Defective'].astype('int'))

  

#Read our testing data

test_X = np.asanyarray(testing_data[['Height','Width','Length','Colour Scheme','Maker Elf ID','Checker Elf ID']])
```

1. The first few lines are all the library imports that we need for our neural network. We will make use of [pandas](https://pandas.pydata.org/) to read our datasets and [scikit-learn](https://scikit-learn.org/) for building our neural network.
2. Next, we load the datasets. In our case, there is a training and testing dataset. While we have the labels for the training dataset, we don't have them for the testing dataset. So, while we can perform supervised learning, we will only know our neural network's true performance once we have uploaded our predictions for review.
3. Once the data is loaded, we need to make sure that all the inputs are numerical values. One of our data types is the toy's colour scheme. In order to provide this data to our network, we will encode the colours to numbers.
4. Lastly, we load the data. Variable X is used to store our training dataset together with its labels stored in variable y. Lastly, test_X stores the testing dataset that we will use to perform the predictions on.

##### Creating datasets

```python
train_X, validate_X, train_y, validate_y = train_test_split(X, y, test_size=0.2)
```

##### Normalising the data
```python
scaler = StandardScaler()
scaler.fit(train_X)
 
train_X = scaler.transform(train_X)
validate_X = scaler.transform(validate_X)
test_X = scaler.transform(test_X)
```


##### **Training the Neural Network**
```python
clf = MLPClassifier(solver='lbfgs', alpha=1e-5,hidden_layer_sizes=(15, 2), max_iter=10000)
```

1. **solver=''** - This is the algorithm used to update the weights. This is a classic back-propagation algorithm, but others can be used as well.
2. **alpha=''** - The alpha value is used for the regularisation of the neural network. We won't dive too deep into the maths here, but we have selected a fairly default value.
3. **hidden_layer_sizes=''** - This tells us the structure of the hidden layers in our neural network. Based on the provided configuration, we will have 2 hidden layers with 15 nodes in each.
4. **max_iter=''** - This sets a cap on the number of iterations we can train our neural network before it is forcibly stopped.

We can train our classifier
```python
clf.fit(train_X, train_y)
```

##### **Validate our Neural Network**
```python
y_predicted = clf.predict(validate_X)
```

### CyberSec Applications for Machine Learning
- Can be used for classifications that are complex, such as whether network traffic is malicious or not
- Good at anomaly detection
- Can be used for authentication applications such as biometric authentication

### CyberSec Cautions for Machine Learning
- It should be used in conjunction with humans to play to each of their strengths.
-  it can also be used for _offence_



### Answers
**What is the other term given for Artificial Intelligence or the subset of AI meant to teach computers how humans think or nature works?**
Machine learning

**What ML structure aims to mimic the process of natural selection and evolution?**
Genetic algorithm

**What is the name of the learning style that makes use of labelled data to train an ML structure?**
Supervised learning

**What is the name of the layer between the Input and Output layers of a Neural Network?**
Hidden layer

**What is the name of the process used to provide feedback to the Neural Network on how close its prediction was?**
back-propagation
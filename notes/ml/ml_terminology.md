Machine learning combines Applied Math, Statistics, and Computer Science and each field may use different formal definitions of the same terms. Here are the terminology 

# Terminology

**Machine learning**, or ML, is a modern software development technique that enables computers to solve problems by using examples of real-world data. Here are some formal definitions:

In **supervised learning**, every training sample from the dataset has a corresponding label or output value associated with it. As a result, the algorithm learns to predict labels or output values.

In **reinforcement learning**, the algorithm figures out which actions to take in a situation to maximize a reward (in the form of a number) on the way to reaching a specific goal.

In **unsupervised learning**, there are no labels for the training data. A machine learning algorithm tries to learn the underlying patterns or distributions that govern the data.

A **model** is an extremely generic program, made specific by the data used to train it.

**Model training algorithms** work through an interactive process where the current model iteration is analyzed to determine what changes can be made to get closer to the goal. Those changes are made and the iteration continues until the model is evaluated to meet the goals.

**Model inference** is when the trained model is used to generate predictions.

**Clustering**. Unsupervised learning task that helps to determine if there are any naturally occurring groupings in the data.

A **categorical label** has a discrete set of possible values, such as "is a cat" and "is not a cat."

A **continuous (regression) label** does not have a discrete set of possible values, which means possibly an unlimited number of possibilities.

**Discrete**: A term taken from statistics referring to an outcome taking on only a finite number of values (such as days of the week).

A **label** refers to data that already contains the solution.

Using **unlabeled** data means you don't need to provide the model with any kind of label or solution while the model is being trained.

*Impute* is a common term referring to different statistical tools which can be used to calculate missing values from your dataset.

*Outliers* are data points that are significantly different from others in the same sample.

**Hyperparameters** are settings on the model which are not changed during training but can affect how quickly or how reliably the model trains, such as the number of clusters the model should identify.

A **loss function** is used to codify the model’s distance from this goal

**Training dataset**: The data on which the model will be trained. Most of your data will be here.

**Test dataset**: The data withheld from the model during training, which is used to test how well your model will generalize to new data.

**Model parameters** are settings or configurations the training algorithm can update to change how the model behaves.

**Log loss** seeks to calculate how uncertain your model is about the predictions it is generating.

**Model Accuracy** is the fraction of predictions a model gets right.

**Continuous**: Floating-point values with an infinite range of possible values. The opposite of categorical or discrete values, which take on a limited number of possible values.

**Hyperplane**: A mathematical term for a surface that contains more than two planes.

**Plane**: A mathematical term for a flat surface (like a piece of paper) on which two points can be joined by a straight line.

**Regression**: A common task in supervised machine learning.

**Bag of words**: A technique used to extract features from the text. It counts how many times a word appears in a document (corpus), and then transforms that information into a dataset.

**Data vectorization**: A process that converts non-numeric data into a numerical format so that it can be used by a machine learning model.

**Silhouette coefficient**: A score from -1 to 1 describing the clusters found during modeling. A score near zero indicates overlapping clusters, and scores less than zero indicate data points assigned to incorrect clusters. A score approaching 1 indicates successful identification of discrete non-overlapping clusters.

**Stop words**: A list of words removed by natural language processing tools when building your dataset. There is no single universal list of stop words used by all-natural language processing tools.

**Convolutional neural networks(CNN)** are a special type of neural network particularly good at processing images.

**Neural networks**: a collection of very simple models connected together.
- These simple models are called **neurons**
- the connections between these models are trainable model parameters called **weights**
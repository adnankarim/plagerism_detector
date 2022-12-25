# Plagiarism Detection, Feature Engineering

In this project, I build the a plagiarism detector that examines an answer text file and performs binary classification; labeling that file as either plagiarized or not, depending on how similar that text file is to a provided, source text.


###### First task was to create some features that can then be used to train a classification model. This task will be broken down into a few discrete steps:

* Clean and pre-process the data.
* Define features for comparing the similarity of an answer text and a source text, and extract similarity features.
* Select "good" features, by analyzing the correlations between different features.
* Create train/test .csv files that hold the relevant features and class labels for train/test data points.


## Read in the Data

This data is a slightly modified version of a dataset created by Paul Clough (Information Studies) and Mark Stevenson (Computer Science), at the University of Sheffield. You can read all about the data collection and corpus, at their university webpage.

## Types of Plagiarism
Each text file is associated with one Task (task A-E) and one Category of plagiarism, which you can see in the above DataFrame.

Tasks, A-E
Each text file contains an answer to one short question; these questions are labeled as tasks A-E. For example, Task A asks the question: "What is inheritance in object oriented programming?"

Categories of plagiarism
Each text file has an associated plagiarism label/category:

1. Plagiarized categories: cut, light, and heavy.

These categories represent different levels of plagiarized answer texts. `cut` answers copy directly from a source text, `light` answers are based on the source text but include some light rephrasing, and `heavy` answers are based on the source text, but heavily rephrased (and will likely be the most challenging kind of plagiarism to detect).

2. Non-plagiarized category: non.

`non` indicates that an answer is not plagiarized; the Wikipedia source text is not used to create this answer.

3. Special, source text category: `orig`.

This is a specific category for the original, Wikipedia source text. We will use these files only for comparison purposes.


## Text Processing & Splitting Data

Recall that the goal of this project is to build a plagiarism classifier. At it's heart, this task is a comparison text; one that looks at a given answer and a source text, compares them and predicts whether an answer has plagiarized from the source. To effectively do this comparison, and train a classifier we'll need to do a few more things: pre-process all of our text data and prepare the text files (in this case, the 95 answer files and 5 original source files) to be easily compared, and split our data into a `train` and `test` set that can be used to train a classifier and evaluate it, respectively. 

## Split data into training and test sets

The next cell will add a `Datatype` column to a given DataFrame to indicate if the record is: 
* `train` - Training data, for model training.
* `test` - Testing data, for model evaluation.
* `orig` - The task's original answer from wikipedia.

### Stratified sampling

The given code uses a helper function which you can view in the `helpers.py` file in the main project directory. This implements [stratified random sampling](https://en.wikipedia.org/wiki/Stratified_sampling) to randomly split data by task & plagiarism amount. Stratified sampling ensures that we get training and test data that is fairly evenly distributed across task & plagiarism combinations. Approximately 26% of the data is held out for testing and 74% of the data is used for training.

### Similarity Features 

One of the ways we might go about detecting plagiarism, is by computing **similarity features** that measure how similar a given answer text is as compared to the original wikipedia source text (for a specific task, a-e). The similarity features you will use are informed by [this paper on plagiarism detection](https://s3.amazonaws.com/video.udacity-data.com/topher/2019/January/5c412841_developing-a-corpus-of-plagiarised-short-answers/developing-a-corpus-of-plagiarised-short-answers.pdf). 
> In this paper, researchers created features called **containment** and **longest common subsequence**. 

Using these features as input, you will train a model to distinguish between plagiarized and not-plagiarized text files.

### Plagiarism Detection Model

Now that you've created training and test data, you are ready to define and train a model. Your goal in this notebook, will be to train a binary classification model that learns to label an answer file as either plagiarized or not, based on the features you provide the model.

This task will be broken down into a few discrete steps:

* Upload your data to S3.
* Define a binary classification model and a training script.
* Train your model and deploy it.

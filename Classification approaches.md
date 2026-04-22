## Approach 1: Text classification using TfIdf w/ scikit
- We have 3 different classes, positive, negative, and mixed
- Used an LLM to generate 300 fake tweets for testing for each candidate
- Data is cleaned
	- removing any rows with na or nan, classes are converted to strings
	- removing any classes in the training data that are not 0, -1, or 1.
1) Use scikit and its TfIdf vectorizer to create a basic text classifier
	- We use a RidgeClassifier, sparse_cg solver
	- Initial results are fairly poor, barely not even better than random.
	- Evaluation for Obama Classifier:
	    Accuracy: 0.38
	    Precision: [ Negative: 0.39732142857142855 | Mixed: 0.1951219512195122 | Positive: 0.4857142857142857 ]
	    Recall: [ Negative: 0.7672413793103449 | Mixed: 0.11764705882352941 | Positive: 0.14655172413793102 ]
	    F1 Score: [ Negative: 0.5235294117647059 | Mixed: 0.14678899082568808 | Positive: 0.2251655629139073 ]
	- Evaluation for Romney Classifier:
		Accuracy: 0.43666666666666665
		Precision: [ Negative: 0.42857142857142855 | Mixed: 0.2962962962962963 | Positive: 0.7142857142857143 ]
		Recall: [ Negative: 0.8852459016393442 | Mixed: 0.13333333333333333 | Positive: 0.1271186440677966 ]
		F1 Score: [ Negative: 0.5775401069518716 | Mixed: 0.1839080459770115 | Positive: 0.2158273381294964 ]
	- Playing with the classifier parameters, we can get around 60 percent classification accuracy, with better precision, recall, and f-scores. However, there is a possibility of overfitting the data.
# Approach 2: Deep Learning sentiment analysis w/ TensorFlow
- Scikit approach was good but needed lots of tweaking with the "bag of words" approach. If we can do a deep learning method maybe we can get better results without much effort.
- This time we're going to do a deep learning method but split the training data into test and validation sets.
- Start off by looking at sentiment analysis with text data https://www.tensorflow.org/tutorials/keras/text_classification
	- We adapt the tutorial to work with the tweet data by using a multiclass loss function
	- We modify the NN params too
	- Initial accuracy is just okay, just a little bit better than random. We can definitely improve this
- ### Improving the sentiment analysis model
	- I initially tried looking at stopwords - actually bad idea we kind of need these words for sentiment analysis. These are only good for web retrieval / information search - many stop words lists include negation words which absolutely modify the sentiment analysis.
	- I figured maybe the tweet dataset is imbalanced - it's actually pretty close to evenly distributed among all classes so this probably isn't an issue
	- We can try modifying how we're actually setting up the model.
	- We can also try leveraging existing LLMs to build a better sentiment analysis tool, but I would like to stick with this current deep learning approach.
	- This approach is already significantly better than the original sci-kit approach and it did not require much tweaking - the scikit model was *worse* than random initially - the naive bayes approach is pretty poor for sentiment analysis
	- Model gets around 60% accuracy which is pretty  average.
	- We can try using multiple binary classifiers to get a better accuracy.
- ### What if we try using multiple binary classifiers?
	- E.g.: We train 2 classifiers per candidate. One for positive classes, and one for negative classes. Then figure out which class is the most likely based on the outputs of both classifiers.
	- This might be slightly more effective than using 1 model for 3 classes
	- Some weird caveats
		- doing multiple binary classifiers achieves up to a .90 accuracy when using a subset of its own data as test data. However, plugging generated tweets as custom test data the accuracy drops dramatically. this might be because the generated tweets from the LLM aren't really indicative of actual tweets?
		- The value loss increases a bit but if we stop at 15 epochs then it's fine.........
		- I suppose it remains to be seen whether or not the classifier is good on the actual data during the demo time....
		- there's a lot more negative tweets about romney maybe we need to fix the class distribution
		- Maybe we need to downsample the data to get a more even distribution of classes
		- Okay I could try modifying the class distributioncd  but messing with masking actually solved most of the problem. Both classifiers get around 80% now
		- The final 

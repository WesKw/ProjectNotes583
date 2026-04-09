- We have 3 different classes, positive, negative, and mixed
- Used an LLM to generate 300 fake tweets for testing for each candidate
- Data is cleaned
	- removing any rows with na or nan, classes are converted to strings
	- removing any classes in the training data that are not 0, -1, or 1.
1) Use scikit and its TfIdf vectorizer to create a basic text classifier
	- We use a RidgeClassifier, sparse_cg solver
	- Initial results are fairly poor, barely better than random.
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
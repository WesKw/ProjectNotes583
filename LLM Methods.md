- Want to write 3 different prompts to classify tweets into positive, negative, and mixed classes
- Used Claude w/ Sonnet 4.6 model
- ### Prompts:
	- 1) I've given you a training data file "training data file" containing real tweets about the 2012 presidential candidates Barack Obama and Mitt Romney. That file contains the date, time, a tweet, and whether or not it was positive, negative, or mixed. I've also given you a test data file "test data file" containing more tweets about each candidate. Use the tweets in the training data file to classify the tweets for each candidate as positive, negative, or mixed. Place the results into a xlsx file, with one sheet for each candidate, where the first column is the tweet, and the second column is the class. 
		- Claude immediately noticed that the test data already has class labels. Oops.
		- Also it noticed that the training data had 4 classes
		- How did this prompt perform?
		- The prompt is using scikit to train a classifier on the training data (this is basically what I'm doing for the first part of the project.)
		- Accuracy for Obama was 0.54, for Romney was 0.47.
		- We can probably do better.
	- 2) I'm giving you a file called 'test-data.xlsx'. There are 2 sheets, one containing tweets for the 2012 presidential candidate Barack Obama, and another containing tweets for the 2012 presidential candidate Mitt Romney. Assume the Class column in the sheet does not exist. Read each tweet for the candidate and classify it as either a "Positive", "Negative", or "Mixed" tweet about the presidential candidate. Output your results into an xlsx file, with one sheet per candidate, where the first column is the tweet named "Tweet", and the second is the classification named "Class". Make sure the tweets in the output file are in the same order as the input file.
		- Immediate response gave accuracy that might be *too* good (like 98%). Will try again but physically remove the class column from the test dataset.
		- Uses anthropic API to classify the tweets
		- I'm noticing that if you give it training data it actually does worse because the model uses more traditional means of classifying text data rather than use using the LLM itself.
		- Removing the class data decreased the accuracy. 
			Accuracy for Obama: 0.79
			Accuracy for Romney: 0.83
		- This one has been the most accurate so far.
	- 3) Read the tweets in each sheet in the input data file and classify them as positive, negative, or mixed about the subject. Assume the "Class" is not there. Save the results in an xlsx file, where the first column is the "Tweet", and the second is the "Class".
		- What if we try and do a simpler prompt? Do we get similar results? Can we use less tokens and get better classifications?

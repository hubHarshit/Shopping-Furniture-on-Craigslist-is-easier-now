# Shopping-Furniture-on-Craigslist-is-easier-now
Data Analysis 

 

Data Collection 

 

For the data collection portion, we utilized the opensource package python-craigslist which is a simple wrapper for Craigslist by Julio Malegria.  We used this to search for all the posting web addresses in the furniture category in greater Lafayette and Indianapolis.  This gave us the listing URLs for about 1800 listings.  We then when through the list requested each webpage with only a simple uniform distribution sleep timer of between 2 to 3 seconds.  We never faced a rate limit issue from Craigslist using this approach.  So, from this we were able to pull from every listing: Listing_ID, region, title, description, and the list of the listing image URL links.   

 

Using PIL we requested and downloaded each image from each listings image list.  This was performed with no wait time in between requests and caused us no rate limits or 403’s.  We then additionally supplemented this image data by using a pre-tagged dataset found on Kaggle. 

 

For the text data we had to clear out some of the encoding issues that happen when dealing with text data from the web.  We cleaned the text for all PHP and HTML tag issues as well as encoding issues. 

 

There are three parts to the model in order to automatically generate tags for the advertisement posting. The model goes through the convolutional neural network, natural language processing, and an ensemble stage. 

 

Convolutional Neural Network (CNN) 

 

For image classification, we used Convolutional Neural Network. We implemented the transfer layer technique and added 2 dense layers on top of it to construct a model for furniture. Xception Model is a depth wise CNN that makes tensor calculations faster by multiplying channels one at a time rather than all at once. The initial layer, Xception, was frozen to avoid re-training it, followed by the two dense layers which we added and trained our dataset. Our training dataset comprises of two sets, the images we scraped from Craigslist and a furniture dataset we found online. After manually labeling this dataset, we fed it to our neural network for it to be trained. We used ReLU activation for our first dense layer and softmax function to vectors of probabilities. We used ImageDataGenerator from Keras to generate batches of tensor image data with image data augmentation. We then made predictions using our trained model on our test images. 

 

 

  

 

 

 

Natural Language Processing (NLP) 

 

The NLP part of the model looks at the title and description of the advertisement postings and predicts which predefined furniture tag this post belongs to through Random Forest algorithm. This part of the model was created in order to prevent misclassification from a post that lacks relevant pictures. For example, if a post does not have any pictures or has a picture of an unassembled chair, the CNN model will not provide an accurate tag. This is a potential scenario where the NLP can be a great substitute and reinforcement to the CNN model. 

 

The NLP part goes through below steps: 

Fixes the PHP and HTML parsing issues 

Fixes the Latin encoding issue 

Replaces punctuation like ’ , which will help fix some spelling and grammar issues 

Combines the title and description into one string as a data. 

Preprocess the data through removing the stop words and lemmatizing the tokens. 

Concatenate the lemmatized tokens and convert it to TF-IDF matrix as a feature value for model training. 

Predicts the predefined furniture category labels using Random Forest algorithm. 

 

This part of the model will successfully solve the issues of misclassification of the posts with lack of relevant pictures through analyzing the words used in the title and description and extracting the necessary information, preparing to compare with the predefined categories of furniture. Just the NLP part alone provides 88.46 percent accuracy based on the scrapped test data from Craigslist furniture section. 

 

Ensemble 

Once we have 2 models working, we need to bring them together.  We do this by looking at the prediction score that each model assigns to their highest prediction value.  Once we have the top prediction from each model and their respective probability, we select the prediction label that has the highest probability.  This probability can be changed by assigning weights if one model is to be preferred over another. 


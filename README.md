# Food-Chat-Bot-System-

## Problem Statement

Developed a chat bot system which will generate top restaurants for a particular cuisine and location. It is based on RASA framework. Zomato API is used for search results to be displayed to the user and is deployed on the Slack platform using ngrok application. 

Note: Rasa environment is created in anaconda. 

## Overview of RASA Framework 


![Capture](https://user-images.githubusercontent.com/36281158/89122024-8c9c3900-d4e1-11ea-8c7c-5a0965bb2bb8.PNG)

Two important parts of the RASA Framework are RASA NLU and RASA Core

The user sends the message to the bot, RASA NLU finds the intent and entities in the message and then it is stored and passed to the RASA stack. Then RASA stack decides whether a search action is needed using Zomato API or RASA core dialgue management system is needed to directly resolve the query. 

### RASA NLU 

RASA NLU has two main important functions to do; to look for the intent and entities provided by the user in the message. Intent is what the user is looking for or the purpose of using the chat bot. It can be simple "hi", "goodbye" or in our case "restaurant search". Entities are the information provided by the user which will help to solve the query. In our case it is location where the user wants to search and cuisine what the user wants to eat. 

For training the RASA NLU, two files are needed; nlu_data.md and nlu_config.yml.

##### nlu_data.md 

Here we define all the intents, the examples for training so that the RASA NLU will be abe to detect the user intent and entitites. 

![Capture1](https://user-images.githubusercontent.com/36281158/89122483-16013a80-d4e5-11ea-80aa-48613b7b1653.PNG)

![Capture2](https://user-images.githubusercontent.com/36281158/89122485-18639480-d4e5-11ea-8861-feee6d827307.PNG)

We have defined 6 intents; greet, goodbye, thanks, affirm, deny and restaurant search. So any of the words found in the user message, it will detect that intent. 


##### nlu_config.yml 

![Capture3](https://user-images.githubusercontent.com/36281158/89122589-eacb1b00-d4e5-11ea-9bc9-09c52df47c5d.PNG)

The language used is english. We will be using spacy_sklearn pipeline. It works really for small datasets. It has the following parts. Here the order has to be maintained while defining the below parts  

nlp_spacy: Spacy language initializer, it outputs nothing

tokenizer_spacy: 	Creates tokens of the user message using the spacy tokenizer, it outputs nothing 

intent_entity_featurizer_regex:	Regex feature creation to support intent and entity classification.During training, the regex intent featurizer creates a list of regular expressions defined in the training data format. If an expression is found in the input, a feature will be set, that will later be fed into intent classifier / entity extractor to simplify classification (assuming the classifier has learned during the training phase, that this set feature indicates a certain intent).

intent_featurizer_spacy: Creates feature for intent classification using the spacy featurizer

ner_spacy: SpaCy has excellent pre-trained named-entity recognisers, works really well if you have less data 

ner_crf: Resposible for entity extraction from the text 

ner_synonyms: For different messages, if the intent is same although asked in a different way this will ensure they are mapped to the same intent. 

intent_classifier_sklearn: Classifies the intent using linear SVM model and ranks them according the confidence score. Outputs intent and intent ranking 


#### Training 

Following commands are used for traning the model.

python -m rasa_nlu.train -c nlu_config.yml --data data/nlu_data.md -o models --fixed_model_name nlu --project current --verbose

Here the model is trained using nlu_config.yml, picking up the data from data/data.md , fixed means everytime the model is trained it will overwrite the previos one and will be named as "nlu" in the project named as "current" directory. 

Example: Places to eat pizza in jaipur 

Following result is displayed. 

![Capture7](https://user-images.githubusercontent.com/36281158/89124238-0ee12900-d4f3-11ea-863e-cf950934d7f4.PNG)

Here we can see that the intent is identified as "restaurant_search" and entities as "location: jaipur" and "cuisine: pizza". Intent is identified with confidence score of 0.8 which is good and similarly entities with theri values "location: jaipur" and "cuisine:pizza" are identified. Also for every intent we can see ranking of confidence score.


### RASA Core

It can be thought of as dialogue management system. It decides what actions needed to be taken and what message to be replied back to the user.

For training the model domain.yml, stories.md and policy.yml files are needed .

##### Domain.yml: It contains the all the things the bot should know. 

Slots: Acts as bot memory. Store the useful information like location, cuisine from the query sent by the user.

Actions: What action needs to be taken next.

Templates: The text that will be replied back to the user.

![Capture5](https://user-images.githubusercontent.com/36281158/89123767-7f864680-d4ef-11ea-8260-38f5d651a012.PNG)


##### Stories.yml: It contains paths that the conversation can takes place. More you defined the paths, more the better. 

![Capture4](https://user-images.githubusercontent.com/36281158/89123796-beb49780-d4ef-11ea-8782-d88b8087f7c1.PNG)
![Capture12](https://user-images.githubusercontent.com/36281158/89123806-d1c76780-d4ef-11ea-8d3b-7c5fb9f003bd.PNG)

##### policy.yml :Decides which action to take at every step in the conversation

![Capture9](https://user-images.githubusercontent.com/36281158/89124466-ca568d00-d4f4-11ea-99c8-317612296cb2.PNG)

Keras Policy: Uses a neural network implemented in Keras to select the next action. The default architecture is based on an LSTM but can be modified by editing the keras_policy.py file. 

Two variables; epochs and max_history

Epochs: Number of forward and backward pass through the entire training data 

Max_history: number of conversation in previous time to decide what to reply next to the user. In short keeping the context of the conversation. 


Memorization Policy: The MemoizationPolicy just memorizes the conversations in your training data. It predicts the next action with confidence 1.0 if this exact conversation exists in the training data, otherwise it predicts None with confidence 0.0

FallbackPolicy: It basically is a policy if the nlu_threshold or core_threshold confidence score are below 0.3, then an default action (eg.  "Soory I didn't get that" will be replied back to the user) will take place.

#### Training 

For training the model, following commands are used 

python -m rasa_core.train -d domain.yml -s data/stories.md -o models/current/dialogue -c policies.yml

![Capture8](https://user-images.githubusercontent.com/36281158/89125171-17892d80-d4fa-11ea-9134-b1625c8ffe13.PNG)


![Capture10](https://user-images.githubusercontent.com/36281158/89125174-18ba5a80-d4fa-11ea-958b-40261b321e4a.PNG)


On asking for places to eat pizza in jaipur, it displays the top 5 restaurants 

### Deployment over Slack platform 

ngrok is used as it acts as a tunnel for our local host machine to the internet. So any information that over the slack user provides it go through ngrok then through our RASA core and make necessary predictions and those predictions are displayed over the slack. Webhook is used to connect two applications (Slack and RASA model). It helps in delivering real time information to and from the user .  

Steps done are as follows.

1. Go to the https://api.slack.com/apps

2. Login your workspace and go to "create an app"

3. Add a name to your bot and workspace you want your chatbot to install in.

4. To install it , you have to ad features to your bot, got to "Bot" tab in "Add features and functionality tab"

5. There you can add features according to your choice, go to OAuth & Permissions, under scopes for both bot token scopes and user token scopes, add relevant functionality and save the changes

6. Click on install the app, it will generate Bot User OAuth Access Token, which will be used in slack credentials file 

7. Start the action server

   python -m rasa_core_sdk.endpoint --actions actions
  
8.  Deploy action server on the internet, here on running the action server we will get our port number (5055)

    ngrok http 5055

9. Activate the rasa environment

   conda activate rasa

10.  Start the rasa core server:

   python -m rasa_core.run -d models/current/dialogue -u models/current/nlu --port 5002 --connector slack --credentials slack_credentials.yml --endpoints endpoints.yml

11. Deploy rasa core server on the internet: 
   ngrok http 5002

12. Attach the url got by running ngrok in previous step by going to "Event subscription tab" on the left side in slack and attach the url followed by "/webhooks/slack/webhook"

13 Go to your workspace and see the foodbot is present. Yu can add extra features to your bot like its desciption, showing it online all the time.

![Capture11](https://user-images.githubusercontent.com/36281158/89129326-2d0e4f80-d51a-11ea-9328-184e58ef72f4.PNG)



### Features that can be added to the bot 

1. Category: In Zomato API, there is category section similarly to the location . Here various categories are defined such as Delievery, Takeaway, nightlife. For more customization, while picking location and cuisine, category can also be pciked up and it will filter the results according to the category given by the user. 

2. Timings : We can also the display the timings of a particular restaurant is opened by extracting the "timings" from the restaurant details 









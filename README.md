# Food-Chat-Bot-System-

## Problem Statement

Developed a chat bot system which will generate top restaurants for a particular cuisine and location. It is based on RASA framework. Zomato API is used for search results to be displayed to the user and is deployed on the Slack platform using ngrok application. 

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


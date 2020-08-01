# Food-Chat-Bot-System-

## Purpose of the Project 

Here I had developed a chatbot that will tell you top 5 cafes/ restros for a particular cuisine and location. It is based on RASA framework. The model uses ZOMATO API for results and is deployed on SLACK platform. 


## Problem Statement

Create a basic chatbot that:

Can search for places to eat.

Get info like cuisines, address etc.

Support small talk (chit-chat) with the user.

Remember some context of the conversation.

Take feedback and start again if needed.



## Overview of the RASA Framework 

![Capture](https://user-images.githubusercontent.com/36281158/89032872-326d6d80-d353-11ea-904e-05c4402abb11.PNG)


### Explaination:

Here the user sends message " What are good some biryani places in chennai". Now first it go throughs RASA NLU which finds out what the user is looking for (restaurant search) and what other details the user provided (biryani, chennai). Then this inforamtion is stored and relevant details are retrieved from ZOMATO through an API. RASA Core is a dialogue management system which tries to engage the user by asking relevant questions like did it help, goodbye, etc. 


#### RASA NLU 

This part of the framework looks out what the user is looking for (intent) and what other details are provided (entities). Here the intent can be simple "hi", "looking for the restaurant", "goodbye" etc. 

![Capture1](https://user-images.githubusercontent.com/36281158/89034052-d22bfb00-d355-11ea-9548-02d3ef67f351.PNG)

Now for every intent certain key words are provided in the training so that if detected in the user message, NLU will be able to find the intent. For restaurant search, below are the examples provided for training. 

![Capture2](https://user-images.githubusercontent.com/36281158/89034360-6a29e480-d356-11ea-8e02-a59f84de0957.PNG)

Here "cuisine" and "location" are the entities defined. These information will be used further. 

All the intents and theie keywords are stored in a file called "nlu_data.md" 

Following are the intents

- goodbye 

- greet

- thanks

- affirm

- deny

- restaurant_search

There is also a NLU config file for preprocessing and finding intents and enitites. 

![Capture3](https://user-images.githubusercontent.com/36281158/89036846-1372d980-d35b-11ea-9065-99f57d0e40c0.PNG)

We use sklearn spacy pipeline as it is recommended as it has a pretrained GLOVE vector embedding feature and works really well for small training examples. 

##### Training 

For training the nlu model following command is used :

Only the nlu_data.md and nlu_config files are used for training.

python -m rasa_nlu.train -c nlu_config.yml --data data/nlu_data.md -o models --fixed_model_name nlu --project current --verbose

Here, rasa_nlu train model is used, nlu_config file which contains piplines to be used for, data is taken from data.md filw which contains intents and entities and the model is saved in "models" folder with the name "nlu" -> "current". Every time the model is trained with some changes in the training data, it will overwrite the previos one. 
 
![Capture6](https://user-images.githubusercontent.com/36281158/89103016-41741e80-d42c-11ea-87ec-882852c04f11.PNG)

Now the model is trained, by following commands, when we send the message " places to eat pizza in jaipur", we get the result as shown below

Opening python interpreter,

>>from rasa_nlu.model import Interpreter
>>nlu_model = Interpreter.load("./models/current/nlu")

Sending the above message through parse function, 

>>nlu_model.parse("places to eat pizza in jaipur")
 
Result: 

![Capture7](https://user-images.githubusercontent.com/36281158/89103199-a1b79000-d42d-11ea-82c9-5cb64f5df196.PNG)

Above we can se the intenet was identified as "restaurant_search" with over 80 % confidence and "location" and "cuisine" are identified as entities. Alos for every intent it gives the confidence score as well

 
#### RASA Core 

It is the dialogue management system of the chatbot. It makes the conversation live by asking the user relevant questions based on the contextual inforamtion from the user. 

It has following parts.

1. Storyline 

![Capture4](https://user-images.githubusercontent.com/36281158/89043461-23dc8180-d366-11ea-8a88-503d4ea95d6d.PNG)

It basically what are the various path the conversation can take place

2. Domain

It basically has all the information about the intents, actions, entities , slots, templates

Slots: They acts as a memory for the bot. If the user specifies location and cuisine in the text, it will store that data that can be used further 

Templates: These are the texts which the bot which display to the user 


![Capture5](https://user-images.githubusercontent.com/36281158/89043914-dc0a2a00-d366-11ea-9c9f-b735c79f7e28.PNG)

##### Training 

For training purposes, stories.md, policies.yml, and domain.py files will be used 
python -m rasa_core.train -d domain.yml -s data/stories.md -o models/current/dialogue -c policies.yml

A little about Policy:
The rasa core policies decide which action to take at every step in the conversation. There are different policies to choose from, and one can include multiple policies in a single rasa core Agent but at every turn, the policy which predicts the next action with the highest confidence will be used. The fallback policy comes in to picture when ‘nlu_threshold’ & ‘core_threshold’ meets the levels defined in the policy which means that bot is not able to understand the user message and it responds with ‘utter_default’.


![Capture9](https://user-images.githubusercontent.com/36281158/89104004-a121f800-d433-11ea-9c64-9e5c5ad19dce.PNG)

Keras Policy: Number of epochs= 100 and max_history =5 

![Capture8](https://user-images.githubusercontent.com/36281158/89103893-f14c8a80-d432-11ea-90ec-8b634f5f9fdd.PNG)


As we can see the model is trained. By default it is one LSTM layer, one dense layer and an activation layer. 

To talk to the chatbot, we can run the below command

python -m rasa_core.run -d models/current/dialogue -u models/current/nlu --endpoints endpoints.yml

![Capture10](https://user-images.githubusercontent.com/36281158/89105782-d5041a00-d441-11ea-892d-1d72ae1d5b80.PNG)

Above is the snippet which displays the top 5 results on being asked for places to eat pizza in jaipur. 


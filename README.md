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



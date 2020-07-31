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

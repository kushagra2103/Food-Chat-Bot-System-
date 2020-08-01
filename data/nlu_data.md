<!--- Make sure to update this training data file with more training examples from https://forum.rasa.com/t/rasa-starter-pack/704 --> 

## intent:goodbye  
- Bye-bye
- Farewell
- Cheerio
- See you
- I’m out
- Take care
- Gotta go!
- Good night
- See you later
- Keep in touch
- See you soon
- See you next time
- Have a good (nice) day	
- Goodbye
- See you later
- Bye bot
- Goodbye friend
- bye
- bye for now
- catch you later
- end
- finish


## intent:greet
- Hi
- Hey
- Hi bot
- Hey bot
- Hello
- Good morning
- hi again
- hey there
- yo
- hi pal
- anybody there
- who are you 
- what are you
- hi robot 
- hey robot
- hi mister
- hello mister
- howdy 


## intent:thanks
- Thanks
- Thank you
- Thank you so much
- Thanks bot
- Thanks for that
- alright 
- thanks friend
- thanks pal
- thanks robot
- thanks for the help 
- cheers !
- cheers bro
- yes, thanks !
- amazing, thanks !
- cool, thanks 

## intent:affirm
- y
- Y
- yes
- yes sure
- absolutely
- yeah 
- right 
- yes right 
- it is right 
- correct
- it is correct
- correct right 
- yes yes 
- definetly 


## intent:deny
- no
- never
- I don't think so
- not 
- it is not 
- wrong 
- it is wrong 
- not correct
- it is not correct 
- nope
- nopes
- it is not right 
- n
- N

## intent:restaurant_search
- i'm looking for a place to eat
- I want to grab a lunch
- I am searching for a dinner spot
- i'm looking for a place in the [north](location) of town
- show me [chinese](cuisine) restaurants
- show me a [mexican](cuisine) place in the [centre](location)
- i am looking for an [indian](cuisine) spot
- search for restaurants
- anywhere in the [west](location)
- anywhere near [18328](location)
- I am looking for [asian fusion](cuisine) food
- I am looking a restaurant in [29432](location)
- I am hungry
- Can you tell me a place to eat?
- I need food
- Food near [delhi](location)
- Food near me
- cafe near me 
- what are some good restraunts in [paris](location)
- suggest me some good [chinese](cuisine) place
- suggest me a good place to eat
- where can i get best [mughlai](cuisine)
- where can I find best [burger](cuisine) in [tokyo](location)?
- best [biryani](cuisine) in [hyderabad](location)?
- i want to eat [pasta](cuisine)
- best [burgers](cuisine) in [delhi](location)
- what are the best [pizza](cuisine) in [delhi](location)
- i feel like having [sushi](cuisine)
- i would like to have some [biryani](cuisine)
- best [pizza](cuisine) near me
- i am feeling hungry
- tell me the nearest [burger](cuisine) place
- i want a [sandwich](cuisine)
- where can i get [pizza](cuisine) in [chennai](location)
- can you recommend some good [continental](cuisine) place in [mysore](location)
- Actually, I want to eat [pasta](cuisine) in [chennai](location)
- can you suggest good [biryani](cuisine) place in [chennai](location)?
- [dosa](cuisine) in [gurugram](location)
- [dosa](cuisine) in [gurgaon](location)
- best cafes in [Jaipur](location)
- nice food outlets near me 
- nice cafes around 




## lookup:location
data/locations.txt

## lookup:cuisine
data/cuisines.txt
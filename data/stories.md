## path 1

* greet
 - utter_greet
* restaurant_search{"location":"delhi", "cuisine":"pasta"}
 - slot{"location":"delhi"}
 - slot{"cuisine":"pasta"}
 - action_restaurant_search
 - utter_did_that_help
* affirm or thanks
 - utter_gratitude
* goodbye
 - utter_goodbye



## path 2
* restaurant_search{"location":"delhi", "cuisine":"pasta"}
 - slot{"location":"delhi"}
 - slot{"cuisine":"pasta"}
 - action_restaurant_search
 - utter_did_that_help
* affirm or thanks
 - utter_gratitude
* goodbye
 - utter_goodbye

## path 3 
* greet
 - utter_greet
* restaurant_search{"location":"delhi", "cuisine":"pasta"}
 - slot{"location":"delhi"}
 - slot{"cuisine":"pasta"}
 - action_restaurant_search
 - utter_did_that_help
* deny
 - utter_ask_again
* restaurant_search{"location":"delhi", "cuisine":"pasta"}
 - slot{"location":"delhi"}
 - slot{"cuisine":"pasta"}
 - action_restaurant_search
 - utter_did_that_help
* affirm or thanks
 - utter_gratitude
* goodbye
 - utter_goodbye

## path 4 
* greet
 - utter_greet

## path 5
 * goodbye
  - utter_goodbye

## path 6 
* restaurant_search{"location":"delhi", "cuisine":"pasta"}
 - slot{"location":"delhi"}
 - slot{"cuisine":"pasta"}
 - action_restaurant_search
 - utter_did_that_help
* deny
 - utter_ask_again
* restaurant_search{"location":"delhi", "cuisine":"pasta"}
 - slot{"location":"delhi"}
 - slot{"cuisine":"pasta"}
 - action_restaurant_search
 - utter_did_that_help
* affirm or thanks
 - utter_gratitude
* goodbye
 - utter_goodbye


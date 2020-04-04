---
layout: post
title:      "My CLI Data Gem Portfolio Project "
date:       2020-04-03 20:17:40 -0400
permalink:  my_cli_data_gem_portfolio_project
---


I had a lot of different ideas for my CLI data gem portfolio project but I decided to keep it simple and make a gem that scrapes information about dog breeds from "https://dogtime.com". Scraping the information off this site was quite simple. 

The interface works by greeting you and asking what the first letter of the dog breed is you'd like to know more about. Once you've entered the letter, it displays a list of dog breeds that start with that letter with an index number to the left of those breeds. You're then prompted to enter the index number of the breed to learn more about it. 

My code works by creating an instance of the CLI class and using the "call" method in my executable file located in the "/bin" file. The "call" method has multiple other instance methods that belong to the CLI class. Those of those functions include: 

*getbreedlist*
      
This method works by getting input from the user and then, if the input isn't equal to "exit", it creates an instance of the BreedList class using the user input to instantiate it and storing it in the @letter instance attribute. It then uses a BreedList instance method that uses a class method of the Scraper class to create a list of breeds from the website using the @letter of the BreedList instance and storing that list of breeds in a @list attribute, which was originally an empty hash upon initialization of the BreedList.
			
*displaylist*

This method works by taking the new instance of the BreedList class created in the get_breed_list instance, and displaying that list with index IF AND ONLY IF the @list is not equal to an empty hash. It uses a couple of instance methods that belong to the BreedList class to achieve this. One of the instance methods creates instances of the Breed class.

*getbreedinfo*
 
 This method takes in user input asking what dog breed they would like to know more about by entering the index number of the breed. It converts the user input to an integer and subtracts one because the choices start from one and not zero. If the number from the input is less than the length of the however many dog breeds are listed then it will display information about that dog breed. If not, it will prompt the user to input the index number again. 
 
*programlooptext*

This method asks the user if the would like to continue researching dog breeds or "exit". 

*userdecision*

This method takes in user input and either calls the "reset_call" method by typing in "start" or exits the program by typing in "exit". If the input does not align with either of these options you will be asked again for your input. 

**In conclusion**

Overall, the program works as exactly as I'd hoped it would, although I believe there is some room for improvement in the formatting of the program and possibly some unneeded instance methods in some of the classes.  

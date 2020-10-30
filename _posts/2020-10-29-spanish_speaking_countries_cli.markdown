---
layout: post
title:      "Spanish Speaking Countries CLI"
date:       2020-10-30 03:21:05 +0000
permalink:  spanish_speaking_countries_cli
---


## About the Program & Structure
A Command Line Interface (CLI) program is one that interfaces with a user via their computer terminal. The files are communicating via ./environment.rb file and the program is executed using bin/run to create an instance of the CLI class and call the #start method on that new instance. The program prompts the user for input and loops through the program based on the input until exiting. The CLI program I’ve built displays properties of countries that speak Spanish and creates a 3 question quiz on country capitals. The Ruby Gems utilized for this program are ‘pry’ for debugging, ‘net/http’ to call API and receive data response, ‘json’ to parse the response into a readable array, and ‘colorize’ to add color to terminal output. 

## Getting Data to Work With
The country data is gathered within the API class using the Net/HTTP gem to get the data from the [restcountries.eu](https://restcountries.eu/) API. The data stored as an array_of_countries that is parsed into JSON with the JSON gem. Instances of the Country class are created by iterating through the array_of_countries and extracting the desired country properties to store as instance attributes. The Country class adds all instances to an array when initialized and exposes that array to outside the class. 

## Let’s Get on with the Program
The CLI class is where the program logic is stored. To start, the user is greeted while the API class is called to pull the data and create instances of the Country class. The CLI instance counter is established at zero and a call is made to #main_menu to begin the program.

#main_menu is the first level menu of the program. The user is prompted with different messages based on if the counter is at 0, is odd, or even. The counter at 0 means it is the first run of the program, if the counter is odd that means the program is coming from the country data, and if the counter is even that means the program is coming from the capitals quiz. 

Next, the method will take in the user’s input and standardize it to remove whitespace and be lowercase. If the user input has more than 1 character, an error message will appear and prompt the user for the input again to exit the error loop. With the single character user input, the program is directed to either the directory of Spanish speaking countries, the capitals quiz, or exiting the program. 

### Menu Logic: Country Directory
If the directory is chosen with either “l” for learn, “s” for study, or “r” for review, the counter is incremented by one and informs the user the list will be displayed. #display_list_of_countries is called, which iterates through the the array of Country instances with their index. For each country, one is added to the index and is displayed for the numbered list with the corresponding country name.

After the list of countries is displayed, #ask_for_country_selection is called. This method prompts the user with instructions interpolating the number of elements in the array of Country instances. The method then takes in user input and it is standardized to remove whitespace, be an integer, and subtract one to align with the indices of the Country instances array. 

Until the user input, converted to an integer and minus one, is between 0 and the number of elements in the Country instances array minus one, because the index starts at 0, an error message is displayed. The error message prompts the user to try again and asks for user input again to exit the error loop.

Once a valid selection is entered, the index is used to retrieve the country from the Country instances array. That country_selection is then passed as an argument to call #display_selected_country_details(). This method takes the selected country and displays data attributes of that country. If the country does not have any borders, the display will state the country is an island. Otherwise, the country’s array of borders is converted to a string with commas separating the elements. To end #display_of_country_details(), If the CLI instance counter is even, one is subtracted to make it odd and trigger the correct #main_menu method message. 

Lastly in the country directory logic, the #main_menu is recursively called. This starts the method from the top to run through it again and ask ‘Would you like to review more countries or test your knowledge of the country capitals?’.

### Menu Logic: Capitals Quiz
If the capitals quiz is chosen with either “q” for quiz, or “t” for test, the counter is incremented by two then tells the user they’re being taken to the quiz before the #build_capitals_quiz is called.

#build_capitals_quiz stores all instances of the Country class in a random order and retrieves the first 3 countries to be used for the quiz questions. Those countries_for_quiz are then passed to #start_quiz().

#start_quiz() first establishes the quiz score at zero and the arrays of correct_messages and incorrect_messages. The user is greeted by welcoming them to the quiz and providing instructions. Using the array of quiz_questions passed in, each country is iterated through.

In each country iteration the answer_string is established with the country’s capital and name. The user is then prompted for the capital of the country. The program takes in the user input and standardizes it to remove and be all lowercase. If the input matches the country’s capital, in all lowercase, correct_messages is shuffled with the first element pulled and interpolated with the answer_string, display in green. The quiz score is also incremented by one.

If the correct capital is not inputted, incorrect_messages are shuffled with the first element pulled and interpolated with the answer_string, displayed in red. Once each country is iterated through, #score_messages() is called with the final quiz score and the array of questions. The quiz_outcome is established with the quiz score out of how many questions were asked. This string is then added into the score message with additional content based on the quiz_score. Lastly, if the CLI instance counter is odd, one is subtracted to make it even and trigger the correct #main_menu method message before then calling the #main_menu method. This call completes the program loop and prevents it from exiting unexpectedly. 

## Conclusion 
I enjoyed building this CLI application to learn about and quiz on Spanish speaking countries. Successfully grabbing the data from the API is exciting and opens up ideas for different data sets to work with. The capitals quiz took a couple iterations to refactor into what it is currently. Getting an idea out there and continually improving it by making the code more refined and concise is an enjoyable part of coding. I had challenges with overcomplicating #build_capitals_quiz. I was on the path of “I need to have an array of only the country name and capital, no other attributes to use for the quiz.”, but it was much more simple to grab all of the first three elements objects and pick out the country name and capital attributes to use for the quiz. I also had challenges with where to establish the correct_messages and incorrect_messages arrays so they could be shuffled for each country question iteration. I went with establishing the message arrays to start #start_quiz before the iteration. This way in the iteration, depending on if the matching answer is given, the message array is called, shuffled, and the first element is displayed. 

For future functionality I would be interested in expanding the quiz to include more topics that are displayed with the country information. #build_capitals_quiz could stay the same for establishing the countries for the quiz and a menu could be add to prompt user for which quiz to start. Building out a multiple choice option would be useful to quiz the currency symbols and borders. 

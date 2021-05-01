---
layout: post
title:      "JavaScript Wine Cellar"
date:       2021-05-01 06:53:23 +0000
permalink:  javascript_wine_cellar
---


Building this JavaScript front-end, Rails API back-end, single page web application has brought me a range of emotions from proud to despair. Everything seems to be going well and then it’s not quite right. I thought I was finished at least three times before trying to add something, do some testing, and producing or finding more errors. I took it step by step and eventually everything was working in it’s correct sequence. This module felt different for me than the previous. Some parts made a lot of sense and others were harder to pick up on. As a beginner, I don’t feel I understand JavaScript function’s depth yet, but I would like to get there. 

## Back-end
MURCAR Cellar holds wines that can have many comments. MURCAR is the initials of my parents and their appreciation for wine inspired this application
![schema diagram](https://i.imgur.com/PPBnR8O.png)

In the Rails API, I used the FastJSON serializer to control the data being shared and reflect the wine and comments relationship. Wines can be created, read, updated, and deleted, while comments can be created, read, and deleted.
![api data](https://i.imgur.com/00qDQFG.png)

## Front-end
To initially populate the DOM, the front end starts with WineApi.fetchWines(). This method grabs all of the wines from the API, makes them into instances of the Wine class, and updates the DOM. It sets the table of wines by rendering each wine’s table row, displaying the total wines, rendering the Wine table of contents, adding the filter dropdown options, and calling CommentApi.fetchComments() to load in all the comments into their wine’s table row
![MURCAR Cellar front-end](https://i.imgur.com/vttUd5w.png)

To add a new wine, handleNewWineSubmit() calls WineApi.createWine() to make a new instance of the Wine calls and update the DOM the same way that was done when the wines were initially fetched. Everything is updated to include the newly created wine and the wine form is reset by being closed and cleared of the data that was entered.

To update a wine, handleWineUpdateSubmit() grabs the values entered in the wine form and updates the properties of the wine instance being updated. Then a call is made to WineApi.updateWine() to update the wine in the API, render the wine’s table row with the updated data, fetch the wine’s comments to added to the table row, and renders the wine table of contents for the event that the wine name, region, or year was updated. 

To delete a wine, handleWineDeletion() removes the wine’s table row from the DOM and deletes the wine from the API. An alert appears to confirm the wine was deleted and the wine total, filter options, and wine table of contents are updated to reflect the wine being removed.

New comments can be added to a wine. The comments have a wine_id and after they are created, they are added to the wine’s comment div in the table row. Comments can also be deleted. 

The total number of wines in the collection is displayed and updated whenever a wine is added or deleted. There is also a breakdown of wine totals from the different countries in the collection. The collection of wines can be sorted by the wine, year, price, or country and can also be filtered to show opened and unopened wines and the wines from each of the countries.

## Development
The component that gave me the most trouble was getting all of the wine’s comments to appear when a wine is added or updated. I was having issues where only some of the wines comments were not populating. I would think it was working and then it wouldn’t be. By this point my code was repetitive in some places so I decided to start from the top and wrote down all of the function sequences. I was getting confused by methods firing in the not intended order, causing errors when trying to append data to an element that didn’t exist on the DOM yet. With the help of debugger I was able to figure out what was firing when and was able to get the DOM to be rendered and updated correctly. 

I also had an issue with creating comments associated to the appropriate wine.
```
showWineCommentForm = (event) => {
   const c = new Comment({wine_id: this.id})
   c.renderCommentForm()

   const commentForm = document.querySelector(`#add-cmt-wine-${this.id}`)
   commentForm.className = "form-open"

   event.target.innerText = "Close Comment Form"
}
```
I was instantiating a new Comment in this Wine method to be able to call Comment#renderCommentForm() and then was creating another new Comment in CommentApi.createComment() with the comment’s entered values. I refactored this to renderCommentForm as a Wine method and only instantiating a new Comment in CommentApi.createComment(). The error was strange because it would only appear when I would add comments back to back and would either render blank comments or completely error out.

For future development I would like to add in breakdowns of price totals of the collection and by country. This would be in a section that's able to be opened and closed like the country totals.

Once I got all of the pieces functioning properly I felt very proud of my work. I went through stages of coming up with additional functionality, not knowing how it would be implemented, doing some googling, trying out ideas in the console, getting discouraged, repeat, and then actually getting it built. I know this app would be useful as a way to manage a personal inventory of wines and I look forward to attempting to build more JavaScript front-end Rails API back-end web applications. 


---
layout: post
title:      "Krafty Krab"
date:       2021-06-29 05:56:27 +0000
permalink:  krafty_krab
---

## About
I have been creating different kinds of crafts for many years. When I took up knitting in 2017 I wanted to start keeping track of the row and stitch counts for different garment types. With this creating the base of my spreadsheet, I began keeping track of other crafts in additional tabs. React-Redux felt like a good fit to create an application tracking craft ideas, in progress, in inventory, and (if I ever tried) crafts sold. The Krafty Krab name is inspired by Spongebob season 2 episode 22b titled Bossy Boots. Pearl wants to make some changes at the Krusty Krab and suggests different K adjectives to replace Krusty with. 

## Back-end
The Krafty Krab Rails API back-end consists of 1 table with columns for name, craft_type, description, date_started, date_completed, date_sold, price, image_url. Using the Fast JSON serializer, I added attributes for what stage the craft is in based on the dates populated. Backlog = no dates, Work In Progress = Date Started & no Date Completed, Inventory = Date Completed & no Date Sold, Sold = Date Sold. The crafts_controller is set up to read, create, and delete crafts.

```ruby
  	attribute :sold do |craft|
   		craft.date_sold != nil ? true : false
  	end
```

![JSON](https://i.imgur.com/IUAdVPgh.png)

## Front-end
The Krafty Krab React front-end is broken up into containers and components. The containers consist of the top level navigation items of Home, Crafts, Gallery, and Index. Each of these are connected to the Redux store to manage state and call fetchCrafts() on componentDidMount(). All of the front-end libraries that were installed were

```javascript
	react-redux
	redux
	redux-thunk
	react-router-dom
	redux-devtools-extension
```

fetchCrafts() utilizes thunk to be able to have the action creator be a function instead of an object. fetchCrafts() makes a fetch call to the API, turns the JSON data into craft objects, and dispatches those crafts using setCrafts() to dispatch SET_CRAFTS to reducer. This case adds all the crafts to state and calculates the total of crafts with sold prices to also add to state. 

The Home page is intended to be a dashboard with inspiration of the 3 most recently added backlog items, the total of crafts in each of the stages, and the total of crafts sold. Crafts with image_urls are added to an array, randomized, and then the first 4 images are displayed. 

From there, the Crafts page is split up into All, Backlog, Work in Progress, Inventory, and Sold crafts pages. Each page displays only the relevant fields to the craft in the stage. These pages are the only place crafts can be deleted from. Each card is connected to the store and imports deleleCraft() action creator to mapDispatchToProps. deleteCraft() makes a fetch call to the API with a delete method configObj, sends a dispatch to DELETE_CRAFT to remove the craft from state, causing a re-render. An alert is also displayed to confirm to the user the craft was deleted. 

The Crafts container defines getDaysSince() and passes it as props to WIPCard and InventoryCard. This method takes in 2 dates and calculates the number of days between them. WIPCard shows the days since the craft was started with craft.date_sold and new Date() for today’s date. InventoryCard shows the number of days it took to complete the craft based on craft.date_started and craft.date_completed.

Add Craft contains a controlled craft form with local state. Input field values are based on state and whenever a change is made to a field, setState() is called.  onSubmit, the local state is sent to createCraft() action creator. createCraft() makes a fetch call to the API with a post method configObj, turns the returned JSON into a craft object, and sends a dispatch to ADD_CRAFT to add the new craft to state. Using withRouter from react-router-dom on the CraftForm export, the CraftForm is able to call push(‘/crafts’) on the routerProps history to redirect to All Crafts without a page load. 

```javascript
	export default connect(null, {createCraft})(withRouter(CraftForm))
```

```javascript
	this.props.history.push("/crafts")
```

Gallery grabs the crafts with image_urls and displays a large version of the image. The intent of the gallery is to showcase all of the images in a larger format than the Crafts cards. Index displays all of the crafts alphabetically by name with all of the craft fields in the API. Since the Craft cards only display sub-sets of the data, I wanted a page to be able to see all of the crafts with their full details. 

## Future Development
This application was fun to make. I had to stop myself from continuing to add content and features to the site. React is a great front-end javascript library and there was great joy not having to grab each HTML element to update it. The auto re-render on state change is a gift. 

For future development goals I would like to add functionality to be able to update crafts. Another would be to expand the back-end by adding a users table to be able to build multiple profiles with their own crafts. I would also like to create a customer-facing page to have actual listings for each craft in inventory. This would be where the craft is added to a cart and purchased. I am proud of this application and will continue to use it to keep track of my crafts.

![front-end](https://i.imgur.com/vEyVQTJl.png)

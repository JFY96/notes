# Overview

Common concept in web development which stands for *Model, View, Controller*. It refers to the architecture of your code, a way to organize your application by seperating the actions into those 3 main components

# Models

- Building blocks of your database
	- Managing your data (save, fetch, etc)
	- For every entry in your DB, there will be a model that holds the details of that entry
- Responsible for representing your data (memory, files, databases,...)
	- Models define the types of information that get used by your views and controllers
- Contains data-related logic

# Views

- The component that generates tthe UI for your application
	- e.g. templating engine
- uses data supplied by a controller to display the desired information
- Shouldn't contain too much logic
- Decoupled from your application code

# Controllers

- Components that decide what view to display and what information is going to be put into it
- Connecting Models and Views, so that the two can communicate (both directions)
- Contains the "in-between" logic
- (Express) Routes
- (Express) Split across middleware functions

# General Express JS App Structure

- `Models` connect to the database to read and write data
- `Routes` to forward the supported requests (and any info encoded in request URLs) to controller functions
- `Controller` functions to get the requested data from the `models`, create an HTML page displaying the data, and return it to the user to view in the browser
- `Views` (templates) used by controllers to render the data

![MVC diagram from https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/routes](../images/mvc_1.png)
# 🚀 Express.js Web API Project

## 🎯 Project Goals

In this project, you will build a small Express.js web API to store and serve different quotes about computers, coding, and technology.

## 🛠️ Setup Instructions

To work on this project on your local machine, follow these steps:

1. **📥 Download the Project**
   - Click the “Download” button below to get the project files.

2. **🖥️ Set Up Your Environment**
   - **💻 Text Editor**: Open and work on `server.js` in a text editor of your choice. If you need recommendations or assistance in setting up an editor, refer to our article on setting up a text editor for web development. Follow along until you reach the section: “Practice: Let’s Make a Project”.
   - **🌐 Node.js**: To run your API locally, you need to have Node.js installed. For installation guidance, refer to our article on installing Node.js.

3. **📦 Install Dependencies**
   - After downloading and unzipping the project, navigate to the project folder in your terminal.
   - Run `npm install` to install Express and other dependencies.

4. **🏁 Start Your Server**
   - Once dependencies are installed, you can start your server and begin working on your API.

[📥 Download]

---
 You’ve been given some starter code in the form of a front-end site and some Express.js boilerplate. You’ll use this to build several route handlers to serve up interesting quotes. As you build out your app, test out the functionality either using our front-end or with a tool like Postman. Make sure to re-run `node server.js` as you make changes to the server, and visit `localhost:4001` in the browser to interact with the front-end.

   As you work, stop your server at any point with `Ctrl + C` in the terminal, and then restart it to see new changes in its behavior.

   In `server.js`, we’ve provided you with some imported helper functions and data:
   - A `quotes` array with some pre-populated quotes about technology. Each quote in the array has a `person` and `quote` property. You can use our array or write your own, but make sure to have at least the `person` and `quote` properties, as the front-end that we’ve provided expects each quote to have them.
   - The `getRandomElement()` function, which takes an array and returns a random element from that array.

3. Set your server to listen on the `PORT` variable.

   Once you start up the server with `node server.js`, navigate to `localhost:4001` in the browser. You’ll know things are up and running when you load the blue Quote API site in the browser.

   This diagram explains how the front-end buttons correspond to different request routes.

4. Your API should have a `GET /api/quotes/random` route. This route should send back a random quote from the quotes data. The response body should have the following shape:

   ```json
   {
     "quote": {/* quote object */}
   }
Your API should have a GET /api/quotes route. This route should return all quotes from the data if the request has no query params.

If there is a query string with a person attribute, the route should return all quotes said by the same person. For instance, the data set has multiple quotes for Grace Hopper, so GET /api/quotes?person=Grace Hopper should return an array of only those quotes. If there are no quotes for the requested person, send back an empty array.

The response body should have the following shape for all GET /api/quotes requests:

```json

{
  "quotes": [ /* Array of requested quotes */ ]
}

```
Your API should have a POST /api/quotes route for adding new quotes to the data. New quotes will be passed in a query string with two properties: quote with the quote text itself, and person with the person who is credited with saying the quote.

This route should verify that both properties exist in the request query string and send a 400 response if it does not. If all is well, this route handler should add the new quote object to the data array and send back a response with the following shape:

```json
{
  "quote": {/* new quote object */}
}
```
# SOLUTION 
```
const express = require('express');
const morgan = require('morgan');
const app = express();

const { quotes } = require('./data');
const { getRandomElement } = require('./utils');

const PORT = process.env.PORT || 4001;

// Middleware
app.use(morgan('dev'));
app.use(express.static('public'));

// GET /api/quotes/random route
app.get('/api/quotes/random', (req, res) => {
  const randomQuote = getRandomElement(quotes);
  res.json({ quote: randomQuote });
});

// GET /api/quotes route
app.get('/api/quotes', (req, res) => {
  const person = req.query.person;
  if (person) {
    const quotesByPerson = quotes.filter(quote => quote.person === person);
    res.json({ quotes: quotesByPerson });
  } else {
    res.json({ quotes: quotes });
  }
});

// POST /api/quotes route
app.post('/api/quotes', (req, res) => {
  const newQuote = {
    quote: req.query.quote,
    person: req.query.person
  };

  if (!newQuote.quote || !newQuote.person) {
    res.status(400).send('Both "quote" and "person" are required.');
  } else {
    quotes.push(newQuote);
    res.status(201).json({ quote: newQuote });
  }
});

// Start server
app.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});
Feel free to reach out if you encounter any issues or have questions regarding the setup process. Happy coding! 💻✨

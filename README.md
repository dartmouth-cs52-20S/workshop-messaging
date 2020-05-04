# CS52 Workshops:  Messaging

![](http://i.giphy.com/eUh8NINbZf9Ys.gif)

Brief motivation here as well as in presentation

## Overview

In this tutorial, you will be building a simple client-server app to send text messages via Twilio, a popular service for messaging via a REST API.

## Setup

Clone the provided repo to get started!

## Step by Step

* Explanations of the what **and** the why behind each step. Try to include:
  * higher level concepts
  * best practices

Remember to explain any notation you are using.

### Install dependencies
```javascript
yarn install
yarn upgrade --interactive
touch .env // this is where we will store our Twilio API info
yarn build // this will optimize your project for faster run times during dev
```

At this point, you can verify everything's working by running:
```javascript
yarn run dev // then go to http://localhost:3000/ in your browser
```

### Building the server-side stuff

Now, we need to install the Twilio Node.js module.  
```javascript
yarn add twilio --dev
```

Let's set up your Twilio phone number and API account. Luckily, we can play around with the Twilio API for free w/ the provided free trial.  

< ADD TWILIO ACCOUNT SET UP HERE >

Now, in your .env file, add the following (replacing the values w/ your own):  
```
TWILIO_ACCOUNT_SID=YOUR_ACCOUNT_SID
TWILIO_AUTH_TOKEN=YOUR_AUTH_TOKEN
TWILIO_PHONE_NUMBER=YOUR_TWILIO_PHONE_NUMBER
```

Next, head to your `server/index.js` file. Don't worry about the code that's there, it's just the basics to get your API server running and listening as well as an example route to demonstrate structure.  

Add the following code just below the require statements at the top:

```javascript
const client = require('twilio')(
  process.env.TWILIO_ACCOUNT_SID,
  process.env.TWILIO_AUTH_TOKEN
);
```

This stores the functionality of the Twilio module in our `client` constant.

Since we will be sending the data to this API from our React app, we need our Express app to understand and read JSON. To do this, we add the body parser module's JSON parser:  

```javascript
const app = express();
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json()); // this is the new line
app.use(pino);
```

Now that our API is setup and can understand JSON, we need to create a route that we will use to send a message:

```javascript
// add this below our 'greeting' route
app.post('/api/messages', (req, res) => {

});
```

Since we'll be responding to requests with JSON as well, add the content-type header as shown below (`res` is our 'result'):  

```javascript
app.post('/api/messages', (req, res) => {
  res.header('Content-Type', 'application/json');

});
```

Finally, we will send a message using the `client` constant we created earlier. To do so, we will utilize the `client.messages.create()` method that Twilio provides. 

(Note: this method takes a JSON object w/ at least the 'from', 'to' and 'body' parameters.)

```javascript
app.post('/api/messages', (req, res) => {
  res.header('Content-Type', 'application/json');
  client.messages
    .create({
      from: process.env.TWILIO_PHONE_NUMBER,
      to: req.body.to,      // req.body is what's passed to our API from our client 
      body: req.body.body
    })
    .then(() => {
      res.send(JSON.stringify({ success: true }));
    })
    .catch(err => {
      console.log(err);
      res.send(JSON.stringify({ success: false }));
    });
});
```

SWEET! That's it for the server-side stuff, let's jump into the client!  


### The client-side


![screen shots are helpful](img/screenshot.png)

:sunglasses: GitHub markdown files [support emoji notation](http://www.emoji-cheat-sheet.com/)

Here's a resource for [github markdown](https://guides.github.com/features/mastering-markdown/).

## Use collapsible sections when you are giving away too much code
<details>
 <summary>Click to expand!</summary>
 
 ```js
 // some code
 console.log('hi');
 ```
</details>



## Summary / What you Learned

* [ ] can be checkboxes

## Reflection

*2 questions for the workshop participants to answer (very short answer) when they submit the workshop. These should try to get at something core to the workshop, the what and the why.*

* [ ] 2 reflection questions
* [ ] 2 reflection questions


## Resources

* cite any resources
# React application with Express server

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app). Then an Express server was added in the `server` directory. The server is proxied via the `proxy` key in `package.json`.

## Using this project

Clone the project, change into the directory and install the dependencies.

```bash
git clone https://github.com/philnash/react-express-starter.git
cd react-express-starter
npm install
```

Create a `.env` file for environment variables in your server.

You can start the server on its own with the command:

```bash
npm run server
```

Run the React application on its own with the command:

```bash
npm start
```

Run both applications together with the command:

```bash
npm run dev
```

The React application will run on port 3000 and the server port 3001.

## React Twilio starter

The [twilio branch](https://github.com/philnash/react-express-starter/tree/twilio) is a similar setup but also provides endpoints with basic [Access Tokens](https://www.twilio.com/docs/iam/access-tokens) for [Twilio Programmable Chat](https://www.twilio.com/docs/chat) and [Twilio Programmable Video](https://www.twilio.com/docs/video). You can use the project as a base for building React chat or video applications.

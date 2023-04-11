## JSDR213

# Express Controllers

![controller](https://thumbs.gfycat.com/DistinctTastyFossa-size_restricted.gif)

## Overview

In this lesson, we'll be learning about Express controllers. Controllers are functions that we build to handle specific requests. Instead of handling logic inline with our routes/endpoints, we build methods to handle that logic. The aim is to organize and reuse as much code as possible.

## Objectives

- Build Controller Functions
- Visualize and establish the relationship between controllers and routes.

## Getting Started

- `Fork` & `Clone` this repository and `cd` into the directory
- Run `npm install` to install our dependencies
- Open in VSCode with `code .`
- Once we've built it out, we'll run `npm run dev` to spin up our backend server

## Setting Up the Server

Add the following to your `app.js`:

```js
const express = require('express')

// Middleware will be required here

const PORT = process.env.PORT || 3001

const app = express()

// Middleware will be used here

app.listen(PORT, () => console.log(`Server running on ${PORT}`))
```

Check your `package.json` to see that you have `cors` in your dependencies.
Let's add some middleware:

```js
const express = require('express')
const cors = require('cors')
const PORT = process.env.PORT || 3001

const app = express()

app.use(cors())
app.use(express.json())

// Routes go here

app.listen(PORT, () => console.log(`Server running on ${PORT}`))
```

Create a base route to test our server, add the following _below_ our middleware and _above_ the app.listen() on our PORT:

```js
app.get('/', (request, response) => {
  response.send({ msg: 'Server Running' })
})
```

Start your server by running `npm run dev`.

Test this endpoint in your RESTful client by _sending_ this request in Postman or Insomnia: `GET`: `http://localhost:3001/`

## What Are Controllers

Controllers are methods that we create to handle how our server behaves during a request. They are in charge of sending back the requested information for a specific endpoint. We typically group them based on the actions that they perform and for the `router` that handles an endpoint or `route`. For example, if we have a `router` that handles all requests for a user ie. `log in`, `register`, `profile` etc.. We would create a controller to handle all of these endpoints. Our controller is a group of functions that will then handle the behavior for a specific `endpoint`.

If our user requests to `log in`, we would have a controller method that handles only the log in functionality for our server.

The following diagram shows the flow during the `request`/`response` lifecycle:

![diagram](images/controller_flow.png)

We want to keep our codebase as organized as possible. This helps us to later scale by adding features or functionality. By taking a simple functional approach, if we wanted to add a new feature, we could just add a new function or controller to handle that request.

## Creating Controllers

Create a folder called controllers in the root directory of your repo:

```sh
mkdir controllers
```

Inside the controllers folder create a file called `CatController.js`.

```sh:
touch controllers/CatController.js
```

Inside of `CatController.js` we'll be creating functions that handle different HTTP requests.

Create a function called `getCats`, this function will accept a `request` and `response` as arguments/parameters.

Send a `response` with the following:

```js
const getCats = (req, res) => {
  res.send({
    message: 'Getting Cats'
  })
}
```

We now need to be able to use this function for a specific route by exporting it. At the bottom of your `CatController.js` add a `module.exports`:

```js
module.exports = {
  getCats
}
```

## Using Controllers

Now that we've created a controller, it's time to put it to use. Back in your `app.js`, create a `GET` route with an endpoint of `/cats`.

```js
app.get('/cats')
```

Let's use our `getCats` function in our `CatController.js` file.

At the top of your `app.js`, we need to `require` our controller file.
Add the following below all of our other `require`.

```js
const catController = require('./controllers/CatController.js')
```

Now in our `GET` cats endpoint use the `getCats` function, our `catController` functions were exported as an object, so we would apply this function by doing the following to your `app.get('/cats')`:

```js
app.get('/cats', catController.getCats)
```

Now let's test this endpoint. In your rest client, perform a `GET` request on: `http://localhost:3001/cats`

You should recieve a 200 status code and a JSON object:

```json
{
  "message": "Getting Cats"
}
```

You've just successfully implemented your first controller function!

Now we can start playing with real data! Lets make an array of cats and put it at the top of our CatController.js file

```js

const cats = [
  { name: "Salem",
   color: "black", 
   lovesLasagna: false },
  { name: "Garfield", 
  color: "orange", 
  lovesLasagna: true },
  { name: "Heathcliff", 
  color: "orange", 
  lovesLasagna: false },
];



```

Because we are going out of our Controllers folder and back into our Root directory, we'll need to have the extra '.' in the path.
Lets do a console.log(cats) to make sure our data is loaded up, and change our getCats function to have this data in :

```js
const getCats = (req, res) => {
  res.send(cats)
}
```

We now have what is called an Index route, which returns a List of data as an array.

Lets take this a step further by creating individual Show routes, which will give the detail of each data point.

We'll use our Params in this to create dynamic slug end points.

In server.js lets create a new route

```js

app.get('cats/:id')

```

Then create a new function in your CatController to target one individual part of the array, export it, and add it to our route!

```js

const getCat = (req, res) => {
    res.send(cats[req.params.id])
  }

```

```js
app.get('cats/:id', CatController.getCat)
```
Now we have both Index and Show routes to target all of our data, or one particular peice!

## You Do

You'll be creating your own controller functions. Once you create a function, don't forget to add it to our `module.exports`.

- Create a dogs array with any properties you'd like to include about your dog objects, a dogsController.js file inside of your controllers folder, and run the same steps as the Cats in your server.js file. Remember, you'll need to export and require your array data, and your different controllers through their respective files!

- Can we target our data by any other properties besides Id?



## Recap

In this lesson, we learned how to break up the functionality of our server into modular but effective pieces of code. By utilizing this pattern, adding features and functionality are much simpler and unintrusive. Express was meant to be lightweight and scalable, and by leveraging modular code, we can adhere to best practices and patterns.

## Resources

- [Express](https://expressjs.com/)

## Social Network API


## Description
This application is a back-end for a social network web application where users can share their thoughts, react to friends’ thoughts, and create a friend list. It uses a NoSQL database, which allows the network to handle large amounts of unstructured data and provide the ability to scale.


## Technologies Used

Node.js
    npm install -g node
Express.js
    npm install express

MongoDB
    mongod

Mongoose
    npm install mongoose


## Packages Used

express
mongoose


## Problems Solved

The application successfully creates a database schema using Mongoose and handles routes with Express.js. It includes models for Users and Thoughts, as well as subdocuments for Reactions.


## File Structure

The main server file is server.js which sets up the Express.js server and handles connection to the MongoDB database.

The routes directory contains all the route handling for the application, with a separate file for each of the main models (User and Thought).

The controllers directory contains the functionality for each route, including database queries using Mongoose.

The models directory contains the Mongoose models and schema definitions for the User, Thought, and Reaction models.


## API Requests

    GET
    To get all users or thoughts:

In Postman or Insomnia, make a GET request to http://localhost:3001/api/users or http://localhost:3001/api/thoughts.

    To get a specific user or thought:

Make a GET request to http://localhost:3001/api/users/:id or http://localhost:3001/api/thoughts/:id, replacing :id with the id of the user or thought you want to retrieve.

    POST
    To create a new user:

Make a POST request to http://localhost:3001/api/users with the following JSON in the request body:


{
    "username": "newuser",
    "email": "newuser@email.com"
}

    Thought
    To create a new thought:

Make a POST request to http://localhost:3001/api/thoughts with the following JSON in the request body, replacing :userId with the id of the user:


{
    "thoughtText": "Here's a cool thought...",
    "username": "lernantino",
    "userId": ":userId"
}

    PUT
    To update a user or thought:

Make a PUT request to http://localhost:3001/api/users/:id or http://localhost:3001/api/thoughts/:id, replacing :id with the id of the user or thought you want to update. Include the fields you want to update in the request body as JSON.

    DELETE
    To delete a user or thought:

Make a DELETE request to http://localhost:3001/api/users/:id or http://localhost:3001/api/thoughts/:id, replacing :id with the id of the user or thought you want to delete.


## Code Functionality

The application uses Mongoose, an Object Data Modeling (ODM) library, to interact with the MongoDB database. It provides a straightforward, schema-based solution to model the application data.

    GET Request
When a GET request is made, Mongoose queries the database and returns the requested data. Here is an example code snippet from the user-controller.js:

        // get all users
getAllUsers(req, res) {
    User.find({})
        .populate({
            path: 'thoughts',
            select: '-__v'
        })
        .select('-__v')
        .sort({ _id: -1 })
        .then(dbUserData => res.json(dbUserData))
        .catch(err => {
            console.log(err);
            res.status(400).json(err);
        });
},
The User.find({}) function is a Mongoose method that gets all users from the MongoDB database. If a specific id is provided, Mongoose will return that specific user or thought, otherwise it will return all users or thoughts.

    POST Request
When a POST request is made, Mongoose creates a new document in the database with the data provided in the request body. Here is an example code snippet:


        // create user
createUser({ body }, res) {
    User.create(body)
        .then(dbUserData => res.json(dbUserData))
        .catch(err => res.status(400).json(err));
},
The User.create(body) function is a Mongoose method that creates a new user in the MongoDB database with the data provided in the request body.

    PUT Request
When a PUT request is made, Mongoose updates the document with the given id with the new data provided in the request body. Here is an example code snippet:

        // update user
updateUser({ params, body }, res) {
    User.findOneAndUpdate({ _id: params.id }, body, { new: true, runValidators: true })
        .then(dbUserData => {
            if (!dbUserData) {
                res.status(404).json({ message: 'No user found with this id!' });
                return;
            }
            res.json(dbUserData);
        })
        .catch(err => res.status(400).json(err));
},
The User.findOneAndUpdate({ _id: params.id }, body, { new: true, runValidators: true }) function is a Mongoose method that updates a user in the MongoDB database with the new data provided in the request body.

    DELETE Request
When a DELETE request is made, Mongoose deletes the document with the given id from the database. Here is an example code snippet:

        // delete user
deleteUser({ params }, res) {
    User.findOneAndDelete({ _id: params.id })
        .then(dbUserData => {
            if (!dbUserData) {
                res.status(404).json({ message: 'No user found with this id!' });
                return;
            }
            res.json(dbUserData);
        })
        .catch(err => res.status(400).json(err));
},
The User.findOneAndDelete({ _id: params.id }) function is a Mongoose method that deletes a user from the MongoDB database.

## Walkthrough Video
https://drive.google.com/file/d/1agGJNWLaHgYq_RHfLseyuNQzL-EiczxS/view

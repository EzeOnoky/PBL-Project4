# PBL-Project4
## MEAN Stack Implementation

##### Challenges Encountered & Lesson Learnt
As per the Step 2: Install MongoDB, i tried following the script/steps outlined on Darey.io, but i kept getting error. So i went online, and used the steps on https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/ to cross this hurdle.

![PBL4_error1](https://user-images.githubusercontent.com/122687798/221477426-0679d8ec-54d7-4a4c-b07c-24128e9d0c38.JPG)

Lesson Learnt...On the security of my EC2 Instances, i had to open below ports and protocols before i could access the node.js server via port 3300 and also launch Putty or SSH console to test what curl command returns locally from a separate terminal log in.

![PBL4_6](https://user-images.githubusercontent.com/122687798/221551962-7329508a-dfa3-4f81-bd47-c861bbb87885.JPG)

### MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

So far, I have already learnt how to deploy LAMP, LEMP and MERN Web stacks – it is time to get familiar with MEAN stack and deploy it to Ubuntu server.

MEAN Stack is a combination of following components:
MongoDB (Document database) – Stores and allows to retrieve data.
Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
Angular (Front-end application framework) – Handles Client and Server Requests
Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user

#### Side Self Study
Refresh your knowledge of OSI model
Read about Load Balancing, get yourself familiar with different types and techniques of traffic load balancing.
Practice in editing simple web forms with HTML + CSS + JS

# Task for this Project
In this assignment I am going to implement a simple Book Register web form using MEAN stack.

## Step 1: Install NodeJs
Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

Update ubuntu
sudo apt update

Upgrade ubuntu
sudo apt upgrade

Add certificates
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

Install NodeJS
sudo apt install -y nodejs

## Step 2: Install MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

### Install MongoDB Community Edition
Follow these steps to install MongoDB Community Edition using the apt package manager.

Import the public key used by the package management system.
From a terminal, issue the following command to import the MongoDB public GPG Key from https://www.mongodb.org/static/pgp/server-6.0.asc

*wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -*

The operation should respond with an OK.

However, if you receive an error indicating that gnupg is not installed, you can:
1 - Install gnupg and its required libraries using the following command: *sudo apt-get install gnupg*
2 - Once installed, retry importing the key: *wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -*

### Create a list file for MongoDB.
Create the list file /etc/apt/sources.list.d/mongodb-org-6.0.list for your version of Ubuntu. Different scripts are used for the various versions of Ubuntu. To know your Ubuntu version,  open a terminal or shell on the host and execute *lsb_release -dc* or you use *cat /etc/lsb-release*

Below are the Ubuntu Versions that exist, The 1st on he list is what am using
Ubuntu 22.04 (Jammy)
Ubuntu 20.04 (Focal)
Ubuntu 18.04 (Bionic)
Ubuntu 16

Create the /etc/apt/sources.list.d/mongodb-org-6.0.list file for Ubuntu 22.04 (Jammy):
*echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list*

### Reload local package database.
Issue the following command to reload the local package database:
*sudo apt-get update*

### Install the MongoDB packages.
You can install either the latest stable version of MongoDB or a specific version of MongoDB.

To install the latest stable version, issue the following
*sudo apt-get install -y mongodb-org*

Optional. Although you can specify any available version of MongoDB, apt-get will upgrade the packages when a newer version becomes available. To prevent unintended upgrades, you can pin the package at the currently installed version:
*echo "mongodb-org hold" | sudo dpkg --set-selections*
*echo "mongodb-org-database hold" | sudo dpkg --set-selections*
*echo "mongodb-org-server hold" | sudo dpkg --set-selections*
*echo "mongodb-mongosh hold" | sudo dpkg --set-selections*
*echo "mongodb-org-mongos hold" | sudo dpkg --set-selections*
*echo "mongodb-org-tools hold" | sudo dpkg --set-selections*

### Install MongoDB
sudo apt install -y mongodb

### Start The server
sudo service mongodb start

### Verify that the service is up and running
sudo systemctl status mongodb

![PBL4_2](https://user-images.githubusercontent.com/122687798/221482184-2a4ab895-7d1c-4471-87f4-16df735f6aba.JPG)
*MONGODB Server was successfully started*

### Install npm – Node package manager.
*sudo apt install -y npm*

### Install body-parser package
We need ‘body-parser’ package to help us process JSON files passed in requests to the server.
*sudo npm install body-parser*

Create a folder named ‘Books’
*mkdir Books && cd Books*

In the Books directory, Initialize npm project
*npm init*

Add a file to it named server.js
*vi server.js*

Copy and paste the web server code below into the server.js file.

var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});


## Step 3: Install Express and set up routes to the server

Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.

We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.

*sudo npm install express mongoose*

In ‘Books’ folder, create a folder named apps
*mkdir apps && cd apps*

Create a file named routes.js
*vi routes.js*

Copy and paste the code below into routes.js

var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};


In the ‘apps’ folder, create a folder named models

*mkdir models && cd models*

Create a file named book.js
*vi book.js*


Copy and paste the code below into ‘book.js’

var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);

## Step 4 – Access the routes with AngularJS

AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.

Change the directory back to ‘Books’
*cd ../..*

Create a folder named public
*mkdir public && cd public*

Add a file named script.js
*vi script.js*

Copy and paste the Code below (controller configuration defined) into the script.js file.

https://www.darey.io/docs/step-3-install-express-and-set-up-routes-to-the-server/#:~:text=%3C!doctype%20html%3E%0A%3Chtml,div%3E%0A%20%20%3C/body%3E%0A%3C/html%3E


<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>



In public folder, create a file named index.html;
*vi index.html*

Copy and paste the code below into index.html file.

<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>

Change the directory back up to Books
*cd ..*

Start the server by running this command:
*node server.js*

The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

curl -s http://localhost:3300

The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

curl -s http://localhost:3300
It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.

For this – you need to open TCP port 3300 in your AWS Web Console for your EC2 Instance.

![PBL4_3](https://user-images.githubusercontent.com/122687798/221497930-ac11db8d-7a83-4ce9-a7f5-06dc73bed925.JPG)

![PBL4_4](https://user-images.githubusercontent.com/122687798/221548577-beee0b06-a5e0-42cd-8a3f-21896797a349.JPG)

![PBL4_5](https://user-images.githubusercontent.com/122687798/221548902-d506f4c6-0cee-40b2-8d06-1c8325f563c6.JPG)



## Congratulations EZE !
You have now completed all ‘PBL Progressive’ projects and are ready to move on to more complex and fun ‘PBL Professional’ projects!!!

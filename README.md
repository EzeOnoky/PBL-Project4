# PBL-Project4
## MEAN Stack Implementation

##### Challenges Encountered
As per the Step 2: Install MongoDB, i tried following the script/steps outlined on Darey.io, but i kept getting error. So i went online, and used the steps on https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/ to cross this hurdle.

![PBL4_error1](https://user-images.githubusercontent.com/122687798/221477426-0679d8ec-54d7-4a4c-b07c-24128e9d0c38.JPG)


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




Install npm – Node package manager.

sudo apt install -y npm
Install body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

sudo npm install body-parser
Create a folder named ‘Books’

mkdir Books && cd Books
In the Books directory, Initialize npm project

npm init
Add a file to it named server.js

vi server.js
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

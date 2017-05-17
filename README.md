# quizletbot

To build this project run below command in your project directory 

```javascript
npm install
```

# Tutorial: 
* You are going to need an Azure account. 
* Node.js
* Quizlet Developer Account

#### Step 1: Setup on Azure
After you have created your Azure account, it is time to create a web application for the ChatBot to run on. This is where the endpoints for communication with your bot are created.

* Go to http://portal.azure.com
* Select ‘New’
* Then ‘Web + mobile’
* And finally ‘Web App’
* Create a name for your App
* Choose your subscription
* Resource Group: choose Default
* App Service Plan: choose Default
* Click Create.
* Once your Web App is created and available in the ‘All Resources’ menu. Go to your Web Apps Overview section and find the url! Save that url somewhere because it will come in handy later.

#### Step 3: Get coding
First create a directory! In the working directory, you will need to set up the projec as a node project and then download the proper node modules.
Initialize the node project 
```
npm init
```
Install proper node modules
```
npm install --save botbuilder
```
```
npm install --save restify
```
###### If you encounter installing modules in OSX/mac try running with sudo

* Create an app.js file in your directory
* Create an another js file that will communicate with the quizlet API (in this repository, the file is called api.js)

In your app.js file you will need the following required code just to properly set up your bot:
```javascript
var restify = require('restify');
    var builder = require('botbuilder');

    //=========================================================
    // Bot Setup
    //=========================================================

    // Setup Restify Server
    var server = restify.createServer();
    server.listen(process.env.port || process.env.PORT || 3978, function () {
       console.log('%s listening to %s', server.name, server.url);
    });

    // Create chat bot
    var connector = new builder.ChatConnector({
       appId: process.env.MICROSOFT_APP_ID,
       appPassword: process.env.MICROSOFT_APP_PASSWORD
    });

    var bot = new builder.UniversalBot(connector);
    server.post('/api/messages', connector.listen());
```

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

This is just the bare bones of the bot. Before we add any dialogs, lets be sure your api file is set up correctly.

###### Quizlet API Code

In order for this bot to work we are going to make two API calls. One to retrieve the different set of user and other to retrieve the sets of data in cards. 

Make sure you have got your Quizlet API 2.0 Client ID. 

Open up api.js file and copy and paste the below code 

```javascript
exports.GetSets = function (user, callback) {
        request.get({
            uri: 'https://api.quizlet.com/2.0/users/' + user + '/sets?client_id=<ENTER CLIENT ID here>',

        },
            function (error, response, body) {
                if (error)
                    callback(error);
                else {
                    body = JSON.parse(body);
                    for (var x = 0; x < body.length; x++) {
                        if ((x + 1) == body.length) {
                            // last set
                            sets = sets + body[x].title;
                        } else {
                            sets = sets + body[x].title + ', ';
                        }
                        table[body[x].title] = body[x].id; //creating a hash table to store set names and IDs
                    }
                    console.log('Got sets');
                    exports.Sets = sets;

                }
            })
    }
    
```
Because the api.js file will be used a node module in other files, all variables and functions needed outside of this file will be exported.The GetSets function does a Get call to a Quizlet uri specific to retrieving a users study sets, with username passed into it and the client ID you received from Quizlet.( The username is passed in from outside files calling the function, so it is not a variable you need to instantiate in this file.) The body received back from the API will contain the different study sets and other information associated with them. The rest of this function will manipulate the the body so that we can store the names of the sets (in the set array) and the set IDs (which will be needed in the next function) using a hash table. Lets take a look at the GetTerms function.

```javascript

```

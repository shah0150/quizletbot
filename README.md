# quizletbot

To build this project run below command in your project directory 

```javascript
npm install
```
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

# MSTeam Connector
- A simple connector to the Microsoft Teams platform
- allows developers to send messages to a MSTeams Channel from their scripts.

## Pre-requisites
- admin access to MSTeams Instance 
- install Incoming Webhook App
- configure connectors to the needed channels
- hands on the connectors URIs

## Configure the connector

- edit the file ./config
```javascript
const team = {
    baseUrl:"https://outlook.office.com/webhook/",
    name: "Test Team",//not used for now but it will help you distinguish your apps
    channels: {
        //add all your channels 
        <ChannelName>: "<IncomingWebhookURI>",
        
    },
    defaultChannel: "general"
};
```
## Use the connector
```javascript
var mscModule = require('./MicrosoftTeams');

var mt=new mscModule.MicrosoftTeams();
var cnMngr=mt.getConnectorManager();
var myConnector=cnMngr.getConnector("general");
```
## Send Message to the channel
```javascript
var msg={
    "@context": "https://schema.org/extensions",
    "@type": "MessageCard",
    "potentialAction": [
        {
            "@type": "OpenUri",
            "name": "View All Tweets",
            "targets": [
                {
                    "os": "default",
                    "uri": "https://twitter.com/search?q=%23MicrosoftTeams"
                }
            ]
        }
    ],
    "sections": [
        {
            "facts": [
                {
                    "name": "Posted By:",
                    "value": ""
                },
                {
                    "name": "Posted At:",
                    "value": ""
                },
                {
                    "name": "Tweet:",
                    "value": ""
                }
            ],
            "text": "A tweet with #MicrosoftTeams has been posted:"
        }
    ],
    "summary": "Tweet Posted",
    "themeColor": "0072C6",
    "title": "Tweet Posted"
};

try{
    
    return myConnector.send(
    msg
);
}catch(exception){
    return exception;
}
```
- the message object is a Microsoft Message Card 
- A Card is structured using JSON. I won’t go over all permutations of a Card (as they differ between Card types) - the Microsoft Docs do a pretty good job explaining it, though - https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/cards/cards-reference
If you wish to test your JSON for a Message Card before using in Teams, I recommend using the Message Card Playground website - https://messagecardplayground.azurewebsites.net/ - it allows you to visualize the layout and view sample code.

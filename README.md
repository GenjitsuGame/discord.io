# node-discord
A low-level library for creating a Discord client from Node.js. [Come join the discussion!](https://discord.gg/0MvHMfHcTKVVmIGP)

### Warning:

**Please update to 0.1.5 immediately. A Discord update has outmoded the previous versions.**
I'd recommend updating frequently during the start of the project. I've also been told, by one of the developers, "we change it [The API] often", so I'll try to keep the updates regular.

# What you'll need
* An email and password from Discord. The client doesn't support anonymous joining.

# How to install
````javascript
npm install node-discord
````

# Example
````javascript
/*Variable area*/
var Discordbot = require('node-discord');
var bot = new Discordbot({
	username: "",
	password: "",
	autorun: true
});

/*Event area*/
bot.on("err", function(error) {
	console.log(error)
});

bot.on("ready", function(rawEvent) {
	console.log("Connected!");
	console.log("Logged in as: ");
	console.log(bot.username);
	console.log(bot.id);
	console.log("----------");
});

bot.on("message", function(user, userID, channelID, message, rawEvent) {
	console.log(user + " - " + userID);
	console.log("in " + channelID);
	console.log(message);
	console.log("----------");
	
	if (message === "ping") {
		sendMessages(channelID, ["Pong"]);
	}
});

bot.on("presence", function(user, userID, status, rawEvent) {
	/*console.log(user + " is now: " + status);*/
});

bot.on("debug", function(rawEvent) {
	/*console.log(rawEvent)*/ //Logs every event
});

bot.on("disconnected", function() {
	console.log("Bot disconnected");
	/*bot.connect()*/ //Auto reconnect
});

/*Function declaration area*/
function sendMessages(ID, messageArr, interval) {
	if (!interval) interval = 200;
	
	var messInt = setInterval(function() {
		if (messageArr.length > 0) {
			bot.sendMessage({
				to: ID,
				message: messageArr[0]
			});
			messageArr.splice(0,1);
		} else {
			clearInterval(messInt);
		}
	}, interval);
}

````

# Events
Events for the bot.

## ready
````javascript
bot.on('ready', function(rawEvent) { });
````
* **rawEvent** : The entire event received in JSON.


## message
````javascript
bot.on('message', function(user, userID, channelID, message, rawEvent) { });
````

* **user** : The user's name.
* **userID** : The user's ID.
* **channelID** : The ID of the room where the bot received the message.
* **message** : The chat message.
* **rawEvent** : The entire event received in JSON.

## presence
````javascript
bot.on('presence', function(user, userID, status, rawEvent) { });
````
* **user** : The user's name.
* **userID** : The user's ID.
* **status** : The user's status. Currently observed: ['online', 'idle', 'offline']
* **rawEvent** : The entire event received in JSON.

## debug
````javascript
bot.on('debug', function(rawEvent) { });
````
* **rawEvent** : In this section, it logs ANY event received from Discord.

## err
````javascript
bot.on('err', function(error) { });
````
* **error** : Logs the backend error (login, connection issues, etc).

## disconnected
````javascript
bot.on('disconnected', function() { });
````

# Properties

The client comes with a few properties to help your coding.
* **id**
* **username**
* **servers**
    * **[Server Choice]**
        * **channels**
        * **members**
* **directMessages**
* **internals**
    * **token**

# Methods
Methods that get the bot to do things.

## connect()
Connects to Discord.
````javascript
bot.connect()
````

## disconnect()
Disconnects from Discord and emits the "Disconnected" event.
````javascript
bot.disconnect()
````

## sendMessage(object)
````javascript
bot.sendMessage({
	to: "userID/channelID",
	message: "Hello World"
});
````
A recent Discord update now forbids you from Direct Messaging a user that does not share a server with you.

##### The following are similar in syntax. There's a function to resolve server IDs from the channel ID (assuming your bot has enough permissions to access the api, which it should, if you're doing anything below). So you can use the channel ID or server ID.

### kick(object), ban(object), unban(object), mute(object), unmute(object), deafen(object), undeafen(object)
````javascript
bot.kick({
    channel: "Server or Channel ID",
    target: "User ID"
});

//The rest are the same
````

## Special Thanks
* **[Chris92](https://github.com/Chris92de)**
    * Found out Discord's direct messaging method.
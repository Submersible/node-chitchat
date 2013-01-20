# chitchat—Real-time chatting for the web

Currently we use [SockJS](https://github.com/sockjs/sockjs-node) as a transport, but I would like to extend this project to allow any system to be plugged in.

## Server

### Setup

Install the library through NPM, dyuh!

`npm install chitchat`

### Example

```javascript
var http = require('http'),
    sockjs = require('sockjs'),
    chitchat = require('chitchat');

/* Instantiate your chitchat server */
var chat = chitchat.createServer();

/* Setup authorize function */
chat.handleAuthorize(function (data, good, bad) {
    signobj.valid(data.access_token).then(function (data) {
        good(data.username, {meta_data...});
    }).fail(function (err) {
        bad({invalid: true});
    });
});

/* Handle message function */
chat.handleMessage(function (data, good, bad) {
});

/* Hook into your HTTP server */
var server = http.createServer();
chat.installHandlers(server, {prefix: '/chat'});
server.listen(9999, '0.0.0.0');

```

### var chat = chitchat.createServer()

Create a new chat server.

### chat.handleAuthorize(fn(data, goodFn, badFn))

Handle authorization

#### data

Data to validate from the front end.

#### goodFn({String} identifier, {Object} [meta_data])

We authorized the user, you must set an identifier for this user.  You may also send some meta data to the front end for the user, perhaps a list of online users!

#### badFn({Object} [meta_data])

Could not authorize the user, you can pass meta data along to the front end to tell the user why.

----------

### chat.handleMessage(fn(data, goodFn, badFn))

Handle message sending



### chat.message({Object} user, {Object} message)

Send a custom message to a user.



## Client

Hook this into your front end chat UI!

### chitchat.connect({...})
### chitchat.disconnect()

### var state = chitchat.state()

States:

* `connected`
* `disconnected`
* `reconnecting`

You may also get a boolean value of each of these states with the following methods.

#### var cond = chitchat.isConnected()
#### var cond = chitchat.isDisconnected()
#### var cond = chitchat.isReconnecting()

### chitchat.send({message: 'rwar'})

----------

### Events

#### chitchat.on('connect', fn())

Connected to the chat server

#### chitchat.on('disconnect', fn())

Disconnected from the chat server

#### chitchat.on('reconnect', fn())

Reconnecting to the chat server, with the reason why we disconnected.

#### chitchat.on('messages', fn(messages))

Received a list of chat messages!


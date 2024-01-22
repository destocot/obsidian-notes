- Enables real-time communication between a client and server.

### Open web socket connection from the browser to the server
```
const socket = new WebSocket("ws://example.com/socket");
socket.onmessage = (event) => {
	console.log(event.data);
};
socket.send("Hello Sever!)'
```

> There are also web APIs for WebRTC (Web Real-Time Communications) API, which Provides real-time audio, video, and data communication.

### Setting up server web socket with `express-ws`
```
const wsExpress = require("express-ws")(app);
app.ws("/chat/:roomName", function (ws, req, next) {
  setInterval(() => {
    ws.send(Math.random());
  }, 5000);
});
```

### Methods on WebSocket
```
socket.onopen = (event) => {
  console.log("WEB SOCKET OPENED!!!");
};
```

```
socket.onmessage = (event) => {
  console.log("MESSAGE FROM WEBSOCKET", event.data);
};
```

```
socket.onerror = (event) => {
  console.log("SOMETHING WENT WRONG");
  console.log(event);
};
```

```
socket.onclose = (event) => {
  console.log("WEB SOCKET HAS BEEN CLOSED!!!");
};
```

### Close web socket with `express-ws`
```
ws.close()
```

### ==example== WebSocket Flow Between Client and Server

- **Server**
- create a web socket route, on connection create a new User, with the current web socket instance and the room name from the parameters
```
const wsExpress = require("express-ws")(app);
app.ws("/chat/:roomName", function (ws, req, next) {
  try {
    const user = new ChatUser(
      ws.send.bind(ws), // fn to call to message this user
      req.params.roomName // name of room for user
    );
    // register handlers for message-received, connection-closed
    ws.on("message", function (data) {
      try {
        user.handleMessage(data);
      } catch (err) { ... }
    });
    ws.on("close", function () {
      try {
        user.handleClose();
      } catch (err) { ... }
    });
  } catch (err) { ... }
});
```

- **Client**
- connect to the web socket, prompt user for a username and send the data in any way you choose
```
const socket = new WebSocket("ws://localhost:3000/chat/people");
const username = prompt("Enter your username. (no spaces)");

socket.onopen = (event) => {
  console.log("WEB SOCKET OPENED!!!");
  const data = { type: "join", name: username };
  socket.send(JSON.stringify(data));
};
```

> console.log
```
WEB SOCKET OPENED!!!
```
- **Server**
- Create User
```
class ChatUser {
  constructor(send, roomName) {
    this._send = send; // "send" function for this user
    this.room = Room.get(roomName); // room user will be in
    this.name = null; // becomes the username of the visitor

    console.log(`created chat in ${this.room.name}`);
  }

	...
}
```

- Receive message from client
```
ws.on("message", function (data) {
  try {
	user.handleMessage(data);
  } catch (err) { ... }
});
```

- Handle message based on the type
```
 handleMessage(jsonData) {
    let msg = JSON.parse(jsonData);

    if (msg.type === "join") this.handleJoin(msg.name);
    else if (msg.type === "chat") this.handleChat(msg.text);
    else throw new Error(`bad message: ${msg.type}`);
  }
```

- Return message to client with `broadcast` method
```
  handleJoin(name) {
    this.name = name;
    this.room.join(this);
    this.room.broadcast({
      type: "note",
      text: `${this.name} joined "${this.room.name}".`,
    });
  }
```

```
broadcast(data) {
	for (let member of this.members) {
	  member.send(JSON.stringify(data));
	}
}
```

- **Client**
- receive message back
```
socket.onmessage = (event) => {
  console.log("MESSAGE FROM WEBSOCKET", event.data);
};
```

> console.log
```
MESSAGE FROM WEBSOCKET {"type":"note","text":"Colt joined \"people\"."}
```


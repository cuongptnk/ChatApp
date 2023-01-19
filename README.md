# ChatApp
Implement Chat App in Go

## Source 

https://dzone.com/articles/building-a-concurrent-chat-app-with-go-and-websock

## Flow
- When load localhost:4444, app.js will create a new websocket, which connect to /websocket endpoint in http server
  `let websocket = new WebSocket("ws://" + window.location.host + "/websocket");`
- In server, for request to /websocket, handleConnections is called
  - `err := ws.ReadJSON(&msg)` will block until ws receives message
- When UI click Submit, websocket on client side send message
- websocket in handleConnections is unblocked, put message to broadcast channel `broadcaster <- msg`
- the goroutine handleMessages receives messages from broadcast channel, write message to each websocket client
- in app.js, there is an event listener, if websocket client receives message, render the text
# D3 Lecture: Networking with TCP and HTTP

## Random Notes Section

- callbacks execute within the function AFTER the blocking process within that function is complete. That's how we can use them to do things once the blocking process has done its thing
- [Hacker News](https://news.ycombinator.com/) and [Lobsters](https://lobste.rs/)
- A "memory leak" seems to be when we are needlessly storing data in memory (we should remove data from memory when we no longer need it)

## Building a TCP Client/Server connection with Node

- start with the client, because EDD!
- looking at nodejs.org library (don't need tp npm install anything) 
  - find client in the documentation for the code
  - find server in the documentation for the code
  - there are lots of methods and objects to know about in this library 
- ideally pick a port that's greater than 1024 because smaller processes are using <=1024
- Node is a non-blocking language - so if we use something with the node backend (for example, server.listen(port#)), lines of code after will execute while Node handles the server.listen event in the event loop
- client.on('') and server.on('')  to create "callback" functions (different syntax as before though) to perform activities when the client or server do something
  - example: server.on('connection', (connectedClient) => console.log(connectedClient))

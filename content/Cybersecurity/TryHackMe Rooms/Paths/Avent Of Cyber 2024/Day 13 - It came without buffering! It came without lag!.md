#websockets

WebSockets let our browser and the server keep a constant line of communication open.
Great for live chat apps. real time games...

The other side is called polling, where every few seconds we ask for a resource to the server.

### Vulnerabilities
- **Weak Authentication and Authorisation**: WebSockets  don't have built-in ways to handle user authentication or session validation
- **Message Tampering**: attackers could intercept and change messages if encryption isn't used
- **Cross-Site WebSocket Hijacking (CSWSH)**: This happens when an attacker tricks a user's browser into opening a WebSocket connection to another site.
- **DoS**


### WebSocket Message Manipulation
Is when an attacker intercepts and changes the messages sent between a web app and its server.
The solution is strong message validation and encryption.
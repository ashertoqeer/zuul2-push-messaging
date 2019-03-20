# Zuul 2 Push Messaging

This is sample project for blog post: [Zuul 2 Push Messaging Sample](https://www.thebackendguy.com/2019/03/20/zuul-2-push-messaging-sample/)

There are two projects in this directory **Zuul 2 Push**, which acts as a push server in this setup, and **Web Socket Client** which is a very basic web socket client.

When we start **Zuul 2 Push** server, it will start listening on two end points one for clients to make a connection and listen for pushes and second for backend servers to submit a push to Zuul 2.

### Setup Zuul 2 Push Server:

There are two properties in `application.properties` file:

```properties
zuul.server.port.main=8887
zuul.server.port.http.push=9001
```

The main port is for web socket connections and http push port is for servers to submit push messages. 



Once Zuul 2 puch starts, it expects clients to maintain a long lived web socket connection on which Zuul 2 can send them notifications. Client does that by using end point: 
```
ws://[host]:8887/ws
```
Client needs to send two headers while initializing a connection: **origin** (as example.domain.com) and **X-CUSTOMER_ID** (as "1234", its a string not a number). In this way, Zuul 2 will add client connection to its register with client id as key.

Next any backend server can submit a push to Zuul 2 for a customer using endpoint (Try postman):

```
POST http://[host]:9001/push

Headers:
X-CUSTOMER_ID [customer id]

Body (Anything you like, I just added a random Json object):
{
	"message":"message",
	"payload": "this is some payload",
	"someNumber": 1
}
```

Here are saying that send this push (Json body) to specified customer by Id (In header X-CUSTOMER_ID). So, the client Id who is making the connection and who is recipient of the push from server side must match.



### Sample Web Socket Client

The directory also contains a web socket client. This is for testing purposes. The client, when started, try to make connection with Zuul 2 push server. Once established it can receive pushes send from Zuul 2. To try this, first start Zuul 2. Then start the client and when the connection get establish, use POST man to submit push to Zuul 2. It will immediately reflect in client.



#### Side Note:

The Zuul 2 push is modified version of original Zuul 2 sample. Please read https://github.com/Netflix/zuul/wiki/Push-Messaging to get a basic idea. The project is very basic demo or proof of concept.


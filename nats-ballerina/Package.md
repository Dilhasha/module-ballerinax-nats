## Module Overview

This module provides the capability to connect with NATS server and performs the 
below functionalities.

- Point to point communication (Queues)
- Pub/Sub (Topics)
- Request/Reply

### Basic Usage

#### Setting up the connection

First step is setting up the connection with the NATS Basic server. The following ways can be used to connect to a
NATS Basic server.

1. Connect to a server using the URL
```ballerina
nats:Client natsClient = new("nats://localhost:4222");
```

2. Connect to one or more servers with a custom configuration
```ballerina
nats:Client natsClient = new({"nats://serverone:4222",  "nats://servertwo:4222"},  config);
```

#### Publishing messages

##### Publishing messages to the NATS basic server

Once connected, publishing is accomplished via one of the below two methods.

1. Publish with the subject and the message content.
```ballerina
string message = "hello world";
nats:Error? result = 
    natsClient->publishMessage({ content: message.toBytes(), subject: "demo.nats.basic"});
```

2. Publish as a request that expects a reply.
```ballerina
string message = "hello world";
nats:Message|nats:Error reqReply = 
    natsClient->requestMessage({ content: message.toBytes(), subject: "demo.nats.basic"}, 5000);
```

3. Publish messages with a replyTo subject 
```ballerina
string message = "hello world";
nats:Error? result = natsClient->publish({ content: message.toBytes(), subject: "demo.nats.basic",
                                                    replyTo: "demo.reply" });
```

#### Listening to incoming messages

##### Listening to messages from a NATS server

```ballerina
// Binds the consumer to listen to the messages published to the 'demo' subject.
@nats:ServiceConfig {
    subject: "demo"
}
service demo on new nats:Listener() {

    remote function onMessage(nats:Message msg) {
    }
}
```

>**Note:** The default thread pool size used in Ballerina is the number of processors available * 2. You can configure the thread pool size by using the `BALLERINA_MAX_POOL_SIZE` environment variable.

For information on the operations, which you can perform with this module, see the below **Functions**. 

For examples on the usage of the connector, see the following.
* [Basic Publisher and Subscriber Example](https://ballerina.io/swan-lake/learn/by-example/nats-basic-client.html).

# TCP-UDP-Client-Server-App

This project is the second homework assignment for the Communication Protocols course, aiming to develop a client-server application for managing messages following the TCP and UDP communication models.

## Objectives
- Selecting mechanisms for developing network applications using TCP and UDP.
- Multiplexing TCP and UDP connections.
- Defining and utilizing a data type for a predefined protocol over UDP.
- Developing a client-server application using sockets.

## General Description
To achieve the objectives, we will consider implementing a platform comprising three components:
- The server (single) will facilitate communication between clients on the platform, allowing message publishing and subscription.
- TCP clients will connect to the server and can receive subscribe and unsubscribe commands from the keyboard (human interaction) and display messages received from the server on the screen.
- UDP clients (ready-made implementation provided by the responsible team) will publish messages to the proposed platform by sending them to the server using a predefined protocol. The desired functionality is for each TCP client to receive from the server those messages coming from UDP clients referring to the topics to which they are subscribed.

The application will also include a Store-and-Forward (SF) functionality regarding messages sent when TCP clients are disconnected.

## Server
The server acts as a broker, mediating message management in the platform. It opens two sockets (one TCP and one UDP) on a port received as a parameter and waits for connections/datagrams on all available local IP addresses.

### Starting the Server
The server is started using the command:
```
./server <PORT>
```

### Displayed Messages
For monitoring server activity, events of TCP client connection and disconnection are displayed in the standard output:
```
New client <ID_CLIENT> connected from IP:PORT.
Client <ID_CLIENT> disconnected.
```

### Accepted Commands
The server accepts only the `exit` command from the keyboard, which simultaneously closes the server and all connected TCP clients.

### Communication with UDP Clients
The server receives messages from UDP clients following the format defined in the table below:

| Field       | Size       | Description                                                                                   |
|-------------|------------|-----------------------------------------------------------------------------------------------|
| Topic       | 50 bytes   | Maximum 50-character string, terminated with '\0'                                            |
| Data Type   | 1 byte     | Unsigned integer (1 byte) used to specify the data type of the content                        |
| Content     | Max 1500 B | Variable-sized, depending on the data type as described in Section 4                           |

## TCP Clients
TCP clients can subscribe and unsubscribe to topics by sending messages to the server.

### Starting a TCP Client
A TCP client is started with the following command:
```
./subscriber <ID_CLIENT> <IP_SERVER> <PORT_SERVER>
```

### Accepted Commands
TCP clients can receive the following commands from the keyboard:
- `subscribe <TOPIC> <SF>`: Subscribes the client to messages on the specified topic, where SF can be 0 or 1.
- `unsubscribe <TOPIC>`: Unsubscribes the client from the specified topic.
- `exit`: Disconnects the client from the server and closes it.

### Displayed Messages
For each command received from the keyboard, the client displays a feedback line:
```
Subscribed to topic.
Unsubscribed from topic.
```

For each message received from the server, the client immediately displays a message in the format:
```
<UDP_CLIENT_IP>:<UDP_CLIENT_PORT> - <TOPIC> - <DATA_TYPE> - <MESSAGE_VALUE>
```

## Other Observations
Please consider the following additional points while implementing the project:
- To avoid ambiguity in parsing client commands, topics used in the application will be ASCII strings without spaces.
- Components should only display messages specified in the requirements. Any additional messages should be suppressed by default.
- All prints must end with a newline character, and buffering should be disabled for both server and TCP client.
- Error handling should be robust, including checking for errors from the socket.h API and defensive programming principles.
- Testing will involve UDP clients provided by the assistant team and a testing script available in the resources archive.
- An efficient protocol must be designed for TCP communication, considering both low and high message rates.
- Store-and-forward functionality should efficiently handle message storage, possibly utilizing RAM storage.

For the TCP protocol, ensure your custom protocol design is efficient and described thoroughly in the `readme.txt` file.

For store-and-forward, consider efficient message storage mechanisms, assuming sufficient RAM availability on the server for the stored messages. Unused messages should be removed from memory.

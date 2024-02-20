<img src="https://img.shields.io/badge/Passed-100%25-brightgreen" alt="Project Status" width="100" style="margin-bottom: 20px">

# ft_irc - Internet Relay Chat
Welcome to the ft_irc project repository!

ft_irc is an IRC (Internet Relay Chat) server implemented in C++98. 
It represents a significant milestone as the first project undertaken in C++ following the completion of the CPP Piscine at School 42. 
IRC is a widely-used text-based communication protocol facilitating real-time messaging in both group channels and direct messages.

The primary goal of this project is to develop a fully-functional IRC server that meets specific requirements and features outlined in the project specifications. 
Limechat was utilized as the client for testing and interaction with the IRC server.


## Introduction to Sockets

In computer networking, a socket is one endpoint of a two-way communication link between two programs running on the network. 
Sockets are used to establish communication between a client and a server over a network, such as the Internet. 
They provide a means of communication that allows processes to exchange data.

## Server Setup

The server setup involves creating a socket, binding it to a specific IP address and port, and listening for incoming connections. Here's a brief overview of the server setup process in the project:

1. **Creating the Socket**: The `socket()` system call is used to create a new socket. In the project, this socket is configured for streaming communication (`SOCK_STREAM`) and uses the TCP protocol.

2. **Binding the Socket**: The `bind()` system call is used to associate the socket with a specific IP address and port number. This allows the server to receive incoming connections on that IP address and port.

3. **Setting Socket Options**: Optional socket options can be set using the `setsockopt()` system call. In the project, the `SO_REUSEADDR` option is set to allow the reuse of local addresses and ports.

4. **Listening for Connections**: The `listen()` system call puts the socket in a listening state, allowing it to accept incoming connections from clients. The maximum number of pending connections that can be queued is specified as an argument to `listen()`.

5. **Accepting Connections**: When a client tries to connect to the server, the `accept()` system call is used to accept the connection. This creates a new socket for the client and returns a file descriptor that represents the connection.


## The `poll()` Function

`poll()` is a system call used in Unix and Unix-like operating systems to monitor multiple file descriptors to see if I/O is possible on any of them. 
It is an efficient way to handle I/O operations on multiple sockets or file descriptors simultaneously without the need for busy waiting or managing multiple threads.

In the context of this project, `poll()` is utilized to efficiently manage I/O operations on multiple client connections to the IRC server. 
Instead of handling each connection sequentially or using blocking I/O operations, `poll()` allows the server to handle multiple connections concurrently, making the server more responsive and efficient.

### Usage of `poll()` in the Project
In the project, the `poll()` function is used to manage multiple client connections efficiently. Here's how `poll()` is used in the server code:

1. **Initializing `pollfd` Structures**: Before entering the main event loop, `pollfd` structures are initialized for the server socket and each client socket. These structures specify the file descriptors to monitor for events and the types of events to monitor.

2. **Waiting for Events**: The server enters a loop where it waits for events using the `poll()` function. The server can wait for events on multiple file descriptors simultaneously, allowing it to handle multiple client connections concurrently.

3. **Handling Events**: When an event occurs on a file descriptor (e.g., data available for reading), the server reacts accordingly. For example, if data is available for reading on a client socket, the server reads the data and processes it.

4. **Non-blocking I/O**: The sockets used in the project are set to non-blocking mode using the `fcntl()` system call. This ensures that I/O operations on the sockets do not block, allowing the server to handle multiple connections efficiently without waiting for each operation to complete.


## Compile
1. Clone this repository
2. Compile the project:
```make all```
3. Run the program using the below command:
```./ircserv <port> <password>```

## Clients
Once the server is compiled and running, follow these steps to connect to it:
#### Limechat
1. Open Limechat
2. In the bottom right window, right-click and select "_Add Server_"
3. Fill in the following details:
	- **Network name**: Enter any name.
	- **Server**: Enter "_localhost_".
	- **Port**: Enter the port used when running the server.
	- **Server Password**: Provide the password used when running the server.
	- **Nickname**: Choose any nickname.
4. The server should appear in the right bottom window. Right-click on it and select "_Connect_".

#### Command Line (Netcat)
Alternatively, you can use the Netcat (nc) command in the terminal to connect to the server:

```nc localhost <port>```

Replace `<port>` with the actual port number used when running the server.


## Authentication

Once a user is connected to the server, they need to get authenticated which is known as 'server handshake'. For that, the user needs to pass the below three commands with the appropriate parameters:
#### PASS
- **Description**: Authenticate with the server.
- **Syntax**: **`PASS <password>`**
- **Example**: **PASS mypassword**

#### NICK
- **Description**: Set or change the user's nickname. Required initially to authenticate with the server but can also be used at a later stage to change the nickname
- **Syntax**: **`NICK <nickname>`**
- **Example**: **NICK user123**

#### USER
- **Description**: Set username and realname while connecting to the server.
- **Syntax**: **`USER <username> <hostname> <servername> <realname>`**
- **Example**: **USER user123 * * :Real Name**

## Commands

#### JOIN
- **Description**: Join an existing channel. If the channel does not exist, it will be created. This can be used to join several channels at the same time.
- **Syntax**: **`JOIN #<channel>`**
- **Example**: **JOIN #general**

#### TOPIC
- **Description**: Change or view the topic of a channel.
- **Syntax**: **`TOPIC #<channel> : [<topic>]`**
- **Example**: **TOPIC #general : New topic here**

If `<topic>` is not provided, this command will show the topic of the channel.

#### INVITE
- **Description**: Invite a user to a channel.
- **Syntax**: **`INVITE <nick> #<channel>`**
- **Example**: **INVITE user123 #general**

#### KICK
- **Description**: Remove a user from a channel.
- **Syntax**: **`KICK #<channel> <nick> : [<reason>]`**
- **Example**: **KICK #general user123 : Spamming**

#### PART
- **Description**: Leave a channel. It can be used to leave several channels at the same time.
- **Syntax**: **`PART #<channel> : [<comment>]`**
- **Example**: **PART #general : Leaving the conversation**

#### PRIVMSG
- **Description**: Send a private message to a user or a message to a channel.
- **Syntax**: **`PRIVMSG <nick of receiver> / #<channel> :<message>`**
- **Examples**: 

			**PRIVMSG user123 :Hello there!**

			**PRIVMSG #general :Hello there!**

#### MODE (i, t, k, o, l)
- i: Set/remove Invite-only channel.
- t: Set/remove the restrictions of the TOPIC command to channel operators.
- k: Set/remove the channel key (password).
- o: Give/take channel operator privilege.
- l: Set/remove the user limit to channel.

#### QUIT
- **Description**: Disconnect from the server.
- **Syntax**: **`QUIT**



## Contributors

<img src="https://img.shields.io/badge/Contributors-3-blue" alt="Contributors" width="100">

- [Mariam Kovoor](https://github.com/markov199)
- [Cristina Cestini](https://github.com/ccestini)
- [Lara Elkhoury](https://github.com/Larakh88)

## References

https://www.rfc-editor.org/rfc/rfc1459.html

http://chi.cs.uchicago.edu/chirc/irc_examples

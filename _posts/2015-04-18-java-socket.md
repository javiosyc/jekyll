---
layout: post
title: "Java Socket"
description: "java socket"
category: java
tags: [Java, Socket]
---
{% include JB/setup %}

## 1 TCP Sockets

A communication link created via TCP/IP sockets is a connection-orientated link.

This means that the connection between server and client remains open throughout the duration of the dialogue between the two and is only broken (under normal circumstances) when one end of the dialogue formally terminates the exchanges (via an agreed protocol).

 Since there are two separate types of process involved (client and server), we shall examine them separately, taking the server first.

 Setting up a server process requires five steps...

1. Create a ServerSocket object.

The ServerSocket constructor requires a port number (1024–65535, for non-reserved ones) as an argument.

	ServerSocket serverSocket = new ServerSocket(1234);

In this example, **the server will await (‘listen for’) a connection from a client on port 1234**.

2. Put the server into **a waiting state**.

The server waits indefinitely (‘blocks’) for a client to connect. 

**It does this by calling method accept of class ServerSocket, which returns a Socket object when a connection is made.

For example:

    Socket link = serverSocket.accept();

3. Set up input and output streams.

**Methods getInputStream and getOutputStream of class Socket are used to get references to streams associated with the socket returned in step 2**.

These streams will be used for communication with the client that has just made connection.

For **a non-GUI application, we can wrap a Scanner object around the InputStream object returned by method getInputStream**, in order to obtain string-orientated input (just as we would do with input from the standard input stream, System.in).

For example:

	Scanner input = new Scanner(link.getInputStream());

Similarly, we can wrap a **PrintWriter object around the OutputStream object returned by method getOutputStream**.

Supplying the PrintWriter constructor with a second argument of true will cause the output buffer to be flushed for every call of println (which is usually desirable).

For example:

	Scanner input = new Scanner(link.getInputStream());

Similarly, we can wrap a PrintWriter object around the OutputStream object returned by method getOutputStream.

Supplying the PrintWriter constructor with a second argument of true will cause the output buffer to be flushed for every call of println (which is usually desirable).

For example:

	PrintWriter output = new PrintWriter(link.getOutputStream(),true);

4. Send and receive data.

Having set up our Scanner and PrintWriter objects, sending and receiving data is very straightforward.

We simply use method nextLine for receiving data and method println for sending data, just as we might do for console I/O.

For example:

    output.println("Awaiting data...");

    String input = input.nextLine();

5. Close the connection (after completion of the dialogue).

	link.close();


Setting up the corresponding **client involves four steps**...


1. Establish a connection to the server.

We create a Socket object, supplying its constructor with the following two arguments:

1. the server’s IP address (of type InetAddress);

2. the appropriate port number for the service.(The port number for server and client programs must be the same, of course!)

For simplicity’s sake, we shall place client and server on the same host, which will allow us to **retrieve the IP address by calling static method getLocalHost of class InetAddress**.

For example:

	Socket link = new Socket(InetAddress.getLocalHost(),1234);

2. Set up input and output streams.

These are set up in exactly the same way as the server streams were set up (by calling methods getInputStream and getOutputStream of the Socket object that was created in step 2).


3. Send and receive data.

The Scanner object at the client end will receive messages sent by the PrintWriter object at the server end, while the PrintWriter object at the client end will send messages that are received by the Scanner object at the server end (using methods nextLine and println respectively).


4. Close the connection.

This is exactly the same as for the server process (using method close of class Socket).


## Datagram (UDP) Sockets

Unlike TCP/IP sockets, datagram sockets are connectionless.

That is to say, the connection between client and server is not maintained throughout the duration of the dialogue.


A further difference from TCP/IP sockets is that, instead of a ServerSocket object, the server creates a DatagramSocket object, as does each client when it wants to send datagram(s) to the server.

The final and most significant difference is that DatagramPacket objects are created and sent at both ends, rather than simple strings.


This process involves the following nine steps, though only the first eight steps will be executed under normal circumstances...

1. Create a DatagramSocket object.

Just as for the creation of a ServerSocket object, this means supplying the object’s constructor with the port number.

For example:

	DatagramSocket datagramSocket = new DatagramSocket(1234);

2. Create a buffer for incoming datagrams.

This is achieved by creating an array of bytes.

For example:

	byte[] buffer = new byte[256];

3. Create a DatagramPacket object for the incoming datagrams.

The constructor for this object requires two arguments:

1. the previously-created byte array;

2. the size of this array.

For example:

	DatagramPacket inPacket = new DatagramPacket(buffer, buffer.length);

4. Accept an incoming datagram.

This is effected via the receive method of our DatagramSocket object, using our DatagramPacket object as the receptacle.

For example:

	datagramSocket.receive(inPacket);

5. Accept the sender’s address and port from the packet.

Methods getAddress and getPort of our DatagramPacket object are used for this.

For example:

	InetAddress clientAddress = inPacket.getAddress();

	int clientPort = inPacket.getPort();


6. Retrieve the data from the buffer.

For convenience of handling, the data will be retrieved as a string, using an overloaded form of the String constructor that takes three arguments:

1. a byte array;

2. the start position within the array (= 0 here);

3. the number of bytes (= full size of buffer here).

For example:

	String message =  new String(inPacket.getData(), 0,inPacket.getLength());

7. Create the response datagram.

Create a DatagramPacket object, using an overloaded form of the constructor that takes four arguments:

1. the byte array containing the response message;

2. the size of the response;

3. the client’s address;

4. the client’s port number.

The first of these arguments is returned by the getBytes method of the String class (acting on the desired String response).

For example:

	DatagramPacket outPacket = new DatagramPacket(response.getBytes(), response.length(),clientAddress, clientPort);

(Here, response is a String variable holding the return message.)

8. Send the response datagram.

This is achieved by calling method send of our DatagramSocket object, supplying our outgoing DatagramPacket object as an argument.

For example:

	datagramSocket.send(outPacket);

Steps 4–8 may be executed indefinitely (within a loop).

Under normal circumstances, the server would probably not be closed down at all.

However, if an exception occurs, then the associated DatagramSocket should be closed, as shown in step 9 below.


9. Close the DatagramSocket.

This is effected simply by calling method close of our DatagramSocket object.

For example:

	datagramSocket.close();


---
layout: post
title: "Java Sockets for Clients"
description: "Java Sockets for Clients"
category: java
tags: [java, socket]
---
{% include JB/setup %}

 Sockets allow the programmer to treat a network connection as just another stream onto which bytes can be written and from which bytes can be read.

 Sockets shield the programmer from low-level details of the network, such as error detection, packet sizes, packet splitting, packet retransmis‐ sion, network addresses, and more.

 ### Using Sockets

A socket is a connection between two hosts. It can perform seven basic operations:
	
1. Connect to a remote machine

2. Send data

3. Receive data

4. Close a connection

5. Bind to a port

6. Listen for incoming data

7. Accept connections from remote machines on the bound port


Java’s Socket class, which is used by both clients and servers, has methods that correspond to the first four of these operations.

The last three operations are needed only by servers, which wait for clients to connect to them.

They are implemented by the ServerSocket class, which is discussed in the next chapter.

Java programs normally use client sockets in the following fashion:

1. The program creates a new socket with a constructor.

2. The socket attempts to connect to the remote host.

Once the connection is established, the local and remote hosts get input and output streams from the socket and use those streams to send data to each other. 

**This connection is full-duplex.**

Both hosts can send and receive data simultaneously.

What the data means depends on the protocol; different commands are sent to an FTP server than to an HTTP server.

There will normally be some agreed-upon handshaking followed by the transmission of data from one to the other.


When the transmission of data is complete, one or both sides close the connection. 

Some protocols, such as HTTP 1.0, require the connection to be closed after each request is serviced.

Others, such as FTP and HTTP 1.1, allow multiple requests to be processed in a single connection.


### Reading from Servers with Sockets

Now let’s see how to retrieve this same data programmatically using sockets. 

First, open a socket to time.nist.gov on port 13:

	Socket socket = new Socket("time.nist.gov", 13);

This doesn’t just create the object.

It actually makes the connection across the network.

If the connection times out or fails because the server isn’t listening on port 13, then the constructor throws an IOException, so you’ll usually wrap this in a try block.

	Socket socket = null;

	try {
		socket = new Socket(hostname, 13);

      	// read from the socket...

	} catch (IOException ex) { 
	
		System.err.println(ex);
	
	} finally {
	
		if (socket != null) {
	
			try { 

				socket.close();

			} catch (IOException ex) {

				// ignore

			}

		}

	}


The next step is optional but highly recommended.

Set a timeout on the connection using the setSoTimeout() method.

Timeouts are measured in milliseconds, so this statement sets the socket to time out after 15 seconds of nonresponsiveness:

	socket.setSoTimeout(15000);

Although a socket should **throw a ConnectException pretty quickly if the server rejects the connection**,

or **a NoRouteToHostException if the routers can’t figure out how to send your packets to the server**,

neither of these help you with the case where a misbehaving server accepts the connection and then stops talking to you without actively closing the connection.

Setting a timeout on the socket means that each read from or write to the socket will take at most a certain number of milliseconds.

If a server hangs while you’re connected to it, you will be notified with a SocketTimeoutException. 

Exactly how long a timeout to set depends on the needs of your application and how responsive you expect the server to be.


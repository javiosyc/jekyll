---
layout: post
title: "swingUtilities"
description: ""
category: swing
tags: [swing,swingUtilities]
---
{% include JB/setup %}


[Java GUI编程SwingUtilities.invokeLater作用](http://blog.micxp.com/index.php/archives/109/).


###EventDispatchThread

Event-dispatching thread

All events are processed by the listeners that receive them within the event-dispatching thread (an instance of java.awt.EventDispatchThread).

All painting and component layout is expected to occur within this thread as well.

The event-dispatching thread is of primary importance to Swing and AWT, and plays a key role in keeping updates to component state and display in an app under control. 

Associated with this thread is a FIFO queue of events -- the system event queue (an instance of java.awt.EventQueue).

This gets filled up, as any FIFO queue, in a serial fashion.

Each request takes its turn executing event handling code, whether this be updating component properties, layout, or repainting.

All events are processed serially to avoid such situations as a component’s state being modified in the middle of a repaint.

Knowing this, we must be careful not to dispatch events outside of the event-dispatching thread.

For instance, calling a fireXX() method directly from a separate thread of execution is unsafe.

We must also be sure that event handling code, and painting code can be executed quickly.

Otherwise the whole system event queue will be blocked waiting for one event process, repaint, or layout to occur, and our application will appear frozen or locked up.

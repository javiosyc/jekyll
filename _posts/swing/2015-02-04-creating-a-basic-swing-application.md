---
layout: post
title: "creating a basic swing application"
description: ""
category: swing
tags: [swing]
---
{% include JB/setup %}


##Why to use SwingUtilities.invokeLater in main method?

###The docs explain why. [From Initial Threads](http://docs.oracle.com/javase/tutorial/uiswing/concurrency/initial.html)

Why does not the initial thread simply create the GUI itself? Because almost all code that creates or interacts with Swing components must run on the event dispatch thread.


### [From The Event Dispatch Thread](http://docs.oracle.com/javase/tutorial/uiswing/concurrency/dispatch.html)

Some Swing component methods are labelled "thread safe" in the API specification; these can be safely invoked from any thread. All other Swing component methods must be invoked from the event dispatch thread.

Programs that ignore this rule may function correctly most of the time, but are subject to unpredictable errors that are difficult to reproduce.


		SwingUtilities.invokeLater(new Runnable() {

			public void run() {

				JFrame frame = new JFrame("Hello World");

				frame.setSize(600, 500);

				frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

				frame.setVisible(true);

			}

		});


###Application startup code

There's one place where it's very easy to forget that we need SwingUtilities.invokeLater(), and **that's on application startup**.

**Our applications main() method** will always be called by **a special "main" thread that the VM starts up for us**.

**And this main thread is not the event dispatch thread!** So:

The code that initialises our GUI must also take place in an invokeLater().

So our initial main() method should look something like this:

		public class MyApplication extends JFrame {

			public static void main(String[] args) {

				SwingUtilities.invokeLater(new Runnable() {

					public void run() {

						MyApplication app = new MyApplication();

						app.setVisible(true);

					}

				});
			}

			private MyApplication() {

				// create UI here: add buttons, actions etc

			}

		}



## [A Visual Guide to Layout Managers](http://docs.oracle.com/javase/tutorial/uiswing/layout/visual.html)


###Several AWT and Swing classes provide layout managers for general use:

1.BorderLayout (Every content pane is initialized to use a BorderLayout.)

2.BoxLayout

3.CardLayout

4.FlowLayout

5.GridBagLayout

6.GridLayout

7.GroupLayout

8.SpringLayout


		public class MainFrame extends JFrame {

		

			public MainFrame() {

			...

			setLayout(new BorderLayout());

			...

			add(textArea, BorderLayout.CENTER);
	
			add(btn, BorderLayout.SOUTH);

			}

		}


### Setting Component Sizes and Border

You add the component using a layout manager and it's actually the layout manager that determines the size of the component.
 
In order to tell the layout managers what size and the components want to be, the components have anything called a preferred size.


		public class FormPanel extends JPanel{

			public FormPanel() {

				Dimension dim = getPreferredSize();

				dim.width = 250;

				setPreferredSize(dim);

				Border innerBorder = BorderFactory.createTitledBorder("Add Person");
		
				Border outerBorder = BorderFactory.createEmptyBorder(0, 5, 5, 5);

				setBorder(BorderFactory.createCompoundBorder(outerBorder, innerBorder));

			}

		}


### The Observer pattern

In Design Patterns, the authors describe the Observer pattern like this:

Define a one to many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.


The Observer pattern has one subject and potentially many observers.

Observers register with the subject, which notifies the observers when events occur.

The prototypical Observer example is a graphical user interface (GUI) that simultaneously displays two views of a single model; the views register with the model, and when the model changes, it notifies the views, which update accordingly.

### The Decorator Pattern
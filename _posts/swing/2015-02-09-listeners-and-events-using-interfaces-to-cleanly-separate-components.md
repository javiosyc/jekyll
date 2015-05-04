---
layout: post
title: "Listeners and Events Using Interfaces to Cleanly Separate Components"
description: "Listeners and Events Using Interfaces to Cleanly Separate Components"
category: swing
tags: [swing]
---
{% include JB/setup %}


1. create xxxListener


		interface StringListener {
	
			public void textEmitted(String text)
	
		}


2. toolbar 
   

	add attribute  : 

		private StringListener listener;


	add set method : 

		public void setStringListener(StringListener listener){	

		}

	modify actionPerformed method:


		public void actionPerformed(ActionEvent e) {
			
			JButton clicked = (JButton) e.getSource();
		
			if(clicked == helloButton) {

				if(textListener !=null) {

					textListener.textEmitted("Hello");

				}

			} else {

				if(textListener !=null) {

					textListener.textEmitted("goodbye");

				}

			}
		}

3. MainFrame (Controller)


		toolbar.setStringListener( new StringListener () {
		
			public void textEmitted(String text) {
			
				textPanel.appendText(text);	
		
			}
		});

4. 




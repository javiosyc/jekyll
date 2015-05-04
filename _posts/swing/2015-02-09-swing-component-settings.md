---
layout: post
title: "swing component settings"
description: ""
category: swing
tags: [swing]
---
{% include JB/setup %}


### Setting Component Sizes

1. Layout Manger determines the size of the component.

	use preferred size

		Dimension dim = getPreferredSize();
			
			dim.width = 250;
			
			setPreferredSize(dim); // tell the layout managers what size and the components want to be.



## Setting Borders


Border innerBorder = BorderFactory.createTitleBorder("Add Person");

Border outBorder = BorderFactory.createEmptyBorder(5,5,5,5);

setBorder( BorderFactory.createCompundBorder(outBorder,innerBorder);





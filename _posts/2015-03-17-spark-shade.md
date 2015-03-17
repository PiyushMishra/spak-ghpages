---
layout: post
title: "Spark shading"
description: ""
category: 
tags: []
---

Understanding maven shading functionality in building spark jar.

Apache maven shade plugin is used to include/execlude dependencies from uber jar in order to make it compact.

Sometimes what happens that we have mutliple versions of different artifects transitively in project build and while packaging the application in a single distribution jar we realize difficulties. Hence maven shade plugin help us to rename the packages of artifects and packaging it.

There are many remarkable places where shading is used in spark. 



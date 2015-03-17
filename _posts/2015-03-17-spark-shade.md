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

Below is a schenario where we can use it.

A Foo library, which depends on a specific version (e.g. 1.0) of Bar library. Assuming I cannot make use of other version of Bar lib (because API change, or other technical issues, etc). If I simply declare Bar:1.0 as Foo s dependency in Maven, it is possible to fall into a problem: A Qux project is depending on Foo, and also Bar:2.0 (and it cannot use Bar:1.0 because Qux needs to use new feature in Bar:2.0). Then it is a dilemma: should I use Bar:1.0 (which Qux s code will not work) or Bar:2.0 (which Foo s code will not work).

In order to solve this problem, developer of Foo can choose to use shade plugin to rename its usage of Bar, so that all classes in Bar:1.0 jar is now embedded in Foo jar, and the package of the embedded Bar classes is changed from com.bar to com.foo.bar. By doing so, Qux can safely depends on Bar:2.0 because now Foo is no longer depending on Bar, and it is using is own copy of "altered" Bar located in another package.



There are many remarkable places where shading is used in spark. 



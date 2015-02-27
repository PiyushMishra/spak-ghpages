---
layout: post
title:  "BlockManger"
date:   2015-02-27 03:26:52 	
categories: understanding block manager
---

#                                                        Understanding BlockManger

In Spark BlockManager run on every node(driver or executers) which provides interfaces for sending blocks of data to and from various resources like memory, disks, off-heap both locally and remotely.


Note that #initialize() must be called before the BlockManager is usable.

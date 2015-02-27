---
layout: post
title:  "BlockManger"
date:   2015-02-27 03:26:52 	
categories: understanding block manager
---

#                                                        Understanding BlockManger

In Spark BlockManager run on every node(driver or executers) which provides interfaces for sending blocks of data to and from various resources like memory, disks, off-heap both locally and remotely.

Note that #initialize() must be called before the BlockManager is usable.

Blockmangers communicate with other Blockmanagers running on different nodes for sending and retriving data

Blockmanager contains shufflemanagers, security managers and blocktransferService as its components. It extends the interface BlockDataManager
which exposes methods getBlockData and putBlockData to play with data locally.

It contains a BlockManagerSlaveActor which takes command from BlockManagerMaster to do operations like getting, Block Status, removing blocks, removing rdd, removing broadcast to slave nodes.

Below is the high level diagram of BlockManager.



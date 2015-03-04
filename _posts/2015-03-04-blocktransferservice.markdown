---
layout: post
title: "BlockTransferService"
categories: Understanding blockTransferService
---

BlockTransferService is absract class which extends abstract class ShuffleClient. It provides a interface which exposes methods for fetching and putting data blocks locally.
Below is the UML diagram of the interface.

![BlockTransferservice image not found]({{ site.baseurl }}/images/BlockTransferService.jpeg)

<b>init</b> : It takes BlockDataManager as argument in order to fetch and put data blocks locally.

<b>close</b> : It close down the transfer service.

<b>port</b> : Port on which this service listens.

<b>hostName</b> : Hostname of the machine which hosts the service.

<b>fetchBlocks</b> : Fetch a sequence of blocks from a remote node asynchronously,this API takes a sequence so the implementation can batch requests, and does not return a future so the underlying implementation can invoke onBlockFetchSuccess as soon asthe data of a block is fetched, rather than waiting for all blocks to be fetched.
    
<b>uploadblocks</b> : Upload a single block to a remote node
 
<b>fetchBlockSync</b> : Fetches only one block and is blocking

</b>uploadBlockSync</b> : Upload a single block to a remote node, blocks the thread until the upload finishes


<b>NettyBlockTransferService</b> and <b>NioBlockTransferService</b> are concrete classes which extends BlockTransferService.

![hierarchy image not found]({{ site.baseurl }}/images/BlockTransferService-hierarchy.jpeg)

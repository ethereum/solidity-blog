---
id: 3658
title: Introduction of the Light Client for DApp developers
date: 2017-01-07T03:42:14+00:00
author: Zsolt Felföldi
layout: post
guid: https://blog.ethereum.org/?p=3658
permalink: /2017/01/07/introduction-light-client-dapp-developers/
---
<p class="p1"><span class="s1">The first version of the Light Ethereum Subprotocol (LES/1) and its implementation in Geth are still in </span><span class="s2">an</span><span class="s1"> experimental stage, but they are expected to reach a more </span><span class="s2">mature</span><span class="s1"> state in a few months where the basic functions will perform reliably. </span><span class="s1">The light client has been designed to function more or less the same as a full client, but the “lightness" has some inherent limitations that DApp developers </span><span class="s2">should </span><span class="s1">understand and </span><span class="s2">consider when designing their applications</span><span class="s1">. </span></p>
<p class="p1">In most cases a properly designed application can work even without knowing what kind of client it is connected to, but we are looking into adding an API extension for communicating different client capabilities in order to provide a future proof interface. While minor details of LES are still being worked out, I believe it is time to clarify the most important differences between full and light clients from the application developer perspective.</p>
## Current limitations

### Pending transactions

Light clients do not receive pending transactions from the main Ethereum network. **The only pending transactions a light client knows about are the ones that have been created and sent from that client.** When a light client sends a transaction, it starts downloading entire blocks until it finds the sent transaction in one of the blocks, then removes it from the pending transaction set.

### Finding a transaction by hash

**Currently you can only find locally created transactions by hash.** These transactions and their inclusion blocks are stored in the database and can be found by hash later. Finding other transactions is a bit trickier. It is possible (though not implemented as of yet) to download them from a server and verify the transaction is actually included in the block if the server found it. Unfortunately, if the server says that the transaction does not exist, it is not possible for the client to verify the validity of this answer. It is possible to ask multiple servers in case the first one did not know about it, but the client can never be absolutely sure about the non-existence of a given transaction. For most applications this might not be an issue but it is something one should keep in mind if something important may depend on the existence of a transaction. A coordinated attack to fool a light client into believing that no transaction exists with a given hash would probably be difficult to execute but not entirely impossible.

## Performance considerations

### Request latency
<p class="p1"><span class="s1">The only thing a light client always has in its database is the last few thousand block headers. This means that retrieving anything else requires the client to send requests and get answers from light servers. The light client tries to optimize request </span><span class="s2">distribution</span><span class="s1"> and collects statistical data of each server's usual response times in order to reduce latency. <strong>Latency is the key performance parameter of a light client. It is usually in the 100-200ms order of magnitude, and it applies to every state/contract storage read, block and receipt set retrieval.</strong> </span>If many requests are made sequentially to perform an operation, it may result in a slow response time for the user. Running API functions in parallel whenever possible can greatly improve performance.</p>
### Searching for events in a long history of blocks

Full clients employ a so-called "MIP mapped" bloom filter to find events quickly in a long list of blocks so that it is reasonably cheap to search for certain events in the entire block history. Unfortunately, using a MIP-mapped filter is not easy to do with a light client, as searches are only performed in individual headers, which is a lot slower. Searching a few days' worth of block history usually returns after an acceptable amount of time, but **at the moment you should not search for anything in the entire history because it will take an extremely long time.**

### Memory, disk and bandwidth requirements

Here is the good news: a light client does not need a big database since it can retrieve anything on demand. With garbage collection enabled (which scheduled to be implemented), <span class="s1">the database will function more like a </span>cache, and a light client will be able to run with **as low as 10Mb of storage space**. Note that the current Geth implementation uses around **200Mb of memory**, which can probably be further reduced. Bandwidth requirements are also lower when the client is not used heavily. Bandwidth used is usually well under **1Mb/hour when running idle, with an additional 2-3kb for an average state/storage request**.

## Future improvements

### Reducing overall latency by remote execution

Sometimes it is unnecessary to pass data back and forth multiple times between the client and the server in order to evaluate a function. It would be possible to execute functions on the server side, then collect all the Merkle proofs proving every piece of state data the function accessed and return all the proofs at once so that the client can re-run the code and verify the proofs. This method can be used for both read-only functions of the contracts as well as any application-specific code that operates on the blockchain/state as an input.

### Verifying complex calculations indirectly
<p class="p1"><span class="s1">One of the main limitations we are working to improve</span> is the slow search speed of log histories. Many of the limitations mentioned above, including the difficulty of obtaining MIP-mapped bloom filters, follow the same pattern: the server (which is a full node) can easily calculate a certain piece of information, which can be shared with the light clients. But the light clients currently have no practical way of checking the validity of that information, since verifying the entire calculation of the results directly would require so much processing power and bandwidth, which would make using a light client pointless.</p>
<p class="p1"><span class="s1">Fortunately there is a safe and trustless solution to the general </span><span class="s2">task</span><span class="s1"> of indirectly validating remote calculations based on an input dataset that both parties assume to be available, even if the receiving party does not have the actual data, only its hash. </span><span class="s2">This is the exact the case</span><span class="s1"> in our scenario where the Ethereum blockchain itself can be used as an input for such a verified calculation.</span> This means it is possible for light clients to have capabilities close to that of full nodes because they can ask a light server to remotely evaluate an operation for them that they would not be able to otherwise perform themselves. The details of this feature are still being worked out and are outside the scope of this document, but the general idea of the verification method is explained by Dr. Christian Reitwiessner in this <a href="https://www.youtube.com/watch?v=5yiotumm99Q">Devcon 2 talk</a>.</p>
Complex applications accessing huge amounts of contract storage can also benefit from this approach by evaluating accessor functions entirely on the server side and not having to download proofs and re-evaluate the functions. Theoretically it would also be possible to use indirect verification for filtering events that light clients could not watch for otherwise. However, in most cases generating proper logs is still simpler and more efficient.
# Kademlia: A Peer-to-peer Information System Based on the XOR Metric (Petar Maymounkov and David Mazieres)

## Abstract. 

We describe a peer-to-peer distributed hash table with prov-able consistency and performance in a fault-prone environment. 

Our system routes queries and locates nodes using a novel XOR-based met-ric topology that simplifies the algorithm and facilitates our proof. 

The topology has the property that every message exchanged conveys or re-inforces useful contact information. 

The system exploits this information to send parallel, asynchronous query messages that tolerate node failures without imposing timeout delays on users.


## Introduction

This paper describes Kademlia, a peer-to-peer distributed hash table (DHT). 

Kademlia has a number of desirable features not simultaneously offered by any previous DHT.

It minimizes the number of configuration messages nodes must send to learn about each other. 

Configuration information spreads automatically as a side-effect of key lookups. 

Nodes have enough knowledge and flexibility to route queries through low-latency paths. 

Kademlia uses parallel, asynchronous queries to avoid timeout delays from failed nodes. 

The algorithm with which nodes record each other's existence resists certain basic denial of service attacks. 

Finally, several important properties of Kademlia can be formally proven using only weak assumptions on uptime distributions (assumptions we validate with measurements of existing peer-to-peer systems). 

Kademlia takes the basic approach of many DHTs. Keys are opaque, 160-bit quantities (e.g., the SHA-1 hash of some larger data). 

Participating computers each have a node ID in the 160-bit key space. (key,value) pairs are stored on nodes with IDs "close" to the key for some notion of closeness. 

Finally, a node-ID-based routing algorithm lets anyone efficiently locate servers near any given target key. 

Many of Kademlia's benefits result from its use of a novel XOR metric for distance between points in the key space. 

XOR is symmetric, allowing Kademlia participants to receive lookup queries from precisely the same distribution of nodes contained in their routing tables. 

Without this property, systems such as Chord [5] do not learn useful routing information from queries they receive. 

Worse yet, asymmetry leads to rigid routing tables. Each entry in a Chord node's finger table must store the precise node preceding some interval in the ID space. 

Any node actually in the interval would be too far from nodes preceding it in the same interval. 

Kademlia, in contrast, can send a query to any node within an interval, allowing it to select routes based on latency or even send parallel, asynchronous queries to several equally appropriate nodes. To locate nodes near a particular ID, Kademlia uses a single routing algo-rithm from start to finish. 

In contrast, other systems use one algorithm to get near the target ID and another for the last few hops. Of existing systems, Kadem-lia most resembles Pastry's [1] first phase, which (though not described this way by the authors) successively finds nodes roughly half as far from the target ID by Kademlia's XOR metric. In a second phase, however, Pastry switches distance metrics to the numeric difference between IDs. 

It also uses the second, numeric difference metric in replication. Unfortunately, nodes close by the second met-ric can be quite far by the first, creating discontinuities at particular node ID values, reducing performance, and complicating attempts at formal analysis of worst-case behavior. 


## System description 
Our system takes the same general approach as other DHTs. 

We assign 160-bit opaque IDs to nodes and provide a lookup algorithm that locates successively "closer" nodes to any desired ID, converging to the lookup target in logarithmically many steps. 

Kademlia effectively treats nodes as leaves in a binary tree, with each node's position determined by the shortest unique prefix of its ID. 

Figure 1 shows the position of a node with unique prefix 0011 in an example tree. For any given node, we divide the binary tree into a series of successively lower subtrees that don't contain the node. 

The highest subtree consists of the half of the binary tree not containing the node. The next subtree consists of the half of the remaining tree not containing the node, and so forth. In the example of node 0011, the subtrees are circled and consist of all nodes with prefixes 1, 01, 000, and 0010 respectively. 

The Kademlia protocol ensures that every node knows of at least one node in each of its subtrees, if that subtree contains a node. 

With this guarantee, any node can locate any other node by its ID. Figure 2 shows an example of node 0011 locating node 1110 by successively querying the best node it knows of to find contacts in lower and lower subtrees; finally the lookup converges to the target node. 

The remainter of this section fills in the details and makes the lookup algo-rithm more concrete. 

We first define a precise notion of ID closeness, allowing us to speak of storing and looking up (key, value) pairs on the k closest nodes to the key. 

We then give a lookup protocol that works even in cases where no 

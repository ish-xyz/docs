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
XOR is symmetric, allowing Kademlia participants to receive lookup queries from precisely the same distribution of 
This research was partially supported by National Science Foundation grants CCR 0093361 and CCR 9800085. 


# Hazelcast
## What is it?
In-memory data grids, also known as IMDGs, are distributed data structures, where the whole data is stored entirely on the RAM (Random Access Memory) across a cluster. This way, we could have both the advantages of using faster resources such as the RAM, opposed to the hard disk - off course, IMDGs such as HazelCast also use the hard disk as a persistent store for fault tolerance, but still, for the applications usage, the major resource is the RAM - and the horizontal scalability of a cluster, opposed to the traditional vertical scalability of a relational database, for example.



## Features
* Distributed data structures
* Distributed compute
* Distributed query
* Clustering
* Caching
* Multiple language bindings
* Easily embeddable in a Java application
* Horizontal scaling

This feature set makes Hazelcast a multi-use tool in an application. It can be used for simple messaging, caching, a key/value store, and, as described in the remainder of this document, a task coordination service. Unlike some of the other services previously described, Hazelcast can be leveraged to solve multiple problems in an application besides just distributed configuration and coordination. Being designed as a distributed memory-grid from the beginning, Hazelcast solves many of the hard underlying problems such as master election, network resiliency, and eventual consistency.

**Note:** Horizontal scaling means that you scale by adding more machines into your pool of resources whereas Vertical scaling means that you scale by adding more power (CPU, RAM) to an existing machine.


## Common usage
* caching layer for databases
* clustering web sessions for web applications 
* NOSQL solutions



## Installation
```
cd ~/Apps
wget https://download.hazelcast.com/download.jsp?version=hazelcast-3.11&p=402306231
unzip hazelcast-3.11
```



## Run
To start a HazelCast node, we simply start a shell script provided by the installation. On versions previous to3.2.6, this script was called run.sh and was inside the bin folder. On newer versions, the script is called server.sh, but it is still inside the bin folder. So to start a node, simply navigate on a terminal to the folder we extracted previously - if the reader is using the same version I am using, it would be called hazelcast-3.4.2 - and type:
```
cd ~/Apps/hazelcast-3.11/bin

./server.sh
```

For this lab, we will use a HazelCast cluster composed of 3 nodes, so, we open another 2 terminal windows and repeat the previous procedure to start the first node. HazelCast uses multicast to check and establish the participation of new nodes on the cluster. 

That's all we need to do to create the cluster for our lab. Now, let's start using HazelCast!



## Simple java example
To connect to our HazelCast cluster,  there's 3 ways:

* Programmatic configuration, by using the classes of the Java API;
* By XML configuration, using a XML called hazelcast.xml that we put on the classpath;
* By integrating HazelCast with Spring;

On this lab, we will explore the first option, so, starting our lab, let's create a new Maven project - without adding any archetype - and include the following dependencies on the pom.xml:
```
<dependencies>

	<dependency>
		<groupId>com.hazelcast</groupId>
		<artifactId>hazelcast</artifactId>
		<version>3.4.2</version>
	</dependency>
	<dependency>
		<groupId>com.hazelcast</groupId>
		<artifactId>hazelcast-client</artifactId>
		<version>3.4.2</version>
	</dependency>

</dependencies>
```

After including the dependencies,  let's create a class, responsible for creating a single HazelCast's instance for us, for reuse on the whole application:

```
public class HazelCastFactory {

	private static HazelcastInstance cluster;

	private static boolean shutDown = true;

	public static HazelcastInstance getInstance() {
		if (shutDown) {
			ClientConfig clientConfig = new ClientConfig();
			ClientNetworkConfig clientNetworkConfig = new ClientNetworkConfig();
			clientNetworkConfig.addAddress("127.0.0.1:5701");
			clientConfig.setNetworkConfig(clientNetworkConfig);
			cluster = HazelcastClient.newHazelcastClient(clientConfig);
			shutDown = false;
		}
		return cluster;
	}

	public static void shutDown() {
		cluster.shutdown();
		shutDown = true;
	}

}
```

On the code above we created a client to connect to our HazelCast's cluster, pointing the node on the 5701 port as the entry point of our connection. If we want to add other addresses for cases in which our entry node falls on the connection start, we just add more addresses with the addAddress method. There's no need to add the whole cluster to use the data grid, however: HazelCast itself is responsible for load balancing the requests across the cluster. We also included a method to shutdown our connection to the data grid, releasing the resources allocated.

NOTE: One interesting thing to note is that, when a client connects to a HazelCast cluster, he actually establish a connection to the cluster, not just the node we informed as the entry point, meaning that, even if the entry node falls after the connection is established, our client will still maintain a connection with the cluster.

Let's begin by creating a distributed object on the cluster, a map data structure. Distributed objects are objects created and managed by the cluster, with their data distributed and replicated across the cluster.

To create it, all we have to do is call the getMap mehod on the client instance we receive from the factory class, providing a unique name on the cluster to identify the map:
```
public class HazelCastDistributedMap {

	public static void main(String[] args) {
		HazelcastInstance client = HazelCastFactory.getInstance();
		Map<String, String> map = client.getMap("mymap");
		HazelCastFactory.shutDown();
	}

}
```

As we can see, is very simple to create a map. To use it, is even more simple: all we have to do is use the methods from the Map interface, just like we do with any basic Map on a common Java program. Behind the scenes, HazelCast is working for us, supplying a IMDG for our data. Let's demonstrate this by creating a more elaborated example. First, we create a POJO, representing a client (NOTE: in order to be distributed by HazelCast, the objects used must be serializable):
```
public class Client implements Serializable {

	private static final long serialVersionUID = -4870061854652654067L;

	private String name;

	private Long phone;

	private String sex;

	// Setters and getters ...

}
```

Then, we change the example to the following:
```
public class HazelCastDistributedMap {

	public static void main(String[] args) {
		HazelcastInstance client = HazelCastFactory.getInstance();
		IMap<Long, Client> map = client.getMap("customers");

		Client clientData = new Client();
		clientData.setName("Alexandre Eleuterio Santos Lourenco");
		clientData.setPhone(33455676l);
		clientData.setSex("M");
		map.put(clientData.getPhone(), clientData, 5, TimeUnit.MINUTES);

		clientData = new Client();
		clientData.setName("Lucebiane Santos Lourenco");
		clientData.setPhone(456782387l);
		clientData.setSex("F");
		map.put(clientData.getPhone(), clientData, 2, TimeUnit.MINUTES);
		clientData = new Client();

		clientData.setName("Ana Carolina Fernandes do Sim");
		clientData.setPhone(345622189l);
		clientData.setSex("F");
		map.put(clientData.getPhone(), clientData, 120, TimeUnit.SECONDS);

		HazelCastFactory.shutDown();

		client = HazelCastFactory.getInstance();
		Map<Long, Client> mapPostShutDown = client.getMap("customers");
		for (Long phone : mapPostShutDown.keySet()) {
			Client cli = mapPostShutDown.get(phone);
			System.out.println(cli.getName());
		}

		System.out.println(mapPostShutDown.size());
		HazelCastFactory.shutDown();

	}

}
```

As we can see on the new code, we obtained a distributed map from HazelCast, inserted some clients on it, shutdown the connection, reopened and finally iterated by the map, printing the names of the clients and the size of the map. If we run the code, we will receive, alongside logging information from the HazelCast's client, the following prints:
```
Ana Carolina Fernandes do Sim
Alexandre Eleuterio Santos Lourenco
Lucebiane Santos Lourenco
3
```
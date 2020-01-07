# Occasionally Connected Servers
Creating Modern Experiences in Geographically Distributed Environments



## Abstract
In the modern world there is an increasing desire across many industries to provide high quality digital experiences in real world locations. The consumers of these experiences may retail shoppers, attraction visitors, company employees. Powering these experiences with Data, Content or Insight constant access to data. This typically requires good, high speed data connections. Even with the proliferation of high-speed internet and incredibly high-speed mobile data connections, there are still real challenges to providing this consistent low latency internet bandwidth. 

In modern software practices it is expected that we accept the limitations of the technology that we operate with and design our systems to cater for the limitations that we have. This means we need to build software that works despite the intermittent connectivity that it will experience. We have done this since the dawn of mobile apps which the Occasionally Connected Client pattern. Here we solve the challenge by having ‘server’ applications use a mix of similar and new approaches to solve that problem for a site containing a number of end clients. We call this Occasionally Connected Servers.

Occasionally Connected Servers as a pattern seeks to provide high quality local experiences using local computers to serve the experience while they maintain a connection to the central system that serves content to all local servers. While the local environment has a good internet connection it is continually keeping itself in sync with the central system downloading and uploading data as required. When the internet connection is unavailable or has too much latency this local server keeps the experiences working.

While this isn’t a new high-level architecture, the user experience bar has never been higher while new technologies and approaches make it more relevant powerful and easier to build and run. 
 
## Introduction


In recent years there has been significant developments in the technology that underpins the online, connected experiences that companies have been able to provide

In some industries, such as especially retail, the online experiences have had a significant impact on the physical locations, with people choosing to take their business online.

In other industries like food and beverage or even cinemas, there has been a shift towards providing ‘Apps’ to enable mobile order and payment, menu browsing, ticket ordering etc. 

The underlying trend across companies being successful either online or in physical locations is their ability to provide a high quality, information rich experience. 

All this Up-Levelling of the digital experiences 

An example from retail is Amazon vs Traditional clothing retail. On Amazon I can browse clothing, see all the possible sizes, colors, styles and combinations. In traditional retail, I can see what’s on display, if I can find it. If there is a size missing or colors missing, its impossible for me to know. When I ask I often get vague answers. The staff often don’t have any more information than me. Some large department stores don’t even have access to whether something is in stock, or if they have a number it could be ‘well I had 2 yesterday’ and it often doesn’t differentiate between styles, colors or sizes. 

![High Level View of current state. Servers central and experiences in remote locations connected through the internet](./images/occasionally-connected-servers/high-level-view.png)
 

There are retail stores that are working on this, they have apps and better access to data, however they are still hamstrung by the fact that in their stores, their internet connection is slow, sometimes non functional, and the building design often limits cellular connection for peoples devices

What’s possibly more frustrating for consumers is knowing they have an app that would provide them with all the information and access that they desire, but they just see spinning circle that says the information is coming some day
	

## Solving the problems

Previous attempts to build these experiences have taken one of several options:
1)	Online Only Clients – this is where each endpoint client connects to a central server to get access to data and content as needed. When the internet connection is slow or unavailable, the experience degrades or disables. 
2)	Occasionally Connected Clients (OCC) – using more traditional mobile patterns so each endpoint client acts as an individual OCC, caching data it has seen before, and potentially extra data it can predict it will need. When the internet connection is slow or unavailable the experiences may be able to provide partial functionality. 

This can still provide challenges as the devices potentially will have different offline experiences based on their prior online use. It may also be considered a bad idea to allow them to cache writes locally incase they aren’t guaranteed to come back online. They may be broken, lost, or stolen before reconnecting. 

As the name suggests, Occasionally Connected Servers, is a pattern for enabling servers to be deployed to locations to power the local experiences. These servers will cache data and content and serve it to local experiences. 


![High Level View of Microservices style. Services deployed centrally and remote, connected through Message Brokers and the internet](./images/occasionally-connected-servers/microservices-high-level-view.png)

The local experiences will therefore always be online to a backend service over the reliable local network. 
(Obviously no network is reliable but the LAN is normally reliable enough for mostd business use cases to accept)

This simplified the development
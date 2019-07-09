# system-design-notes-for-system-design-primer
my notes for system design 

## horizontally and vertical scaling 
  Horizontal scaling means that you scale by adding more machines into your pool of resources
  
  whereas Vertical scaling means that you scale by adding more power (CPU, RAM) to an existing machine.

## load-balancer

is responsible to distribute user requests (load) among the various back-end systems/machines/nodes in the cluster. Each of these back-end machines run a copy of your software and hence capable of servicing requests. This is just one of the various functions that load balancer may be performing. Another very common responsibility is "health-check" where the load balancer uses the "ping-echo" protocol or exchanges heartbeat messages with all the servers to ensure they are up and running fine.

The request-response can also be done in 2 different ways:

Load Balancer always acts as an intermediary program for every response 

In this case, once the request has been handed over to the server by the load balancer, any response from the server to the user will go through the load balancer. So the server machines that are actually servicing the request will never directly interface with the user machine running the client application. The machine hosting the load balancer program will be handling all the requests/responses to and from the user. 

Load Balancer does not act as an intermediary for the responses coming from the server machine -

In this case, once the server has received the request from load-balancer, it bypasses the load balancer and communicates it responses directly to the client. Setting up a cluster and load-balancer as a front-end interface to the client application does not really complete our scale-out architecture and design. There are still lots of critical questions to be answered and a number of key design decisions to be made which will affect the overall properties of our system.

## the first golden rule for scalability: every server contains exactly the same codebase and does not store any user-related data, like sessions or profile pictures, on local disc or memory. 

Sessions need to be stored in a centralized data store which is accessible to all your application servers. It can be an external database or an external persistent cache, like Redis. 

An external persistent cache will have better performance than an external database. By external I mean that the data store does not reside on the application servers. Instead, it is somewhere in or near the data center of your application server

But what about deployment? How can you make sure that a code change is sent to all your servers without one server still serving old code? This tricky problem is fortunately already solved by the great tool Capistrano. It requires some learning, especially if you are not into Ruby on Rails, but it is definitely worth the effort.


# Golang-gRPC-Microservices
Some code and code snippets on gRPC based microservices by Golang


## A gRPC server demo with all 4 kinds of API, along with a built-in web server for testing

### The gRPC server part
It supports 4 kinds of API:
Unary RPC

Server streaming RPC 

Client streaming RPC

Bidirectional streaming RPC

with option on Deadlines/Timeouts

#### Credit
The gRPC server part is adapted from https://github.com/simplesteph/grpc-go-course/tree/master/greet


### The web server part to test gRPC 
Yes, gRPC on http/2 and Web server on http in one application. The web server will bring UI form to test the gRPC methods.

#### Credit
The web server part is based on https://github.com/fullstorydev/grpcui. The author has a good introduction at https://bionic.fullstory.com/grpcui-dont-grpc-without-it/.

gRPCui is great, but its built-in web server solution is not well-known. I only found a simple example at https://gist.github.com/jhump/3b29dbc042b9ce97536680046202f066.


### The gRPC client in Golang
Nearly all tutorial on gRPC is too technical and make gRPC looks like an ugly toy, althouth it is much friendly than traditional Restful API.

So I wrappered the client code further to make it just look as a normal method. This will make the remote call looks much more natual.

#### Credit
The gRPC server part is adapted from https://github.com/simplesteph/grpc-go-course/tree/master/greet

### To put all together
To make the gRPC concepts and features clearly, I put all together in 1 application.

So far, they are using different ports. But I am trying to use the same port. 



 

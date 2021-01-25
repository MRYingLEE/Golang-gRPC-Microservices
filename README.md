# Golang-gRPC-Microservices
Some code and code snippets on gRPC based microservices by Golang


## A gRPC server demo with all 4 kinds of API, along with a built-in web server for testing

### The gRPC server part
It supports 4 kinds of API:
* Unary RPC
* Server streaming RPC 
* Client streaming RPC
* Bidirectional streaming RPC

with option on Deadlines/Timeouts

#### Credit
The gRPC server part is adapted from https://github.com/simplesteph/grpc-go-course/tree/master/greet


### The web server part to test gRPC 
Yes, gRPC on http/2 and Web server on http in one application. The web server will bring UI form to test the gRPC methods.

#### Credit
The web server part is based on https://github.com/fullstorydev/grpcui. The author has a good introduction at https://bionic.fullstory.com/grpcui-dont-grpc-without-it/.

gRPCui is great, but its built-in web server solution is not well-known. I only found a simple example at https://gist.github.com/jhump/3b29dbc042b9ce97536680046202f066.

#### Credit
The gRPC server part is adapted from https://github.com/simplesteph/grpc-go-course/tree/master/greet

### To put all together
To make the gRPC concepts and features clearly, I put all together in 1 application.


```go

	s := grpc.NewServer(opts...)
	greetpb.RegisterGreetServiceServer(s, &server{})
	// Register reflection service on gRPC server.
	reflection.Register(s)

	// Here goroutine is used to launch the gRPC server
	go func() {
		if err := s.Serve(lis); err != nil {
			log.Fatalf("failed to serve: %v", err)
		}
	}()

	grpcPort := 50051
	//----------------------------
	// Create a client to local server
	//----------------------------
	cc, err := grpc.Dial(fmt.Sprintf("127.0.0.1:%d", grpcPort), grpc.WithInsecure())
	if err != nil {
		log.Fatalf("failed to create client to local server: %v", err)
	}

	//----------------------------
	// Create gRPCui handler
	//----------------------------

	//target := fmt.Sprintf("%s:%d", filepath.Base(os.Args[0]), grpcPort)
	target := fmt.Sprintf("%s:%d", "127.0.0.1", grpcPort)

	// This one line of code is all that is needed to create the UI handler!
	fmt.Println(target)
	h, err := standalone.HandlerViaReflection(context.Background(), cc, target)
	if err != nil {
		log.Fatalf("failed to create client to local server: %v", err)
	}

	//----------------------------
	// Now wire it up to an HTTP server
	//----------------------------
	httpPort := 80
	serveMux := http.NewServeMux()

	fmt.Println("HTTP server started")
	serveMux.Handle("/grpcui/", http.StripPrefix("/grpcui", h))
	// register other handlers...
	serveMux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("content-type", "html")
		_, _ = fmt.Fprintf(w, `
				<html>
				<head><title>Example server</title></head>
				<body>
				<h1>Greeting!</h1>
				<p>Check out the gRPC UI <a href="/grpcui/">here</a>.</p>
				</body>
				</html>
			`)
	})

	ll, errr := net.Listen("tcp", fmt.Sprintf(":%d", httpPort))
	if errr != nil {
		log.Fatalf("failed to open listen socket on port %d: %v", httpPort, errr)
	}

	err = http.Serve(ll, serveMux)
	if err != nil {
		log.Fatalf("failed to serve HTTP: %v", err)
	}


```



 

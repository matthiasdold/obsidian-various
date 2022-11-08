## Some consideration
- The communication between modules would based of a few limited [[Inter Modul Communication#Primary Commands]]
	- Hence a plain JSON API via sockets should be sufficient for communication
	- More advance protocols could be implemented using [gRPC](https://grpc.io/docs/what-is-grpc/introduction/) or [Thrift](https://github.com/facebook/fbthrift) which would optimise the messages with their own serialisation techniques
		- This could be relevant once we need a large bandwidth for the communication, but probably less relevant for the general messaging flow
		- They provide server side reflection which would make them type-save, which could be an interesting aspect - see [this](https://betterprogramming.pub/building-a-grpc-server-with-rust-be2c52f0860e) for a comparison.



## Primary Commands
### I/O
#### Recording
#### Stimulation
### Decoding
### Paradigm
### Control
### Monitoring


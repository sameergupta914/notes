# Problems with REST

- JSON has issues like type checking
- semantic versioning required on API change
- difficulties during streaming
- by default support HTTP 1.1
- manual validations are required
- maintenance of client side libraries and implementations are required

# Features of gRPC

- default HTTP 2.0 support
- protocol buffers instead of JSON
- strict API contract
- built in code generators
- inbuilt load balancing
- unary, server, client and bidirectional streaming
- more secure and around 7x faster than REST

- how is it faster?
    - protobuf serializes and de-serializes data into binary thus reducing size of messages
    - uses HTTP 2.0
    - it can support streaming
    - support for load-balancing

- to use this name the file with ending `.proto`
- 
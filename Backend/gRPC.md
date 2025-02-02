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
- `syntax= "proto3";` written on top of the file.

# Protocol buffers or protobuf

- repeated -> for making an array
- field numbers -> used to identify fields in the binary encoded data which means they cant change from version to version of your service.  backward and forward compatibility are possible.
- naming are action basedd rather than service based


- `npm i @grpc/grpc-js`  ->add grpc npm in your folder
- `filename.proto`  -> create a file with .proto, here `todo.proto` ->

```proto
        syntax= "proto3"
        service TodoService{
            rpc CreateTodo(Todo) returns (Todo){}
            rpc GetTodo(TodoRequest) returns(Todo){}
            rpc ListTodos(Empty) returns(TodoList){}
        }
        message Empty{}

        message Todo{
            string id=1;
            string title=2;
            optional strign content=3;
        }
        message TodoList{
            repeated Todo todos=1;
        }
        message TodoRequest{
            string id=1;
        }
```

-  `index.js`   ->create a index file in which we'll write the following boiler plate ->

```javascript

    const grpc= require('@grpc/grpc-js');
    const todosProto= grpc.load('todo.proto');

    const server= new grpc.Server();

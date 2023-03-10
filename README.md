# Simple RPC message

This library implements a very simple RPC message system to be used in a text-based communication system.

## Message format

The messages are UTF-8 encoded strings, with parts split by line breaks (\n):
 
  - The first line is the message type (upper case string)
  - After it, the message can have an arbitrary number of parameters. Each parameter has a name, followed by a colon and it's value. Parameter names are case-insensitive.
  - Optionally, after the arguments, it can be an empty line, followed by the body of the message (an arbitrary string).

```
MESSAGE-TYPE
Request-ID: request-id
Auth: auth-token
Argument: value

{body}
```

## Usage

You can use the structure `RPCMessage` to create RPC messages. Use the `Serialize` method to serialize them to string, in order to send them.

You can parse a received RPC message with `ParseRPCMessage`.

After the message is parsed, you can access the structure properties: `Method`, `Params` and `Body`.

In order to get a parameter, since names are case-insensitive, you can use the `GetParam` method.

```go
package main

import (
  "fmt"

	// Import the module
	simple_rpc_message "github.com/AgustinSRG/go-simple-rpc-message"
)

func main() {
  message := simple_rpc_message.RPCMessage{
    Method: "TEST",
    Params: map[string]string{
      "Test-Param":   "Test-Value",
      "Test-Param-2": "Test-Value-2",
    },
    Body: "Test Body\nTest second line\nThird line",
  }

  // Serialize the message to string
  serialized := message.Serialize()

  // Parse serialized message
  recovered := simple_rpc_message.ParseRPCMessage(serialized)

  fmt.Println("Method: " + recovered.Method);
  fmt.Println("Params[Test-Param]: " + recovered.GetParam("test-param"));
  fmt.Println("Params[Test-Param-2]: " + recovered.GetParam("Test-Param-2"));
  fmt.Println("Body: " + recovered.Body);
}
```

## Compilation

In order to install dependencies, type:

```
go get .
```

To compile the code type:

```
go build
```

To order to run the tests, type: 

```
go test
```

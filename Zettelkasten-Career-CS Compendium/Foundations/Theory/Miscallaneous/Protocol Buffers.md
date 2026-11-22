Protocol Buffers (protobufs) is a language-neutral, platform-neutral data serialization format developed by Google.

# Why Protobufs Instead of JSON?

Example of Protobuf vs JSON

JSON:

```json
{
  "name": "Alice",
  "age": 25,
  "email": "alice@example.com"
}
```

Protobuf Definition (proto file):

```proto
syntax = "proto3";

message Person {
  string name = 1;
  int32 age = 2;
  string email = 3;
}
```

After serialization, Protobuf represents data in a compact binary format, which is faster and smaller compared to JSON.

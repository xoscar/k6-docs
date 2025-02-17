---
title: "Response"
head_title: 'gRPC.Response'
excerpt: 'The response object of a gRPC request.'
---

| Name | Type | Description |
|------|------|-------------|
| `Response.status` | number | The response gRPC status code. Use the gRPC [status constants](/javascript-api/k6-net-grpc/constants) to check equality. |
| `Response.message` | object | The successful protobuf message, serialized to JSON. Will be `null` if `status !== grpc.StatusOK`. |
| `Response.headers` | object | Key-value pairs representing all the metadata headers returned by the gRPC server. |
| `Response.trailers` | object | Key-value pairs representing all the metadata trailers returned by the gRPC server. |
| `Response.error` | object | If `status !== grpc.StatusOK` then the error protobuf message, serialized to JSON; otherwise `null`. |

### Example

<CodeGroup labels={["grpc-test.js"]}>

```javascript
import grpc from 'k6/net/grpc';
import { check, sleep } from 'k6';

const client = new grpc.Client();
client.load(['definitions'], 'hello.proto');

export default () => {
  client.connect('grpcb.in:9001', {
    // plaintext: false
  });

  const data = { greeting: 'Bert' };
  const response = client.invoke('hello.HelloService/SayHello', data);

  check(response, {
    'status is OK': (r) => r && r.status === grpc.StatusOK,
  });

  client.close();
  sleep(1);
};
```

</CodeGroup>

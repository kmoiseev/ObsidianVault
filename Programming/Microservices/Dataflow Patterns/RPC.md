 The RPC model tries to make a request to a remote network service look the same as calling a function or method in your programming language, within the same process.
 Points to consider:
-   Retry might be needed in the code calling RPC
-   No way to definetly understand status of your request if it fails on timeout
-   Idempotence must be inplemented in the side being called
-   Network request delay is inconsistent
-   All the parameters must be sent over the network
-   Might be problems when client and server are on different languages


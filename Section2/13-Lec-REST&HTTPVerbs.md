# REST - Representational State Transfer

1. Style of architecture for building web services.
2. Set of principles that define how web services should be design and interact with each other.
3. Based on concept of resource. A resource can anything that can be identified and manipulated through a web services
4. In RESTfull architecture resources are accessed through URLs. Each URI identified a specific resource
5. An HTTP verb are used to perform actions on these resources. For example, a get request can be used to retrieve a resource, while a post request can be used to create a new resource.
6. REST also emphasizes a stateless client server model. This means that the server should not store any client state between these requests and the client should provide all necessary information with each and every request. This allow for scalability and readability in the system as the server can handle multiple requests simultaneously without needing to maintain client state.

HTTP Verbs - HTTP verbs define the types of actions that can be performed on a resource identified by a URI
1. GET
2. POST
3. PUT
4. DELETE
5. PATCH
6. OPTIONS

Computers can only do CRUD at the end of the day. 
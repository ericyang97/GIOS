# READ ME - STUDENT












## ECHO CLIENT / ECHO SERVER

. This part of the project needs a socket communication between a client and server where the 
server echoes the message sent by the client.

. The specification mentions requirement of making the server/client IP agnostic meaning they
serve requests from both versions of Ipv4 and Ipv6 .

. Used the AF_UNSPEC specification in the addrinfo struct which can be passed on to the getaddrinfo
method to fetch all available address informations.
. The getaddrinfo method required 4 arguments where only one of the first two arguments be NULL.
Either the node or the service name parameter can be NULL.
. constructed a addrinfo struct with ai_family - AF_UNSPEC and socket type as SOCK_STREAM since 
we use the TCP protocol.
.To avoid memory leak, made sure that the address infos fetched are freed once connection/binding is successful.

## Transfer

. Understood the limitations in socket read / write and send / recv methods. Its not always the case read/write and send/recv may provide you with the requested amount of data.
. Therefore in order to completely send / receive data we have repeat the same.
.Since file size if unknown to both the server and the client, implemented a buffer with a arbitrary size
of memory allocated.
.Each iteration of read / send reads the file content upto the mentioned bytes making sure the bytes read is sent completely before doing the next.
. Similarly each iteration of receive / write receives the data of requested size and confirms the same amount of data is written in the file mentioned.
. To avoid address memory leaks, made sure no variables declared are left unused. Also, freed the allocated
memory for the buffer used for  read/write and send/receive.

## GFlib

.GFCLIENT:- Receiving data through a socket is handled in bytes. As suggested, the incoming data may or may not 
be null terminated. Therefore implemented the memset setting the buffer values to '\0' before starting to receive.
. Once, data is received, checked  each char value for '\r' and next consecutive value '\n' to make sure the response is received as expected.
. In order to capture the trailing bits of file content in the response header, implemented a char pointer to the buffer that can move upto the marker to start sending the file data to write from the position.
. Response from the server may not be same always , made sure to break the receiving loop if response from the server is ERROR or FILE_NOT_FOUND or INVALID.
. Since the server is expected to run indefinitely, in order to close the client connection once the request is processed, stored the file length from the server response header and kept updating the received data bytes value and when the filelength is reached, close the connection.

.GFSERVER:- Receive the client request in a while loop followed by a byte-by-byte check for the request end marker.
. Since the request is a string , used string functions strcmp and strncmp to check for scheme,method and filepath correctness. If failed, the request is deemed as invalid.

## MTGF

.Failed in 5 test cases.
Usage of memset to reset the char buffer each time after reading and sending data seems to be 
a bad idea while using multiple threads.



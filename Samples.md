### Samples

Included along with the source code on CodePlex are a few samples illustrating how to use some of the main features.  

**Bing Request**  
This sample is a simple command line application takes in a search term and the name of a file. It then constructs an HTTP request to www.bing.com to perform a search. The HTTP response is examined and the response body is written to a file. In this sample the HTTP client and asychronous file streams are utilized.  

**Search File**  
Search file is a command line application to illustrate the usage of a variety of the stream types in Casablanca. The application works by taking in as arguments the name of a file, a string to query for, and name of a file to write the results to. Asynchronous file streams are used to handle reading and writing to/from the files, container buffers are used to read and write strings to streams, and a producer consumer buffer stream is used to separate the logic of reading and writing files on to two different threads. The sample is cross platform working on both Windows and Linux.  

**Connect to Facebook**  
[Connecting to Facebook with the C++ REST SDK](http://blogs.msdn.com/b/vcblog/archive/2013/03/21/connecting-to-facebook-with-the-c-rest-sdk.aspx)  

**CasaLens**  
CasaLens is a data mash-up sample. It uses Casablanca features to build a service that collates data from different services and provides them to the user. Given the postal code or city name, it collects events, movies, weather, pictures around that place.  
The CasaLens service portion uses Casablanca http_listener to listen for requests, with GET and POST handlers. Upon receiving request for a city, it sends HTTP requests to multiple services, receives responses with JSON data. It aggregates the information and responds to the client's request with the JSON data.  

**Connect to Windows Live**  
A Windows store application illustrating how to connect and authenticate with Windows live services like SkyDrive and Hotmail.  

**Blackjack**  
A version of the Blackjack card game. Using the http_listener used to host tables and act as the dealer and http_client to join tables and play hands as individual players.
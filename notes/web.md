# How does the web work

- What is a network?
    - Two or more computers connected/linked together physically (cable) or wirelessly (WiFi, bluetooth)
- What is a router?
    - Networking device that receives, analyses, and forwards data packets (IP packets) across connected computer networks
    - Connects different networks together
    - When an IP packet arrives, the router will inspect the destination address, consult its routing tables to decide the optimal route to transfer the packet to the network it belongs to
- What are packets and how are they used to transfer data?
    - When data is sent across networks, they are sent in thousands of small chunks, which are what we call packets. Data is sent in small chunks rather than one large chunk because this allows the packets to be routed along different paths, instead of "clogging up" a path. This makes the exchange faster and allows multiple users to download the data at the same time, and also means that any data chunks that are corrupted or dropped can be replaced easier.
- What is the internet?
    - global system/infrastructure of interconnected computer networks
    - uses the Internet protocol suite (TCP/IP) to communicate between networks and devices
        - TCP/IP: Transmission Control Protocol and Internet Protocol - these define how data should travel across the internet
- What is an ISP (Internet Service Provider)?
    - Company that manages special routers that are all linked and can also access other ISPs routers
- What is an IP address (Internet Protocol address)?
    - Unique address (numerical label) that identifies a device connected to a computer network
    - IPv4 defines an IP address as a 32-bit number
    - IPv6 - 128 bits
- What is a client?
    - computer hardware (e.g. typical internet connected user devices like computer connected to WiFi) or software (e.g. web-accessing software like Chrome) that accesses a service made available by a server
- What is a server?
    - Computers that store webpages, sites, apps.
    - data is downloaded from the server onto the client machine when accessing a webpage
        - Client sends "request" and server sends back a "response"
- What is a web page?
    - A document which can be displayed in a web browser
- What is a website?
    - A collection of web pages that are grouped together (linked)
- What is a web server?
    - A computer that hosts one or more websites on the internet
- What is a web browser?
    - software that retrieves and displays web pages
- What is a search engine?
    - A special kind of website that helps users find other websites or webpages
- What is a DNS request?
    - DNS is Domain Name Server - like an address book for websites 
    - a domain name corresponds to an IP address - it provides a human-readable address which is generally easier to remember/find
    - When accessing a webpage using a domain name, the browser will check if the computer already recognises the IP address identified. If not, then it will ask an DNS server to get the corresponding IP address. Once that IP address is known, the browser will negotiate the contents with the web server of that webpage.


# How the web works
Client -> Request -> Server -> Response -> Client
- Domain and IP
- Client and Server
- Request and Response

# HTTP, HTTPS

Hyper Text Transfer Protocol 
- A Protocol for Transferring Data which is understood by Browser and Server
- application protocal that defines a language for clients and servers to communicate

Hyper Text Transfer Protocol Secure - HTTP + Data Encryption (during Transmission)

A `HTTP request` is made to a server when a client needs to access a resource or a action is required. 

The request includes:
- a URL identifying the target server and resource
- a method that defines the required action 
	- `GET` - get a specific resource
	- `POST` - Create a new resource
	- `HEAD` - Get the metadata information about a specific resource without getting the body like `GET`. E.g. retrieve when the last time a resource was changed before doing a `GET` if required as that is "more expensive"
	- `PUT` - update an existing resource
	- `DELETE` - delete the specified resource
	- `TRACE`, `OPTIONS`, `CONNECT`, `PATH` are less common
- additional information encoded 
	- URL parameteres (`GET` query string)
	- POST data
	- client-side cookies that contain session data about client (e.g. keys the server can use to determine login status)

The body of a successful response contains:
	- a HTTP response status code
	- the (body of) requested resource (e.g. new HTML page, image etc)
## HTTP Response Codes
Here are some common status codes:
- 1xx - Informational
- 2xx - Success
    - 200 OK
    - 201 OK, created
- 3xx - Redirect
    - 301 Moved permanently
- 4xx - Client-side error
    - 401 Unauthorized
    - 403 Forbidden
    - 404 Not Found
    - 422 Unprocessable Entity
    - 429 Too Many Requests
- 5xx - Server-side error
    - 500 Internal Server Error


# Html Forms

- `enctype` attribute specifies how the form data should be encoded when submitted.
    - Default is `application/x-www-form-urlencoded` - All characters are encoded (spaces converted to +, special characters converted to ASCII HEX values)
    - `multipart/form-data` - required if uploading files through the form
    - `text/plan` - no encoding so not recommended
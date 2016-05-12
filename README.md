# GoREST
Team project for SJSU - CMPE273 - Prof. Sithu Aung's Class

Team: GoRest 
Project Title: Service Level Agreement

Members: 
i.  Shounak Gujarathi
ii.  Saikrishnan Baskaran
iii. Kartik Patil
iv.  Sivakumar Sivaraman

Technology used: Node.js using Express framework

******************************SERVER******************************************************************************************************
Health features that are monitored of the server:

1. CPU Usage of the complete PC
2. CPU Usage of the server application (node.exe in this case)
3. Memory consumption of the server
4. Memory Available in the server
5. Rate of connections (number of connections per minute)
6. Current number of active connections (Used for concurrency check at the server side)

Server provides these facts to the client via, rest end points:

get /Monitor(returns values to the client in the body, used for GUI) and get /ClientCall (returns values to client in the header)

The health check is done every 2 seconds to get the latest values.
Whenever a client requests the health stats at one of the end points,
the lastest facts are provided by the server.
The client can then use this information to readjust it's behaviour.

On the server side, throlling or concurency check is maintained.
If more than 100 concurrent connections are detected, then the 101st 
connection is denied service (Status Code: 503) with a error message saying
"Too many concurrent connections, please try again."

The working of server is coded in the js files of the project. More particularly the monitoring logic is given in index.js.

******************************Intelligent Client********************************************************************************
Client uses the information provided by the server to implement the remedial logic.
How this works is that in case the server's performance is going down,
i.e, i.   Used Memory > Available Memory
     ii.  CPU Usage > 80%
     iii. High number of current connections, etc.
then, client will take remedial actions like,
       i.   Decreasing the number of retires per minute (To not clog the server)
       ii.  Increasing the timeout (So that the server can take more time to process the request before it being ruled as failed)
       iii. Increasing the number of retries, etc.

The client also consists of a Circuit Breaker that trips when there failures are detected consecutively.

The working of this client is demonstated in the RestClientFinal.html file.

****************************Testing and Monitoring******************************************************************************
We've used newrelic.com's API to monitor the health of the server and provide us with graphical representation of
i.   Throughput
ii.  Transaction Time
iii. Apdex Score, etc.
This API provided a good way for us to tell when our server health varies and what values of CPU usage, connection rate etc. must be used.

****************************Dynamic UI******************************************************************************************
A dynamic view of system health, updated every 2 seconds can be seen using the UI. The code can be found in index.html.



Project contribution:
1. Shounak Gujarathi (Architecture and Server-side implementation) 
2. SaiKrishanan Bhaskaran (Client Side implementation including the circuit breaker) 
3. Kartik Patil (Client side remedial logic, New Relic integration) 
4. Sivakumar Sivaraman (Dynamic UI, Ajax integration)

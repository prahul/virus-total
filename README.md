# Virus Total System Design
This repository discusses an approach of System Design for a publicly available product like Virus Total

## Functionality covered
* Allows users to upload files to be scanned by multiple AV engines and metadata extraction scripts.
* Engines and scripts may perform a wide range of operations such as: run a proprietary virus scanner, extract metadata from file headers, make calls to external service endpoints, execute uploaded files to observe their behavior, etc.
* Stores metadata and AV engine results about uploaded files. 
Retrieves metadata about uploaded files (including file attributes, metadata and AV engine results).
* Here is an example of metadata collected from file uploads: https://www.virustotal.com/en/file/0bf01094f5c699046d8228a8f5d5754ea454e5e58b4e6151fef37f32c83f6497/analysis/ 
* Ensures all major functionality is available via both a user interface and a public-facing APIs that users can build apps on top of.

## System requirements and constraints
* The system will support millions of users per day.
* Uploaded files will range in size from 100K to 1GB.
* Uploaded files will be retained as long as possible (ideally forever).
* The system provide results as close to real-time as possible.
* Metadata and scanning services can run on a mixture of Linux or Windows (not all scanning services support the same OS).
* Design will be able to accommodate the addition of new AV engines or metadata scripts.  These additions should be as minimally invasive to the running system as possible (i.e. minimal or zero system downtime, no “big bang” deployments, etc.).

## System design
<img width="1002" alt="image" src="https://user-images.githubusercontent.com/2245716/186568721-e29367ef-c8fc-409e-b542-ec222895e4dd.png">

## Technology components
* Containerization: Docker, Kubernetes, OpenShift
* Event backbone: Kafka
* Registry: Artifactory
* Datastore: MaongoDB, Big Data/Datalake, AI/ML
* Search engine: SOLR
* Storage: Object
* CI/CD pipeline: Jenkins
* Cache: CDN, Redis
* Monitoring: Dynatrace, Runbook Automation, ServiceNow
* Logging: Humio log aggregation, Jaeger distributed tracing, Kibana Dashboard
* Load balancer: F5, API Gateway
* Application security: OAUTH (jwt token), Istio Service Mesh
* Governance: Open Policy Agent, Sonar
* Programming language: Java, Python, Groovy, Angular
* Testing: Spock, Karate, Jmeter, Consumer contract
* Operating System: Linux
* API: REST, 12 Factor, Swagger

## Architecture and design considerations
### User interface
* Usability and UI design must be done for developing the components
* UI can be angular/react based components deployed on a containerized environment
* It must be able to support atleast n-2 versions of major browsers like Chrome, Safari, Firefox, Edge
* Multipart upload features of the UI/script must be used for uploading large file for scanning

### Think API
* Use the REST API for microservices and streaming events
* Use 12 factor principles of API development
* Use JSON for microservices and Avro for events
* Use containerized approach which can be horizontally sclable for load and performance

### Security
* Protect the service components using OAUTH and Service Mesh
* System must ne protected behind a firewall

### Availability, scalability and fault tolerance
* Use load balancer and API Gateway to secure and manage traffic.
* Use Circuit breaker and rate limting patterns
* Use CQRS pattern for design of components
* Use Cloud Native principles in the overall design and architecture
* Use monitoring and show metrics to surface the golden signals
* CI/CD automation, testing, governance
* Have an active / warm data center architecture
* Consider using Content Delivery Network (CDN) for access for the system from different regions
* Setup auto deplication between data centers / regions. 
* Use features like retries, redo logs and software fault toleration mechanism to ensure system is not in inconsistent state due to failures.

### Data driven
* Make a data driven architecture to manage able to manage the volume, velocity, value, variety and veracity of data
* Use horizontally scalable schema less database
* Use object storage to have infinite storage capacity
* Use data catalog and search engines to flatten and search data
* Use BigData/Datalake for storing the data and building data pipelines, Analytics , Machine Learning models and AI
* File formats like csv, json, avro, parquet can be used

## API
REST API and microservices development 12 factor principles to be used

### Register user
##### path: <br/>
POST https://www.myvirustotal/api/v1/users/register

##### request: <br/>
<pre>
{  <br/>
  "firstname":"fname",  <br/>
  "lastname":"lname",  <br/>
  "username":"uname",  <br/>
  "password":"pwd",  <br/>
  "email":"my@email.com"  <br/>
}
</pre>
##### response: 200 OK <br/>
<pre>
{ <br/>
  "status":"SUCCESS", <br/>
  "error":{}, <br/>
  "responsedata":{ <br/>
  "userid":"12345", <br/>
  "username":"uname",  <br/>
  "token":"ABVVxTPG", <br/>
  "data":{} <br/>
  } <br/>
}
</pre>
### Login user
##### path: <br/>
POST https://www.myvirustotal/api/v1/users/login

##### request: <br/>
<pre>
{  <br/>
  "username":"uname",  <br/>
  "password":"pwd",  <br/>
  "loginip":"255.255.255.255",  <br/>
  "password":"pwd"  <br/>
}
</pre>
##### response: 200 OK <br/>
<pre>
{ <br/>
  "status":"SUCCESS", <br/>
  "error":{}, <br/>
  "responsedata":{ <br/>
  "username":"uname",  <br/>
  "token":"ABVVxTPG", <br/>
  "refreshtoken":"ABVVxTPG", <br/>
  "data":{} <br/>
  } <br/>
}
</pre>
### Upload file
##### path: <br/>
POST https://www.myvirustotal/api/v1/files/upload

##### request: <br/>
<pre>
{  <br/>
  "filename":"abc.txt",  <br/>
  "filetype":"txt",  <br/>
  "filesize":"100",  <br/>
  "username":"uname"  <br/>
}
</pre>
##### response: 200 OK <br/>
<pre>
{ <br/>
  "status":"SUCCESS", <br/>
  "error":{}, <br/>
  "responsedata":{ <br/>
  "uploadid":"12345",  <br/>
  "data":{} <br/>
  } <br/>
}
</pre>

### File upload status
##### path: <br/>
GET https://www.myvirustotal/api/v1/files/upload/status/{uploadid}

##### request: <br/>

##### response: 200 OK <br/>
<pre>
{ <br/>
  "status":"SUCCESS", <br/>
  "error":{}, <br/>
  "responsedata":{ <br/>
  "uploadid":"12345",  <br/>
  "filename":"abc.txt",  <br/>
  "filesize":"100", <br/>
  "uploadsize":"50", <br/>
  "elapsedtime":"100", <br/>
  "data":{} <br/>
  } <br/>
}
</pre>
### Scan status
##### path: <br/>
GET https://www.myvirustotal/api/v1/files/scan/status/{uploadid}

##### request: <br/>

##### response: 200 OK <br/>
<pre>
{ <br/>
  "status":"SUCCESS", <br/>
  "error":{}, <br/>
  "responsedata":{ <br/>
  "uploadid":"12345",  <br/>
  "filesize":"100", <br/>
  "filename":"abc.txt",  <br/>
  "scans":[ <br/>
     { <br/>
       "scanid":"12345", <br/>
       "securityvendor":"Google", <br/>
       "scanpercent":"10", <br/>
       "elapsedtime":"100", <br/>
       "scanstatus":"complete",
       "virusdetectection":["Malware","Trojan"] <br/>
     }, <br/>
     { <br/>
       "scanid":"12346", <br/>
       "securityvendor":"McAfee", <br/>
       "scanpercent":"20", <br/>
       "elapsedtime":"105", <br/>
       "virusdetectection":["ML"] <br/>
     }, <br/>
     { <br/>
       "scanid":"12347", <br/>
       "securityvendor":"WebRoot", <br/>
       "scanpercent":"15", <br/>
       "elapsedtime":"115", <br/>
       "scanstatus":"complete",
       "virusdetectection":["Trozan","Yandex"] <br/>
     }, <br/>
     { <br/>
       "scanid":"12348", <br/>
       "securityvendor":"BigDefender", <br/>
       "scanpercent":"20", <br/>
       "elapsedtime":"125", <br/>
       "scanstatus":"complete",
       "virusdetectection":[] <br/>
     } <br/>
  ], <br/>
  "data":{} <br/>
  } <br/>
}
</pre>
### Search scan
##### path: <br/>
GET https://www.myvirustotal/api/v1/files/scan/search/{uploadid}

##### request: <br/>

##### response: 200 OK <br/>
<pre>
{ <br/>
  "status":"SUCCESS", <br/>
  "error":{}, <br/>
  "responsedata":[{ 
     { <br/>
        "uploadid":"12345",  <br/>
        "filesize":"100", <br/>
        "filename":"abc.txt",  <br/>
        "scans":[ <br/>
         { <br/>
           "scanid":"12345", <br/>
           "securityvendor":"Google", <br/>
           "scanpercent":"10", <br/>
           "elapsedtime":"100", <br/>
           "scanstatus":"complete",
           "virusdetectection":["Malware","Trojan"] <br/>
         }, <br/>
         { <br/>
           "scanid":"12346", <br/>
           "securityvendor":"McAfee", <br/>
           "scanpercent":"20", <br/>
           "elapsedtime":"105", <br/>
           "virusdetectection":["ML"] <br/>
         }, <br/>
         { <br/>
           "scanid":"12347", <br/>
           "securityvendor":"WebRoot", <br/>
           "scanpercent":"15", <br/>
           "elapsedtime":"115", <br/>
           "scanstatus":"complete",
           "virusdetectection":["Trozan","Yandex"] <br/>
         }, <br/>
         { <br/>
           "scanid":"12348", <br/>
           "securityvendor":"BigDefender", <br/>
           "scanpercent":"20", <br/>
           "elapsedtime":"125", <br/>
           "scanstatus":"complete",
           "virusdetectection":[] <br/>
         } ],<br/>
       "data":{} <br/>
       } 
   }]<br/>
}
</pre>

## Data model

### User
<pre>
{ <br/>
 "userid":"12345", <br/>
 "firstname":"fname",  <br/>
 "lastname":"lname",  <br/>
 "username":"uname",  <br/>
 "password":"pwd",  <br/>
 "email":"my@email.com",  <br/>
 "token":"ABVVxTPG", <br/>
 "registertype":"R", <br/>
 "registerip":"255.255.255.255", <br/>
 "quota":"15" <br/>
}
</pre>
### Filemetadatascaninfo
<pre>
{ <br/>
 "uploadid":"12345", <br/>
 "filename":"abc.txt",  <br/>
 "filetype":"txt",  <br/>
 "filesize":"100",  <br/>
 "username":"uname",  <br/>
 "filemetadata":{},  <br/>
 "scans":[ <br/>
      { <br/>
       "scanid":"12345", <br/>
       "securityvendor":"Google", <br/>
       "scanstatus":"complete",
       "elapsedtime":"200", <br/>
       "virusdetectection":["Malware","Trojan"], <br/>
       "data":{} <br/>
     }, <br/>
     { <br/>
       "scanid":"12346", <br/>
       "securityvendor":"McAfee", <br/>
       "scanstatus":"complete",
       "elapsedtime":"250", <br/>
       "virusdetectection":["ML"], <br/>
       "data":{} <br/>
     }, <br/>
     { <br/>
       "scanid":"12347", <br/>
       "securityvendor":"WebRoot", <br/>
       "scanstatus":"complete",
       "elapsedtime":"220", <br/>
       "virusdetectection":["Trozan","Yandex"], <br/>
       "data":{} <br/>
     }, <br/>
     { <br/>
       "scanid":"12348", <br/>
       "securityvendor":"BigDefender", <br/>
       "scanstatus":"complete",
       "elapsedtime":"210", <br/>
       "virusdetectection":[], <br/>
       "data":{} <br/>
     }, <br/>
 ], <br/>
 "data":{} <br/>
}
</pre>

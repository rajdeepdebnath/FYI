# Backend Link Shortener
This document describes details about Backend of a Link Shortener project

## Tech stack
### My preferred way
1. Development and running framework: Asp.Net core 2.2
2. Language: C# 7.0
3. Unit testing framework: MSTest
4. Database: Mongo DB Atlas on Azure
5. Logging: Elasticsearch, Logstash, Kibana (ELK) using own library
6. Deployment (CI/CD) pipeline: Azure devops pipeline
7. Hosting (Web server): Azure Web app (HTTPS)
8. Code repository: Github / Bitbucket

### Node.js way
1. Development and running framework: Node.js 12.16, express, mongoose
2. Language: Javascript (Common Js syntax, not ES6)
3. Unit testing framework: Mocha and Chai
4. Database: Mongo DB Atlas on AWS
5. Logging: Elasticsearch, Logstash, Kibana (ELK) using own library
6. Deployment (CI/CD) pipeline: AWS CodePipeline & CodeDeploy
7. Hosting (Web server): AWS Elastic Beanstock (HTTPS)
8. Code repository: Github / Bitbucket

## Pros and Cons (feedback)

## Coding specification
### Term explain
1. 'Internal user' means who wants to use our url shorten service
2. 'Social media user' means who will click on shortened url.

### Flow
1. Internal user will call our Token api by providing username and password by POST REST Http request to receive new token.
2. Internal user will call our Url shorten api by providing the tokan as Authorization Http header and url as POST body. Content type shuld be text/plain.
3. Url shroten and Report Api will be preceeded by a middleware which will validate the token.
4. Our system will receive the Url shorten request and check for User and existing url validation and perform the shortening if validation passed.
5. Afterthat result will be returned with Content type text/plain with Http status code 200. In case of any error Http status code 500 should be returned. In case of mal formed url, like not having http or https, error description should be returned with status code 400.
6. Internal user will call our report api with valid token and parameter as Get request. Report data will be returned as json body with content type appliation/json.
7. External user will click on the shortened link. The Http request will hit our web server. Express router should redirect any url of structure shortened to our Url api/controller. 
8. Url controller will return original url with Http status code 301 - permanent redirection.

### Application Architecture
1. 3 Tier: Controller, Service and Repository
2. Controllers: Token, UrlShorten, Url, Reporting
3. Service: Token, UrlShorten, Url, Reporting
4. Repository: Token, UrlShorten, Url, Reporting
5. Operations: CRUD as applicable mapped to HTTP Verbs (Get, Post, Put, Delete)
6. Communication flow: request will be routed to proper controller. Controller will do input validation and invoke proper service method. Service will fetch data by repository method and carry on required business logic work and return the result. Repository will talk to Database to fetch data and return result to Service method.
7. Token controller: User will submit Username and Password via post method to obtain Token. Token will be created with expired time set.
8. UrlShorten controller: Valid token is required to access UrlShorten controller endpoint. Perform url format validation, like must have http or https etc.
9. UrlShorten service: 
  a. The UrlShorten service will validate all the business logic validations like 1. If same user is requesting for same url more than once, the existing converted url will be returned, 2. If two different users are requesting for url shortening for same url, two different shortened url will be returned. 
  b. The UrlShorten create method will generate an alphanumeric 7 digit code and check in database if the code does not exist. The original url will be stored against this code. The code will be appended to our shorten url string with a leading front slash. The resulting string will be returned to the user.
10. Url controller: Upon receiving the code, the Url controller will fetch the original url from database by Url Service and repository. And the return the original url with http status code '301' - permanent redirection. This request will be stroed in the database with time stamp for reporting purpose.
11. Url repository: To increase performance, cache the data for configured duration in memory redis cache by code.
12. Report Cotroller: Will only expose Get method.

### Database schema
1. Table 1: Url
  1. UrlId: int, primary key
  2. ShortenId: varchar(7) 
  3. Original Url: varchar(2048)
  4. UserId: int, foreign key to user table
  5. CreatedOn: datetime
  6. IsActive: boolean
  
 2. Table 2: Reporting
  1. Id: int
  2. UrlId: int, foreign key to Url table
  3. Click: smallint
  4. VisitorData: varchar(100) like user agent, device type etc.
  5. CreatedOn: datetime
  
 3. Table 3: User
  1. Id: int, primary key
  2. username: varchar(20)
  3. passwordhash: varchar(40)
  4. IsActive: boolean

### Data capacity model
##### Url table
1. As per requirement, max url shorten request will be 10000 per day.
2. column wise:
  1. UrlId = 4 byte (32 bit)
  2. ShortenId = 7 byte
  3. Original Url = 200 byte, on an average mostly url will be within 200 characters
  4. UserId = 4 byte
  5. CreatedOn = 8 bytes
  6. IsActive = 1 byte
3. Total = 224 byte for single url.
4. @ 10000/day url => 224 * 10000 = 2240 kb / day
5. Monthly => 2240 * 30 = 67200 kb = 6.72 Mb
6. Yearly => 6.72 * 12 => 134.4 Mb

##### Reporting table
1. As per requirement, max 100,000 visitors clicks per day.
2. column wise:
  1. Id = 4 byte (32 bit)
  2. UrlId = 4 byte
  3. Click = 4 byte
  4. UserId = 4 byte
  5. VisitorData = 100 bytes
  6. CreatedOn = 8 bytes
3. Total = 124 byte for single url.
4. @ 10000/day url => 124 * 100,000 = 12.4 Mb / day
5. Monthly => 12.4 * 30 = 372 Mb
6. Yearly => 372 * 12 => 4.5 GB

### Non functional requirement
1. Performance: As per requirement, daily throughput will be max 100,000 read requests and 10000 create requests per day. Azure Web app or AWS Elastic Beanstock plan can take care of this. With normal web server setup any request should not take more than 300 ms. Asynchronous programming model requires to be followed.
2. Availability: Azure Web app or AWS Elastic Beanstock plan can take care of this.
3. Scalability: Not a concern as we can expect uniform load. But in case sudden increase of load, scale up Azure or AWS plane will take care of this.
4. Code maintainability: To ensure high coding standard linters or code analyzer requred to be used. Coding guideline needs to be defined and followed. Sonar qube quality check requires to be done. Unit test code coverage should be around 90%.
5. Logging: Technical exception requires to be logged with stacktrace and input data. Business exception also requires to be logged.  Logging requires to be set at warning level but can be changed from environment variable, else logging may overwhelm the storage. 
6. Caching: To increase performance, cache the data for configured duration in memory redis cache.
7. QA: Testing required to be done in all major browser and device combinations. Testing service provider like CrossBrowserTesting, SauceLab can be used.



## Project management
1. Tracking tool: Jira/Asana/Trello (i prefer Asana or Trello for simple project)
2. Environment: Dev, QA, Staging, Production
3. Procurement: 
  a. AWS or Azure subscription, 
  b. Development machine, monitors and other required accessories
  c. Buy shortend domain lin.ks
4. Sprint duration: 1 week
5. Sprint flow: 
  1. Day 1 first-half: Planning and estimation, 
  2. Day 1 first-half to Day 3: Development, 
  3. Day 4 day start: release to QA and QA testing
  4. Day 4 first half: complete development with bug fixing
  5. Day 4 second half: release to staging and demo. Product backlog, Technical backlog grooming.

## WBS in more detail:

### Estimation
1. Resource: 1 Dev
2. 1 day = 6 productive hours
3. Development machine setup with installation of required softwares: 1 day
4. All the environments setup, including repository: 1 day
5. CI\CD pipeline setup across environment: 1 day
6. Database development: 1 day
7. Application architecture setup: 1 day
8. Token development: 1 day
9. UrlShortener: 2 day
10. Url: 1 day
11. Reporting: 1 day
12. QA: 30% of dev effort = 3 days aprox


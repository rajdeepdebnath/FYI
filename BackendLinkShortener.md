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



## Coding specification
### Application Architecture
1. 3 Tier: Controller, Service and Repository
2. Controllers: Token, UrlShorten, Url, Reporting
3. Service: Token, UrlShorten, Url, Reporting
4. Repository: Token, UrlShorten, Url, Reporting
5. Operations: CRUD as applicable mapped to HTTP Verbs (Get, Post, Put, Delete)
6. Communication flow: request will be routed to proper controller. Controller will do input validation and invoke proper service method. Service will fetch data by repository method and carry on required business logic work and return the result. Repository will talk to Database to fetch data and return result to Service method.
7. Token controller: User will submit Username and Password via post method to obtain Token. Token will be created with expired time set.
8. UrlShorten controller: Valid token is required to access UrlShorten controller endpoint. 
9. UrlShorten service: 
  a. The UrlShorten service will validate all the business logic validations like 1. If same user is requesting for same url more than once, the existing converted url will be returned, 2. If two different users are requesting for url shortening for same url, two different shortened url will be returned. 
  b. The UrlShorten create method will generate an alphanumeric 7 digit code and check in database if the code does not exist. The original url will be stored against this code. The code will be appended to our shorten url string with a leading front slash. The resulting string will be returned to the user.
10. Url controller: Upon receiving the code, the Url controller will fetch the original url from database by Url Service and repository. And the return the original url with http status code '301' - permanent redirection. This request will be stroed in the database with time stamp for reporting purpose.
11. Url repository: To increase performance, cache the data for configured duration in memory redis cache by code.

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
3. Procurement: AWS or Azure subscription, Development machine, monitors and other required accessories
4. Sprint duration: 1 week
5. Sprint flow: 
  1. Day 1 first-half: Planning and estimation, 
  2. Day 1 first-half to Day 3: Development, 
  3. Day 4 day start: release to QA and QA testing
  4. Day 4 first half: complete development with bug fixing
  5. Day 4 second half: release to staging and demo. Product backlog, Technical backlog grooming.

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


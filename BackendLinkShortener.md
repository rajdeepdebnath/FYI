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
9. UrlShorten service: The UrlShorten service will validate all the business logic validations like 1. If same user is requesting for same url more than once, the existing converted url will be returned, 2. If two different users are requesting for url shortening for same url, two different shortened url will be returned. The UrlShorten create method will 



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


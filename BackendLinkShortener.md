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
7. Hosting (Web server): Azure Web app
8. Code repository: Github / Bitbucket

### Node.js way
1. Development and running framework: Node.js 12.16, express, mongoose
2. Language: Javascript (Common Js syntax, not ES6)
3. Unit testing framework: Mocha and Chai
4. Database: Mongo DB Atlas on AWS
5. Logging: Elasticsearch, Logstash, Kibana (ELK) using own library
6. Deployment (CI/CD) pipeline: AWS CodePipeline & CodeDeploy
7. Hosting (Web server): AWS Elastic Beanstock
8. Code repository: Github / Bitbucket


## Coding specification
### Application Architecture
1. 3 Tier: Controller, Service and Repository
2. Controllers: Token, Url, Reporting
3. Service: Token, Url, Reporting
4. Repository: Token, Url, Reporting
5. Operations: CRUD as applicable mapped to HTTP Verbs (Get, Post, Put, Delete)
6. Communication flow: request will be routed to proper controller. Controller will do input validation and invoke proper service method. Service will fetch data by repository method and carry on required business logic work and return the result. Repository will talk to Database to fetch data and return result to Service method.

## Project management
###

## (1) Similar Avatar Matching API microservice
See 05 first (AvatarServices)[Back-05-AvatarServices.md] 

### Who?
Test assisgments for the last year university students.

### Goal
Build micro-service for matching profiles across different networks based on scrapped/crawled avatar pictures

#### Key moments
* Main networks: Linkedin, Facebook, Github, Twitter
* Data source: Scrapping through Chrome Extension 
* Node estimation: 100,000 ~ 10,000,000 
* Format (jpg, gif, png, etc), blur, cut&crop agnostic 
* Preferred stack: Docker, Golang, Postgres
 
#### API Features
* Batch profiles processing 
* Expected response time: <.2 sec (take full advance of memory and golang goroutines if neccassary)

Image footprint matching is the main source of truth. Meta info scrapped by Chrome Ext will be optional. Important is to keep simple and mantainable. Maybe convert images to special hash and later assess similarity.

The need for API is based on the fact that most of Sales and HR researcher teams are looking at the same social profiles over and over again. This microservices will be used for showing similar profiles seen in the past.

### Research Papers
* User profile matching in social networks Elie Raad, Richard Chbeir, Albert Dipanda 
https://hal-univ-bourgogne.archives-ouvertes.fr/hal-00643509/document

### Related People Search APIs
* Fullcontact, social profile lookup by email or twitter https://www.fullcontact.com/developer/docs/person/
* Betaface API https://www.betafaceapi.com/wpa/


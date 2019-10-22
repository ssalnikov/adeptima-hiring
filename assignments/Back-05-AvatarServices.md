### AvatarServices
Test assisgments for the last year university students.
Avatar Dublicates and Similarity Search Services

Using the following repos:
* https://github.com/patrickmn/go-cache
* https://github.com/DataWraith/vptree
* https://github.com/corona10/goimagehash


### Steps
Each Avatar associated with unique ID and TYPE as we have multiple table associated with TYPE. For example, contact, company, cash social profile with corresponding unique ID

1. Get various type of hashes for each goimagehash

2. Create Vptree in-memory index with go-cache

3. Add Batch Adding
  Typical use case is to feed AvatarServices with 20 avatars from twitter or github user search 

4. Design Deduplication Service. Beside 100% matching, they will be picture Similarity. You have to think how to represent the data for Admin. It can be Sorting, Grouping, False Positive scenarios. Do your own research! 


Use gRPC
https://grpc.io/docs/guides/

REST 
https://echo.labstack.com/

### Test Data Sets 
- Partial dump of 2 tables in cvs or json format 
- Folder with 1Gb of avatars

### Twitter API
https://brain-twitter-api.join-us-today.com/api/twitter?query=Ilya%20Sidorov

Service should be Dockerized https://www.docker.com/
So please include Dockerfile in the result repo
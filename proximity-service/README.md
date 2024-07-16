# GeoLocation-design


# Requirements
**Functional**

* Given a user's location in search radius as inputs, return all businesses within the search radius. Yelp is example of this use case.
* Business owners can add, update and delete a business. These changes no need to appear in real time, might take some time to appear.
* Users can see detailed view about a business

**Non Functional**
* latency should be low.
* Users should able to find nearby businesses quickly.
* Service should be highly available.
* It should be able to handle traffic spikes during paek hours.

# Statistics or Estimations
**Traffic**
* Assume we have 100M Daily Active Users(DAU).
* 200M businesses.
* 5 Queries per day per user.
* So, Maths for Number of requests per second is 5000/sec (100M*5/100k) approx.

**Storage**
* Suppose storage takes 1 KB for one business. 
* 200M Businesses*1KB = 200GB
* For geospatial table 24bytes for one record, 200M*24 bytes = 5GB

How much storage to hold for 200M users?

# Design
**API Design**
* Search businesses
    - GET v1/search/nearby
    - Request : Lattitude, longitude and radius(default 3 Miles)
    - Response : {"total": 10, "businesses":[{business object}]}

* Return detailed information of business 
    - GET v1/businesses/{id}

* Add a business
    - POST v1/businesses

* Update a business
    - PUT v1/businesses/{id}

* Delete a business
    - DELETE v1/businesses/{id}

**Data Schema**

- Two key database tables
    - Business
        - business_id   PK
        - address
        - city
        - state
        - country
        - lattitude
        - longitude
    - Geospatial Index
        - location - can be told as geohash
        - business_id

**HLD**
* In our design, the load balancer distributesincoming traffic across the two services based on API routes. Both services are stateless.
* Behind load balancer we have two services. location based service and business service.
* Location based service 
    - Read heavy with no write requests at all.
    - The QPS is high 5000 QPS is our estimate.
    - Service is staeless.
    - Performs read operation in **Secondary/Replicas Database**
* Business service
    - It handles the CRUD requests for managing operation.
    - QPS is not so high.
    - The changes do not need to reflect real time.
    - Performs write operation in **Primary Database**


- One idea to design, build indexes on latitude and longitude.
- Two approach for geospatial indexing.
    - Hash -> Even Grid, Geohash(Keep diving whole world in smaller grid, start by dividing whole world with 4 quadrants) and Cartesian Tiers
    - Tree -> QuadTree, Google S2 and Rtree
    



# Challenges


# Databases
* What database to use to power the location search?
- GIS from redis

# Architecture

# References
[https://www.youtube.com/watch?v=L7LtmfFYjc4](https://www.youtube.com/watch?v=M4lR_Va97cQ) 11:10
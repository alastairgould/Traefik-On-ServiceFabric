
# Notes

## Flexible Routing

### Urls
Multiple urls rules can be used per service. For example very different
urls could be matched and sent to the same service.

For example

cluster-url.com/vouchers-commerce/vouchers

cluster-url.com/vouchers-finance/vouchers

could both go the same service. We are currently stuck with a strict
convention of  {domain}-{service}/{service}

We had to hack around this convention on rooks by deploying the same 
service multiple times in order to get multiple urls to match.

### Headers 
You can match on headers which allows you to redirect users to
different servers based on their headers which includes cookies.

Allows for a powerful release technique discussed in the next slide.

### Query Params
Could implement service partitioning based on query params if you wanted.

## Deployment Techniques

### Canary Release - Arbitrary percentage
Could move release to a small percentage of users, then gradually
expand to more users

### Canary Release - Headers
You can redirect users to different microservices based on their headers(includes cookies).
So you could logon as a test user in production which sets a test user cookie. You could then
manually validate the new microservice in production without effecting any other user.
Then release to everyone. If the new microservice fails validation in production then
no customer would be effected.

Would require getting the clients to pass certain
headers to the microservice

Testing in production 

## Robust
Has tons of features i'm not even mentioning

## How does it work

### Entrypoint 
Normally a port + SSL config

### Frontends
Are a collection of rules which match requests. When matched it gets forwarded
to a backend

You can match urls, headers, host etc

### Backends
Backends are responsible to load balancing incoming traffic to a set of
http servers. You can define load balancing methods like round robin, 
dynamic(performance), add circuit breakers to avoid overwhelming servers which 
are in a broken state. You can define what percentage of traffic goes to which
backend for canary releases

### Dynamic labels changing
Change the labels at runtime without redeploying. For example
you could gradually ramp up traffic in a canary release without having
to redeploy the service

## Provider 
Queries and regenerates config at predefined intervals like 10 seconds.

### Default template file
The template file is similar to a razor template. But instead of generating
html, its generating a config file. 

It's actually implemented using the go templating language


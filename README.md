spring-cloud-examples
=====================

### Purpose

> WARNING:
> This project is a work in progress.  It is not stable yet and I reserve the right to refactor and rebase at any time. Entire projects may come and go.  Have a look at it for ideas but at this early stage things can still change drastically.

This application is meant to demonstrate the spring-cloud suite of projects, and show how they can be used together.


#### The Application
A simple shopping portal with cart and user accouts.  Users can add items to their cart and place orders. Users must be registered, activated, and logged in to place orders.  The Authentication server manages the users in a Postres relational database which acts as the source of truth for user data.  If user information is changed the downstream systems will be notified via Rabbit MQ so that they can update their local copies.  User/Order information will be stored in Mongo DB.  Cart Data and CSRF token info will be stored in Redis and shared across services and instances by using spring-session.  JWT tokens are used for authentication and are encoded and decoded using spring-cloud security.


#### Features
- [] An Angular single page application
- [] Shopping cart backed by spring data redis
- [] Spring session to manage CSRF and session data in Redis across the applications
- [] Spring data Mongo as a customer/order store
- [] Hypermedia driven Restful Web Services with spring-Data Rest
- [] Angular application uses Hypermedia links to discover resources
- [] New user registration flow with activation
- [] Oauth2 protected services (implicit grant for browser app and password grant for a Restful API endpoint)
- [] Central Authentication server (Authorization is handled by each application)
- [] API Gateway layer to handle all requests.  A Zuul proxy will forward the requests to the appropriate microservice.
- [] Eureka server to handle service registrations and allow for discoverability.  Each service will act as a client.  Zuul will make use of this to forward requests in the API gateway application.
- [] Admin (protected) dashboards accessable from the web application for viewing the Eureka and Hystrix dashboards
- [] Hystrix for the circut breaker pattern
- [] Ribbon for client side load balancing
- [] Turbine to aggreagate Hystrix streams (viewable from the Hystrix dashboard on the admin UI)
- [] Feign declarative web service client (integrated with Eureka and Ribbon for load balancing)

### Services/Applications
[acme-ui](https://github.com/wgorder/acme-ui) - The AngularJS browser application

[auth-server](https://github.com/wgorder/auth-server) - The shared authentication server

[api-gateway](https://github.com/wgorder/api-gateway) - The public facing application.  Routes requests to the appropriate places.

[service-discovery](https://github.com/wgorder/service-discovery) - The Eureka server.  The services (microservices) are clients which register with this server, so they can be discovered.

TODO: more to come

### Running the applications
Since this example is a microservice based architecture there are lots of services at play.  To facilitate ease of local testing and deployment it is suggested you use [docker](https://www.docker.com/ "docker") and [fig](https://github.com/docker/fig/ "Fig")
.

The easiest way to run the demo, is to use [fig](https://github.com/docker/fig/ "Fig")

You will need to install `docker` and `boot2docker` (if you are on a mac or windows).  After this you can set-up fig.

####Run just the supporting services
Supporting services in this context refer to the relational, mongo, and redis databases, as well as Rabbit MQ.  You can examing the `fig.yml` for a complete list of supporting services

TODO link to fig

Run `fig up` in the directory containing the `fig.yml`


####Run all the applications
TODO link to fig

Run `fig up` in the directory containing the `fig.yml`

######Customizing
You may edit the fig file to remove a service if you wish to build and run one locally (perhaps to debug it). Instructions for building and running locally are available on the page for each service.

If you are running docker containers on windows or a mac, be sure to set the  app.host env property for each application to the output of the command `boot2docker ip` rather than using the default `localhost` setting.  Some applications also have a configurable host property that can be set for the supporting services such as DB's and MQ etc.  You may want to edit this as well if you have installed any of these services locally.  You can see the fig.yml file for a full set of examples.
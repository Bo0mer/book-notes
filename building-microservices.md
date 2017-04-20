## Building Microservices: Designing Fine-Grained Systems by Sam Newman.
If you like this notes, you can [buy the book](https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems/dp/1491950358).


### Testing
* [The testing pyramid](https://martinfowler.com/bliki/TestPyramid.html)
    * You should have many more low-level unit tests than high level
      end-to-end tests
* The agile testing quadrants
* Cross-functional tests
* Performance tests
    * Set target for results
    * Run the tests regularly
    * Results should be checked after each run

### Monitoring
* Semantic monitoring
    * Correlation IDs - start early and try to standardize them across the
      organization 
* Consider the audience
    * Different types of dashboards for different audience

### Security
* Authentication and authorization
    * For end users
        * SSO solutions
        * SSO gateway
    * For services
        * Allow everything inside
        * Basic authentication
        * SAML or OpenID connect
        * HMAC over HTTP
        * API Keys
* Go with the well known - be it technology, library or encryption algorithm. Do
  not try to invent your own.
* It’s all about the keys - treat them carefully.
* Encrypt all backed up data.
* Firewalls
* Network segmentation

### Microservices at scale
* Failure is everywhere
    * Optimize for graceful recovery, not for avoiding the inevitable failure
    * Architectural safety measures
        * Timeouts
        * Bulkheads
        * Circuit breakers
        * Isolation
        * Idemptency
        * Caching
* Measure before scaling and see which part of the system needs to scale.
  This is usually not known in advance.
* Scaling databases
    * Scaling for reads
        * Caches
        * Read replicas
    * Scaling for writes
        * Sharding
        * Changing DB technology

### Bringing it all together
* Model around business concepts
* Adopt a culture of automation
* Hide internal implementation details
* Decentralize all the things
* Independent deployable
* Isolate failure
* Highly observable

## Software Deployment Strategies

### Enterprise SaaS (Node API)

Software should be split into three discrete units for deployment: 

* A triple-tier deployment allows for different scaling and high availability (HA) strategies per tier versus single monolithic deployments.
* This may be more difficult to implement and roll out from an administration perspective, but allows for more scalability for large-scale SaaS.

##### Static content / User interface

* Static content needs to be delivered over HTTPS with a caching strategy at the HTTP provider, as well as compression.
* The use of content delivery network (CDN) capabilities should be used where possible for global deployment.

##### APIs and Middleware

* APIs need the same HTTPS and compression options enabled, as well as CORS for access.
* Providing load balanced APIs allows for single FQDN for access, but highly parallel and HA systems.

##### Databases and Storage

* Database and other storage options should be either clustered or distributed for default. This allows for growth, as well as performance needs.
* Cross-WAN access to these from external systems or users should not occur, but be protected in a VLAN enclave or firewall rule set.
* Database accesses should also provide caching for certain, high-frequency access objects, like user credentials. This should occur at the SaaS for the most part, but the capability should be provided not only in software but in infrastructure choices (Memcached against MongoDB, for example).

** Retrieve from cache first, on fail, thrash I/O.
** Check cache for write, update, and save into DB.

### Single or Small Cluster Deployments (JVM API)

* The JVM API is designed for smaller, less capacity-intensive deployments.
* The API and UI are packaged as WARs, and can be deployed into the same system.
* Shared resources, such as sessions, are now isolated to the persistence layer versus software. This allows more clustering configurations with Java containers.
* Clustering should occur at the container level versus software options.
* HA, scalability, and elasticity needs should be handled at the IaaS or bare-metal infrastructure inside the JVM or container, versus the software.

### Generic Guidance

Systems need two identical clones: production and staging

* Production should be a manual 'check-the-box' from a queue of builds from continuous integration / deployment, and give a visual indication of pass/fail from automated or human testing.
* Staging should be a live version of builds with automated testing that passed via continuous integration.
* Builds should be promoted from staging to production, via a simple manual queue with push-button delivery to production.
* Production must have an identical configuration as staging (Numbers of machines, networking and software configuration, etc. Resources may vary).

Network and service availability

* Tier-level services should have HA and load balanced FQDNs (database.software.com, api.software.com, software.com, etc).
* Resource distribution where possible, no large aggregate of resources in a single zone, rack, or data center where possible.
---
title : Local Setup
---

A local setup is a (crude)term used to refer to having a set of production applications running in ones laptop.
This setup is such that a desired set of functionality can be executed locally.

This is in contrast to applications running in a production environment which is typically on cloud infrastructure provider like AWS, GCP, Azure etc.
Of course production environment can also be on an on-premise, self-managed data center or some combination of the two.
The crux is that it is not local.

### Why is a local setup required?
For development purposes. Developers need to try out stuff they are developing. 
Having developers do the same on the production environment is a clear no-no as this will affect customers.
Importantly, this saves money. Production grade hardware is not required for such stuff.

However, I would not be surprised if there are companies which wip up isolated environments per developer non-locally.
So maybe the correct term for such a setup is development setup or isolated developer setup. IDS... eeks

### The Scenario
Any non-trivial real world application would consist of multiple web services, batch jobs, 
databases, caches, messaging systems, varied distributed systems(zk, s3, etc, etc, etc, ...)

At a time, a developer would typically want to work on a particular flow. 
Hopefully the code change for this is enclosed in an application service or a job(batch, streaming).

In most cases, it would not be enough to bring up just this service or job up for the developer to check on things.
Assume we are talking of a new API developed in an existing service. 

1. Would need some authentication token to invoke this API. This authentication token is oft generated by a different service.
2. The new API works off of data generated by some other systems/services.

So in addition to the service where the code has changed, there is a clique of other services/jobs/systems that need to come up.

These would typically fall in one of the below categories

1. Services owned by different teams in the same org
   - Expectation is for a containerized service to be provided which works out of box
   - Would typically expect this to be a black box
   - Would need to pick up environment-specific configuration - vault
2. Distributed Systems which are not business applications. 
   Data and compute infra like db's, messaging systems, compute clusters, coordination-services
   - Containerized service
   - Must allow for inspection of state through standard interfaces
   - Need to be set up to be environment-aware
3. Cloud native services - S3, Confluent, Mongo Atlas 
   - Environment-specific namespace isolation
   - Allow for inspection of state through standard interfaces(aws console, confluent UI, Atlas UI etc)

Going a step further, it would be great if Observability stacks can also be implemented as a part of these local setups

## References
- https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-1-a2f5d9a77813
- https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-2-optimizing-for-fast-local-development-9f27a98b47ee
- https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-3-extending-our-envoy-mesh-with-staging-fdaafafca82f
- https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-4-gating-deploys-with-automated-acceptance-4417e0ebc274
- https://tilt.dev
- https://github.com/readme/guides/developer-onboarding
# Alerting for Kubernetes Infrastructure

* Status: proposed 
* Deciders: @ZavenArra, @TANguyen1893
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: 2022-05-29


## Context and Problem Statement

How do we know that things are going wrong with our dev/test/production kubernetes infrastructure?
Currently we have notifications channels, which are certainly helpful, but don't necessarily allow users to define errors of their own, and don't capture all
of the error conditions that might occur within a Kubernetes cluster.

## Decision Drivers <!-- optional -->

* There could be issues, like failed airflow events, that are not necessarily captured by the notification bots
* A comprehensive alerting system is an important part of DevSecOps.

## Considered Options

* Prometheus/Grafana
* Dynatrace
* Datadog
* DigitalOcean monitoring
* Heapster

## Decision Outcome

Chosen option: Prometheus/Grafana, because we already use it for metrics and it is a more robust setup than many of the other options.

### Positive Consequences <!-- optional -->

* Teams will know when their services are failing as early as possible
* Alerting can be configured in code
* Grafana has good access control set up via GitHub organizations.

### Negative Consequences <!-- optional -->

* Alerting can always be noisy. It requires good handling of the rules
* Will need to eventually divide up into appropriate channels based on namespace. 
That might be a bit more difficult to do to start (but is a problem for all alerting types).

## Pros and Cons of the Options <!-- optional -->

### Prometheus/Grafana

One of the most common open source metrics/monitoring systems. Also operates well with Loki/ Jaeger if we operationalize them more

* Good, because we already use it for metrics
* Good, because it's open source and well supported
* Good, because it has great native k8s support
* Bad, because the alerting can be a bit noisy, and it's not as powerful as some paid options

### Dynatrace

APM (application performance management) solution that's relatively standard in the industry

* Good, because has interesting visualizations
* Bad because the deployment setup is supposedly [less straightforward](https://www.dynatrace.com/support/help/setup-and-configuration/setup-on-container-platforms/kubernetes/get-started-with-kubernetes-monitoring)
* Bad, because it doesn't distinguish between linux and kubernetes hosts

### Datadog

APM data pipeline tool for monitoring various systems.

* Good, because it has good kubernetes default support
* Good, because it has rocking visualizations
* Bad, because it costs money! It's not open source.


### DigitalOcean monitoring

Monitoring built into DigitalOcean for cluster health.

* Good, because it comes OOTB with the DO clusters.
* Bad, because it is not as rich as the data provided by other systems.
* Bad, because the access control mechanism is tied to the DO accounts the clusters run in.
* Bad, because the alerting is only at the resource level
* Bad, because advanced alerting requires additional setup, but doesn't allow for service-level metrics like Prometheus.

### Heapster

Kubernetes data aggregation tool.

* Good, because it's relatively simple to set up.
* Bad, because heapster is [retired](https://github.com/kubernetes-retired/heapster)
* Bad, because it requires setting up an additional DB

## Links

* [Resources](https://logz.io/blog/kubernetes-monitoring/)

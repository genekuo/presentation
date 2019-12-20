# `Istio`

---

## `Introduction to new challenges`
* Service communication challenges: reliability, control, security, observability
* Microservices, DevOps, Cloud promise the enabling of faster and agile
* New problems: resilience, metric collection, ... in App?
* Solution: Language/framework agnotic "Application infrastructure services"

---

##  `Service mesh`
* A decentralized application-networking infrastructure that allows apps to be secure,
resilient, observable and controllable
* Istio is an implementation of a service mesh
* Istio's data plane consists of service proxies based on Envoy proxy,
And Istio's control plane implements APIs to manage and configure the proxies

---

## `Faster value stream`
* DevOps and Lean thinking aim to solve the problems around optimizing the system for agility and faster value deliveries.
* Scientific approach: treating "proposal" as hypothesis by running piloted (experimental) system to reduce uncertainty
* Microservices, cloud, automatic testing, containers, CI/CD aim to help go faster testing hypothesis

---

## `Microservices`
* A architecture that decouples large systems into consituent smaller services that work together to implement business functionality

---

## `Automatic testing`
* Unit test, regression, scenario/feature test, test in proudction

---

## `Kubernetes`
* A container deployment platform with a higher-level API to deploy and manage containers
across a cluster of machines

---
## `CI/CD`
* Continuous intergation (CI): force code changes to be integrated as quickly as possible.
* Continuous delivery (CD): orchestrate the steps to successfully take an application from code commit to production
* Control the rollout of a new release by controlling the traffic to the deployment

---

## `Challenges to the service architecture`
* Isolating boundaries around faults
* Apps/services able to respond to changes in their environment
* Observe the overall system as it constantly evloves and changes
* Control the runtime behaviors of the system
* Strong security and lowing the attack surface
* Enforce policies about the usage of components in the system

---

## `Move application networking concerns to infrastructure`
* Kubernetes as container deployment and management platform
* Envoy proxy: application-aware service proxy, L7 proxy
  - retries, timeouts, circuit breaking, client-side load balancing, service discovery, security, metric-collection

---

## `Service Mesh`
* A service mesh is a distributed application infrastrucure that handles network traffic on behalf of application
* The capability of the data plane and control plane
  - Service resilience: retries, timouts, circuit breakers, service discovery, adaptive zone-aware load balancing, health checking
  - Observability signals: request spikes, latency, throughput, failures
  - Traffic control
  - Security: transport-level encryption with mutual TLS, keys/certificate issuance/installation/rotation
  - Policy enforcement: quotas, rate-limiting, organizational policies

---

## `Traffic flow in a service mesh`
* Traffic comes into the cluster
* Traffic goes to the Istio service proxy (Envoy) with mTLS
* Istio routes the traffic to other service according to policy
* Istio Pilot is used to configure proxies which handle routing, security, and resilience
* Request metric are sent to Istio Mixer to store them
* Distributed tracing span sent to an tracing store
* Isito Auth manages certificates and enables mTLS

---

## `Install Istio on k8s: prerequisites`
* Minikube: https://github.com/kubernetes/minikube
* `minikube start --memory=4096 --disk-size=30g --kubernetes-version=v1.13.2`
* kubectl get nodes
* Istio distribution: https://github.com/istio/istio/releases
* `tar -xzf istio-1.2.2-osx.tar`
* `cd istio-1.2.2`
* `ls -l`
* `./bin/istioctl verify-install`

---

## `Install Istio components into k8s`
* Install all components: Pilot, citadel, telemetry, policy, tracing, gateway
* `kubectl create -f install/kubernetes/istio-demo.yaml`
* `kubectl get pod -n istio-system`
*  proxy-config inspects the configuraions of the proxies at runtime
* `kubectl run -i --rm --restart=Never dummy --image=tutum/curl:alpine \
  -n istio-system --command \
  -- curl -v 'http://istio-pilot.istio-system:8080/v1/registration'`

---

## `Control plane`
* APIs for specifying routing/resilience behavior
* APIs for the data plane to consume configuration
* Service discovery for the data plane
* Quota and usage restrictions
* APIs for specifying usage policies
* Certificate issuance and rotation
* Assigning workload identity
* Telemetry collection
* Specifying network boundaries and access

---

## `Pilot`
* Allows operators to configure service proxies (Envoy) in the data plane, exposed as Envoy configuration
* Exposes APIs for the operators and the data plane
* Leverages k8s services (APIs) such as service-registry and configuration through the use of platform adapters
* Configuring: traffic ingress, routing, service-to-service resiliency
* `kubectl create -f catalog-ervice.yaml`

---

## `Istio's configuration resources`
* Implemented using k8s Custom Resource Definitions (CRD)
* The data plane API exposed by Istio Pilot: (xDS APIs)
  - Listener discovery service
  - Endpoint discovery service
  - Route discovery service

---

## `Ingress and Egress gateway`
* For interacting with applications outside of the cluster
* Are Envoy proxies in the data plane that understand Istio configurations
* Independent of application workloads

---

## `Istio Citadel`
* Handles attestation and issuance/mounting/rotation of the X.509 certificates
* Follows SPIFFE (Secure Production Identity Framework For Everyone)
* Provides strong mTLS

---

## `Istio Mixer`
* Telemetry collection, traffic/authorization policy definition and enforcement, quota management/rate limiting
* Attributes and attribute processing for requests
* For each request that comes from the data plane, Istio records these attributes and sends to the Mixer
* Mixer exposed APIs:
  - Check: called inline with a request for validation
  - Report: APIs with Istio Telemetry, used to send attributes about request asynchronously
* Mixer's quota and cache implementation for its availibility and performance

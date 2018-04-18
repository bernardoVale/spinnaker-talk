class: center, bottom


# Achieving Continous Deployment with Spinnaker

???
Present yourself
---
class: center, bottom

# Delivery Principals

???
Agenda

Talk about a few things that are really important in CD
Explain a little bit about Spinnaker
Demo

IMPORTANT THINGS ABOUT CD / NOT TOOLLING

THE COMPLEXITY OF ROLLING OUT CONTINOUS TO INCREASE

NETFLIX ONE DEPLOY TO PROD A CADA 10 SECONDS

NETFLIX EXPERIENCE + SET OF IDEAS + COMMON PATTERNS = SPINNAKER

---
class: center, bottom

# Immutable Infrastructure

???

WHATS THE PROBLEM OF MUTABLE INFRASTRUCTURE (CONFIG DRIFT)

Once you deploy you dont change it

If you need to change anything, you build/deploy/teardown the old machine

Why that is important?

We want a deterministic process (building, testing, deployment)

How?

You bake your infrastructure with your app

Ask audiance why they thing Immutable Infrastructure is important
How many of you guys deployed a new version of your app in dev, it work fine then it broke in prod?

---
# How its implemented - VMland

- Start with a base virtual machine / container

--

- Use packer or similar tool to spin the virtual machine

--

- Use a provisioner Ansible/Salt/Chef/Puppet to add your app

--

- Push the resulting image to a safe image repository


---

# How its implemented - Containerland

- Start with a base container image

--

- Use docker build to build the base container with you app

--

- Push the resulting image to a safe image repository

---
class: center, middle

# If the process is almost the same, why containers?

???

OTHER BENEFITS (FASTER STARTUP TIME)

FASTER BUILD PROCESS

Ability to be elastic. If to scale you take 20min to spin up a VM and later run Chef/puppet you might not be able to attend te demand.

---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/packer-build.jpg)

---
class: center, middle
# Deployment Strategies


???



Canary

???

Incrementally add more traffic to the new version with validation gates

Canary - We look at a set of measures to (canary score) define if we can increase the traffic

---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/bluegreen.png)

# Deployment Strategies

#### Blue/Green

???

Advantages = Easy to rollback

Desadvantages = Cost (you need 2x the amount of resources)

---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/rolling-bluegreen.png)

# Deployment Strategies

#### Rolling Blue/Green

???

Add a validation gate and incrementally increase traffic from blue to green

---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/canary.png)

# Deployment Strategies

#### Canary

???

Just a different way to do the validation

Rather than using functional tests or smoke tests

We look at a set of measures, such as latency, error code rates

Take all this and set weight and define an acceptable score

Then look at a canary score

---
class: center, middle
# Automation

???
Promotes consistency && repeatability

both for failure and success. We also need consitent failure!!!

---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/spinnaker-pipeline.png)

# Automation with Spinnaker


???

Top-level pipelines go out and define and execute for you your process.

Deploy: Fine-grained steps
---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/spinnaker-stages.png)

# Automation with Spinnaker


???

Stage deploy - Steps: deploy in default, shrink cluster, disable cluster - determine health provider, disable, monitor, force cache

Stage -> Steps -> Tasks -> Operations (Cloud provider stuff)


Really is not simple. Sometimes you need manual approvals validation
---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/spinnaker-logo.png)

---
# Spinnaker

Netflix way of deploying Netflix :)

The result of the partnership of Netflix, Google, Microsoft and Pivotal

--

Spinnaker facilitates the creation of pipelines that represent a delivery process that can begin with the creation of some deployable asset (such as an machine image, Jar file, or Docker image) and end with a deployment.

--

Multi cloud deployment tool:

- App Engine
- AWS
- Azure
- DC/OS
- GCE
- Kubernetes
- Openstack
- Oracle cloud
- Cloud Foundry

???
Open Source = November 2015

---
class: top, left, fit-image
layout: false
background-image: url(http://localhost:8000/images/spinnaker-components.png)

# Spinnaker Architecture

---
# Spinnaker Concepts

##### Server Group

A group of servers/containers running a deployable artifact

--

##### Cluster

Logical grouping of server groups. Like, staging,prod, uat

--

##### Load Balancer

A Load balancer :)

--

##### Security Group

Ingress rules

---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/spinnaker-concepts.png)

# Spinnaker Concepts
---

# Spinnaker Concepts

##### Account

A cloud account or orchestrator cluster

--

##### Application

A logical group of clusters and pipelines

--

##### Project

A group of one or more applications

---
class: top, right, fit-image
layout: false
background-image: url(http://localhost:8000/images/talkischeap.jpg)


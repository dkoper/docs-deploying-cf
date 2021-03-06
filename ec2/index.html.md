---
title: Deploying Cloud Foundry on AWS
---

There are several paths you can choose to deploy Cloud Foundry with BOSH on AWS.

## <a id='bootstrap-vpc'></a>Bootstrap on AWS VPC ##

The BOSH and Cloud Foundry Runtime teams maintain a bootstrapping tool to deploy
Cloud Foundry within AWS VPC.

**Note**: This tutorial deploys Cloud Foundry to the VPC flavor of AWS, rather than
the explicit EC2 flavor of AWS.
This is the production choice for running Cloud Foundry currently.
There are two reasons:

* Additional security offered by network isolation
* Static IPs are used for service discovery

Follow the [tutorial](./bootstrap-aws-vpc.html).

This is the solution Pivotal uses to deploy and operate the [hosted Cloud
Foundry solution](http://run.pivotal.io).

## <a id='low-level-ec2'></a>Step-by-step on AWS EC2 ##

To learn many of the steps of deploying Cloud Foundry on BOSH by
following this
low-level, step-by-step tutorial for AWS EC2.

Follow the [tutorial](./aws_steps.html).

**Note**: This tutorial deploys Cloud Foundry to the EC2 flavor of
AWS, rather than the explicit VPC flavor of AWS.
This is not a production choice for running Cloud Foundry currently
due to:

* Use of BOSH's internal PowerDNS for service discover, which has a
single point of failure in its backend PostgreSQL server.

AWS EC2 VMs cannot have a pre-allocated IP address (called a static IP).
Unless a BOSH deployment manages its own service discovery solution, for example
with ETCD, a BOSH deployment must explicitly document in advance where each
service (a BOSH job) is running.
One option is to use AWS Elastic IPs.
Another is to use the BOSH internal DNS feature.
The latter is the method documented in this tutorial.

The PowerDNS implemented in BOSH is backed by PostgreSQL.
If the PostgreSQL server stops operating, then PowerDNS stops operating, and
jobs within each BOSH deployment become unable to discover each other.
This may not happen very often and thus may be a satisfactory risk for you.

## <a id='low-level-ec2'></a>Bootstrap on AWS EC2 ##

There is a community tool,  [bosh-bootstrap](https://github.com/cloudfoundry-community/bosh-bootstrap "cloudfoundry-community/bosh-bootstrap · GitHub"), that automates most of the
steps in the "Step-by-step on AWS EC2" tutorial above.
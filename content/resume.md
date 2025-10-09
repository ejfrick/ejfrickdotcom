---
title: "Resume"
date: 2021-02-13T00:39:47-08:00
draft: false
---
## Summary
I'm a highly adaptable and versatile engineer who has constantly grown in scope, impact, and performance. I have wide variety of experiences across technical environments and a strong systems and software engineering background. I have designed CI/CD pipelines, wrangled Kubernetes clusters, ran multiple 100+ node distributed computing clusters, and turned fragile, opaque, automation into scalable, resilient, transparent, services throughout my career. I'm a whole-systems thinker and believe that reliability is everyone's responsibility.
## Experience

### Senior Software Engineer - Reliability @ [ConductorOne](https://www.conductorone.com), November 2023 - present

I am the first reliability hire and am responsible for pushing the company along the reliability maturity curve.

I've completely automated application deployments using [FluxCD](https://fluxcd.io) (and have been dubbed "deploy BDFL" by my CTO for it), contributed to and written Terraform and Pulumi providers ([including adding users to the Temporal Cloud Terraform provider](https://github.com/temporalio/terraform-provider-temporalcloud/releases/tag/v0.0.6)), [released an open-source connector for our app](https://github.com/ConductorOne/baton-temporalcloud), created an observability-as-code framework, wrote the first SLOs in company history, and helped migrate to Temporal Cloud (again), amongst other things.

### Site Reliability Engineer @ [Box](https://www.box.com), February 2022 to September 2023

At Box, I was a high-performing engineer lauded for versatility, friendliness, and productivity. I owned multiple 100+ node Apache Kafka & ElasticSearch clusters and other data transformation infrastructure, supported the company-wide observability pipeline, and broadly supported all services as part of company-wide SRE team, at various points.

Some key accomplishments:

- saving $2 million in operational costs over 4 years as engineering lead and project manager for on-time migration to Temporal Cloud from self-managed Temporal.
- saving ~1.25 FTE equivalent of effort annually by reducing time to resolution and overall tenant request volume for two different teams with the implementation of a formalized engagement contract, runbook automation in Bash and Python, and a new on-call process between service owners and tenants,
- saving ~140 hours of effort annually across the company by reducing mean time to diagnosis by architecting and building custom Box-integration diagnostic automation platform using Go, MySQL on CloudSQL, GCP PubSub, Redis, Prometheus, and TypeScript/SvelteKit on GKE,
- saving a reduction of ~150 hours of effort annually as measured by incident response time as co-lead for on-time migration ~30% of tenants off on-premise data access platform onto managed CloudSQL,
- building out smoke testing & database migration automation using Python running on GKE, wrote CloudSQL Auth Proxy Kubernetes sidecar, and enforced compliance with Box’s security policies for said CloudSQL migration,
- achieving a ~50% reduction in topic creation time and ~15% reduction in overall tenant requests for team with self-service Apache Kafka topic creation automation using Jenkins, Python, and Terragrunt,
- reducing downtime by 75% during migration of Sensu Go monitoring platform from on-premise to GCP by building out monitoring & alerting strategy and Etcd cluster,
- consulting on Python best practices for and collaborating on the implementation on a custom analytics API for the Core Data platform.
- I organized & facilitated team building summit for entire department and team building social hour for two different teams.
- I mentored the team's junior engineer.
- I was recognized at the org level by management as the most productive engineer for the quarter.

### DevOps Engineer @ [Jama Software](https://www.jamasoftware.com), December 2020 to February 2022

I was a key player of developer productivity-oriented Agile team responsible for CI/CD, code quality, artifact storage, development, staging, and DR environments, customer datacenter deployment orchestration, and Kubernetes migration, amongst other things. I frequently partnered and collaborated with the SRE team and was promoted in less than 6 months.

Some highlights from my time at Jama include:

- saving ~$1 million in cloud computing costs annually with autoscaling service built with Python, JavaScript, Node.js, React.js, MongoDB, Redis, & EKS,
- saving ~5 days of effort annually by automating manual release process using Python, Bash, and TeamCity,
- gaining the company ~910 hours of effort annually by decreasing artifact build time with redesigned image inheritance strategy,
- creating the design for a combined AWS CodeBuild/TeamCity CI/CD pipeline, and trialed GitHub Actions, CircleCI, DroneCI, amongst other solutions, as engineering lead for design of a new CI/CD pipeline,
- leading the POC for tooling for the company's Kubernetes migration, trialing CD, local development environment environment, and deployment manifest solutions such as ArgoCD, FluxCD, k3d, minikube, Helm, and cdk8s,
- mentoring the team's junior engineer, who is still a close friend,
- organizing and facilitating a team-building bicycling trip,
- and co-founding the company's LGBTQ ERG, and brought in a professional linguist as a guest speaker.

### DevOps Engineer @ ChamberDS, February 2020 to December 2020

I was the first infrastructure engineer for mobile-first app development consultancy and incubator, and acted as a systems architect, cloud engineer, security engineer, and FinOps engineer. Many hats, many hats!

Some greatest hits from my time there:

- Accomplished ~310 hour reduction in annual total mean time to resolution by documenting incident response processes and writing automation in Python, Node.js, and Bash
- Achieved ~15% average reduction in cloud computing costs per client by proper capacity planning, implementation of autoscaling, and re-engineering of workloads
- Gained ~75 hours of uptime annually on average per client by implementing architectural and monitoring best practices across the company
- Realized a 50% reduction in infrastructure provisioning time with implementation of Terraform and Ansible, greatly increasing developer clock speed
- and performed monthly security & dependency audits, and implemented & tested security fixes for services.

### Support Engineer @ [Chrome River Technologies](https://www.chromeriver.com), Janurary 2019 - June 2019

I was a highly-technical support engineer for a large enterprise financial software company, providing subject-matter expertise on the mobile application and email flows for two different products. I saved the company ~1800 work hours a year by automating email batch troubleshooting with SQL and was month-to-month leader in customer satisfaction surveys.

### Freelance Systems Administrator, May 2018 - Decemeber 2019 & June 2019 - February 2020
I ran websites & storage networks, automated complex tasks with Python & AppleScript, configured and installed networks, and set up device management, amongst other things, for a variety of small-medium business clients.

### Lead Technical Support Analyst @ Occidental College, August 2016 - May 2018

I was a technical support analyst, and part-time systems administrator for day-to-day operations of a higher education environment. I became team lead within the first month. I achieved a 25% average reduction in time to resolution for various macOS/Linux related manual tasks by automating them with Python & Bash, provided training on macOS/Linux troubleshooting, and redesigned & reimplemented the campus digital signage system.
## Skills

- __Programming/Scripting Languages:__ Go (Echo, Watermill, Gin, gORM, Fiber, etc.), Python (FastAPI, Flask, Celery, etc.), JavaScript/TypeScript (Node.js, Deno, SvelteKit, React.js, etc.), Bash, HTML, CSS, SQL, some Rust (PyO3), some Java, some Groovy

- __Databases:__ MySQL, PostgreSQL, MongoDB, ElasticSearch, Redis/Valkey, BigTable, BigQuery, etcd, some Neo4j

- __Messaging & Streaming:__ Apache Kafka, Apache Storm, GCP PubSub, Redis

- __Linux:__ Debian, Ubuntu, CentOS, RHEL, Alpine, etc.

- __Infrastructure as Code:__ Terraform, Terragrunt, CDKTF, Pulumi, provider development for Terraform & Pulumi, Ansible, AWS CloudFormation

- __Configuration Management:__ Packer, Puppet, Ansible

- __Container Orchestration:__ Docker, ECS, Kubernetes, GKE, EKS, k3s, Istio, Traefik, cdk8s, Helm, Kustomize

- __CI/CD:__ Drone CI, AWS CodeBuild, TeamCity, Jenkins, GitHub Actions, FluxCD, etc.

- __Version Control & Artifact Management:__ Git, GitHub, AWS ECR, GCP Artifact Registry, JFrog Artifactory

- __REST APIs:__ Design & implementation, OpenAPI/Swagger, Swagger UI, Swagger/OpenAPI Codegen

- __gRPC__

- __Observability, Monitoring, & Logging:__ Splunk, Wavefront, Sensu Go, Prometheus, Datadog, New Relic, AWS CloudWatch, etc.

- __Cloud Computing:__ AWS (ECS, EC2, ELB, S3, RDS, AuroraDB, EKS, CloudWatch, CloudFront, CodeBuild, etc.), GCP (GKE, GCE, GCS, Cloud Load Balancing, CloudSQL, PubSub, BigTable, BigQuery, Dataproc, Dataflow, etc.)

- __Workflow Orchestration:__ Temporal, Apache Airflow

- __Web Servers:__ Nginx, Apache, HAProxy, Apache Tomcat

- __Other:__ SDLC, Process re-engineering, SRE/DevOps strategy, Agile (kanban, scrum, Jira)

## Education

I have a Bachelor's degree from Occidental College.

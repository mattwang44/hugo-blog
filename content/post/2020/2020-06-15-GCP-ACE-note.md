---
title: "Course Note: Cloud Engineering with GCP on Coursera"
subtitle: "Course note for the preparation of GCP ACE Certificate"
description: " "
excerpt: " "
date: 2020-06-15
author: "Matt Wang"
image: "https://images.unsplash.com/photo-1523815085589-2ed388376202?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2853&q=80"
published: true
math: true
tags:
  - GCP
  - Course
URL: "/2020/06/15/gcp-ace-note/"
categories: ["Tech"]
---

This is the note of the course [Cloud Engineering with GCP](https://www.coursera.org/professional-certificates/cloud-engineering-gcp).
Go to [this article](/2020/07/01/gcp-ace/) to see how I prepared for GCP ACE exam.

## Mind Map

{{% mind %}}

- GCP
  - Network Resources
    - VPC
      - Network
      - Subnet
      - IP
    - Load Balancer
      - HTTP(S)
      - SSL Proxy
      - TCP Proxy
      - Regional
      - Multi-Regional
    - VPN & Peering & Interconnect
  - Compute Services
    - Compute Engine (GCE)
      - Disk type
        - persistent
          - HDD/SSD
        - Local SSD
      - Instance template
      - Instance group
    - App Engine (GAE)
      - Standard
      - Flexible
    - Kubernetes Engine (GKE)
    - Cloud Function
  - Storage
    - Data warehouse
      - BigQuery
    - Database
      - NoSQL
        - BigTable
        - Cloud Firestore
          - Datastore mode
          - Native mode
      - Relational
        - Cloud SQL
        - Cloud Spanner
      - Redis/Memcache
        - Cloud Memorystore
  - SDK
    - [`gcloud`](https://cloud.google.com/sdk/gcloud/reference)
    - [`gsutil`](https://cloud.google.com/storage/docs/gsutil/commands/acl)
    - [`bq`](https://cloud.google.com/bigquery/docs/reference/bq-cli-reference#command-specific_flags)
  - Stackdriver
    - Monitoring
    - Logging
    - Debug
    - Error reporting
    - Trace
  - Big data services
    - BigQuery
    - Cloud Dataproc
    - Cloud Dataflow
    - Cloud Pub/Sub
    - Cloud Datalab

{{% /mind %}}

## SDK

- [Cloud SDK installation and quick start](https://cloud.google.com/sdk/#Quick_Start)
- [gcloud tool guide](https://cloud.google.com/sdk/gcloud/)
- [`gcloud` command](https://cloud.google.com/sdk/gcloud/reference)
- [`gsutil` command](https://cloud.google.com/storage/docs/gsutil/commands/acl)
- [`bq` command](https://cloud.google.com/bigquery/docs/reference/bq-cli-reference#command-specific_flags)

## Resource Management

- Resource Hierarchy
  - Global
    - Images
    - Snapshots
    - Networks
  - Regional
    - External IP addr.
  - Zonal
    - Instances
    - Disks
- label vs tag
  - |           label            |         tag         |
    | :------------------------: | :-----------------: |
    |    user-defined kv pair    | user-defined string |
    | propagated through billing |   firewall rules    |

### Billing

- [Docs - Billing](https://cloud.google.com/billing/docs/)
- [Pricing Calculator](https://cloud.google.com/products/calculator/)
- Four tools
  - Budget & Alerts
  - Billing export -> to [BQ](https://cloud.google.com/billing/docs/how-to/export-data-bigquery) or [Cloud Storage](https://cloud.google.com/billing/docs/how-to/export-data-file)
  - Reports
  - Quotas
    - types
      - Rate Quotas: e.g. GKE API: 1000 requests per 100 sec
      - Allocation Quotas: e.g. 5 networks per project
    - can be canged on quota page

## GCP Hiearchy

- Organization (-Folder (- Folder)) - Project - Resource
- Policies are inherited downwards

{{< figure src="https://cloud.google.com/resource-manager/img/cloud-folders-hierarchy.png" width="500px" >}}

|   Project ID    |     Project name      | Project number  |
| :-------------: | :-------------------: | :-------------: |
| globally unique | not need to be unique | globally unique |
|  user defined   |     user defined      | assigned by GCP |
|    immutable    |        mutable        |    immutable    |

```shell
# set default project
gcloud config set project <project id>
```

## IAM

- `Who` `can do what` `on which resource`
- 3 types of IAM Roles:
  - **Primitive**
    - Owner: invite/remove members, delete project
    - Editor: deploy/modified/configure
    - Viewer: read-only
    - Billing Administrator: control billing but not affect projects
  - **Predefined**
  - **Custom**
- Services Account
  - Authentication for services
  - Use email addr. & cryptographic keys
- commands
  ```shell
  # add IAM policy binding for a project
  gcloud projects add-iam-policy-binding --member=(user|group|serviceAccount:email) --role=ROLE
  ```
- IAM policy ([doc](https://cloud.google.com/iam/docs/reference/rest/v1/Policy))
  ```shell
  # sample IAM policy
  {
    "bindings": [
      {
        "role": "roles/resourcemanager.organizationAdmin",
        "members": [
          "user:mike@example.com",
          "group:admins@example.com",
          "domain:google.com",
          "serviceAccount:my-project-id@appspot.gserviceaccount.com"
        ]
      },
      {
        "role": "roles/resourcemanager.organizationViewer",
        "members": [
          "user:eve@example.com"
        ],
        "condition": {
          "title": "expirable access",
          "description": "Does not grant access after Sep 2020",
          "expression": "request.time < timestamp('2020-10-01T00:00:00.000Z')",
        }
      }
    ],
    "etag": "BwWWja0YfJA=",
    "version": 3
  }
  ```
- audit log: logs of Admin Activity, Data Access, and System Event
  ```shell
  # read audit log, `project` can be changed to `folder` or `organization`
  gcloud logging read "logName : projects/project-id/logs/cloudaudit.googleapis.com" --project=project-id
  ```
- Resource manager roles
  - organization
    - Admin: full control over all resources
    - Viewer: view access to all resources
  - folder
    - Admin: full control over folders
    - Creator: browse hierarchy and create folder
    - Viewer: view folders and projects below a resources
  - project
    - Creator: create projects
    - Deleter: delete projects

* [Docs](https://cloud.google.com/iam/docs/)
* [Docs - Understanding IAM roles](https://cloud.google.com/iam/docs/understanding-roles)
* [Best Practices](https://www.coursera.org/learn/gcp-infrastructure-core-services/lecture/xghL2/cloud-iam-best-practices)
* [Google Cloud Identity](https://cloud.google.com/identity/)
* [Understanding Service Accounts](https://cloud.google.com/iam/docs/understanding-service-accounts)
* [Cloud Audit Logging overview](https://cloud.google.com/logging/docs/audit/)
* [Security and Identity Fundamentals Quest](https://www.qwiklabs.com/quests/40)

## Compute Resources

- [Brief Comparison: App Engine, Compute Engine, GKE](https://www.coursera.org/learn/preparing-cloud-associate-cloud-engineer-exam/lecture/1VqS2/planning-and-configuring-compute-resources)
- [Related Blogpost from GCP](https://cloud.google.com/blog/products/gcp/choosing-the-right-compute-option-in-gcp-a-decision-tree)
- [Product page - hosting options](https://cloud.google.com/hosting-options)

|          |        GCE         |         GKE         |             GAE-standard             |                        GAE-flexible                        |      Cloud Functions      |
| :------: | :----------------: | :-----------------: | :----------------------------------: | :--------------------------------------------------------: | :-----------------------: |
|   Lang   |        Any         |         Any         |    Python, Node.js, Go, Java, PHP    | Python, Node.js, Go, Java, PHP, Ruby, .NET, Custom Runtime |    Python, Node.js, Go    |
| Scaling  | server autoscaling |       cluster       |     autoscaling managed servers      |                autoscaling managed servers                 |        serverless         |
| Use case |      general       | container workloads | scalable web app, mobile backend app |            scalable web app, mobile backend app            | light-weightevent actions |

### App Engine (GAE)

- [Docs - Cloud Source](https://cloud.google.com/source-repositories/docs/)
- [Docs - How instances are managed](https://cloud.google.com/appengine/docs/standard/python/how-instances-are-managed)

|                  |                            Standard                            |                      Flexible                      |
| :--------------: | :------------------------------------------------------------: | :------------------------------------------------: |
|     Language     |                     Java, Python, Go, PHP                      |                        any                         |
| Instance startup |                           milli-secs                           |                      minutes                       |
|       SSH        |                               N                                |                         Y                          |
|  3rd-party lib   |                               N                                |                         Y                          |
|  Pricing model   | after free daily use, pay per instance class, w/ auto-shutdown | Pay for resource alloc. per hour, no auto-shutdown |
|      other       |                 all requests timeout is 60 sec                 |

- ```shell
  # migrate the service to new version
  gcloud app versions migrate v2 --service="s1"

  # split traffic evenly between 'v1' and 'v2' of service 's1', run:
  gcloud app services set-traffic s1 --splits v2=.5,v1=.5

  # deploy but send no traffic
  gcloud app deploy --no-promote
  ```

### Compute Engine (GCE)

- [Docs](https://cloud.google.com/compute/docs/)
- Features
  - choose vCPUs & memory
  - Networking
  - Access
    - Linux: SSH (tcp:22)
    - Windows: RDP (tcp:3389)
  - metadata server
    - e.g. `startup-script-url` & `shutdown-script-url`
    - fetch instance metadata from application ([doc](https://cloud.google.com/compute/docs/storing-retrieving-metadata#querying))
      ```shell
      curl metadata.google.internal/computeMetadata/v1/
      ```
  - OS images ([doc](https://cloud.google.com/compute/docs/images))
    - Choose between
      - public base images
      - custom images
- machine types
  - predefined
    - |                |     standard      |   high-memory    |     high-CPU     | memory-optimized  | compute-optimized |       shared-core       |
      | :------------: | :---------------: | :--------------: | :--------------: | :---------------: | :---------------: | :---------------------: |
      |      name      | `n1-standard-<N>` | `n1-highmem-<N>` | `n1-highcpu-<N>` | `n1-ultramem-<N>` | `c2-standard-<N>` | `f1-micro`, `g1-small`  |
      | mem/vCPU ratio |      3.75GB       |      6.5GB       |      0.9GB       |       ~24GB       |        4GB        |                         |
      |                |                   |                  |                  |                   |   new platform    | for small intensive app |
    - (`N` indicates # of vCPUs)
  - custom
    - number of vCPU = $2^n$ ($n = 0, 1, 2,...$)
    - memory must in (0.9, 6.5) GB per vCPU
    - total memory must be multiple of 256MB
    - extend memory -> provide higher memory/vCPU ratio with additional cost
- disk:
  - **persistent**: up to 128 disks x 0.5TB (64TB in total)
    - **HDD** (standard) & **SSD**
    - resize while running
    - encryption keys
    - snapshot for backup ([doc](https://cloud.google.com/compute/docs/disks/create-snapshots))
  - **local SSD**: up to 8disk x 375GB (3TB in total)
  - |                    |           HDD            |     SSD     |       local SSD        |            RAM Disk             |
    | :----------------: | :----------------------: | :---------: | :--------------------: | :-----------------------------: |
    |  data redundency   |            Y             |      Y      |           N            |                N                |
    | encryption at rest |            Y             |      Y      |           Y            |               N/A               |
    |    snapshotting    |            Y             |      Y      |           N            |                N                |
    |      bootable      |            Y             |      Y      |           N            |                N                |
    |      use case      | general, bulk file store | random IOPS | high IOPS, low latency | low latency & risk of data loss |
- Billing
  - per-second billing (with minimum of 1-min cost)
  - each vCPU & GB of memory is billed separately
  - discount
    - sustained use (monthly-based)
    - committed use (1-3 years)
    - preemptible
      - cost reduced up to 80%
      - live up to 24 hour
      - no auto-restart

```shell
# create an instance
gcloud compute instances create --machine-type=<machine-type> <instance_name>

# move an instance to another zone within same region
gcloud compute instances move <instance_name>
```

- move instance to another region
  - make snapshot of persistent disk
  - create new persistent disk in target zones by restoring from snapshot
  - create VMs in target zones
  - assign static IP, update the VM's references, and then delete the original VM

#### Instance Group

- [Docs - Instance Group](https://cloud.google.com/compute/docs/instance-groups/)
- [Docs - Instance Template](https://cloud.google.com/compute/docs/instance-templates/)
- [Docs - Create VM from Template](https://cloud.google.com/compute/docs/instances/create-vm-from-instance-template)
- [Docs - Create Groups of Managed Instances](https://cloud.google.com/compute/docs/instance-groups/creating-groups-of-managed-instances)

#### Autoscaling

- [Docs - Autoscaling](https://cloud.google.com/compute/docs/autoscaler/)

### Kubernetes Engine (GKE)

- [Docs](https://cloud.google.com/kubernetes-engine/docs)

```shell
# create cluster
gcloud container clusters create <name>

# Add node to node pool (scale)
gcloud container clusters resize --num-nodes=3

# scale a deployment
kubectl scale <service> --replica=3

# auto scale
kubectl autoscale <service> --min=10 --max=15 --cpu=80
```

## Storage

- [Brief Intro](https://www.coursera.org/learn/preparing-cloud-associate-cloud-engineer-exam/lecture/w16oO/planning-and-configuring-data-storage)
- [Comparison](https://www.coursera.org/learn/preparing-cloud-associate-cloud-engineer-exam/lecture/oKobq/deploying-and-implementing-data-solutions)
- [Product page - Cloud Storage](https://cloud.google.com/products/storage)

### Cloud Storage (GCS)

- Fully-managed scalable -> no manually provisioning is needed
- Not a filesystem
- Always encrypted with HTTPS
- `gsutil` is frequently used
- Can enable versioning
  - ```shell
    gsutil versioning set (on|off) gs://<bucket_name>
    ```
- Data transfer
  - resumable by default
  - `-m` for transfering in parallel:
    ```shell
    gsutil -m cp -p file gs://bucket/object
    ```
  - Use BOTO file for additional configuration - [doc](https://cloud.google.com/storage/docs/gsutil/commands/config)
  - Streaming upload/download with `-I` option ([doc](https://cloud.google.com/storage/docs/gsutil/commands/cp))
    ```shell
    some_program | gsutil -m cp -I gs://my-bucket
    ```
- ACL (Access Control Lists)

  - e.g.
    - e-mail
    - `allUsers`
    - `allAuthenticatedUsers`
  - ```shell
    # set ACL
    gsutil acl set private gs://bucket

    # Change ACL for all users to have read access (O for owner, W for write, R for read)
    gsutil acl ch -u AllUsers:R gs://example-bucket/example-object
    ```

- signed URL
  ```shell
  # duration = 10 minutes
  gsutil signurl -d 10m /path/to/key gs://bucket/object
  ```

#### Storage Class

- [Docs - Storage Classes](https://cloud.google.com/storage/docs/storage-classes)
- Regional or Multi-regional can be changed to Nearline or Coldline
- Regional cannot be changed to Multi-regional and vice versa

|                  |       Multi-Regional       |                 Regional                  |            Nearline             |            Coldline            |
| :--------------: | :------------------------: | :---------------------------------------: | :-----------------------------: | :----------------------------: |
|   Data that is   |     Frequently access      |      Freqenctly access within region      | Access less than once per month | Access less than once per year |
|       SLA        |           99.95%           |                  99.90%                   |               99%               |              99%               |
|  Storage Price   |   $\star\star\star\star$   |             $\star\star\star$             |          $\star\star$           |            $\star$             |
| Retrieval Price  |          $\star$           |                  $\star$                  |        $\star\star\star$        |       $\star\star\star$        |
|    Use cases     | Content storage & delivery | In-region analytics, GCE/GKE-related data |    Long-tail content, backup    |  Archiving, disaster recovery  |
| minimum duration |             -              |                     -                     |             30 days             |            90 days             |

#### Lifecycle

- [Docs - Lifecycle](https://cloud.google.com/storage/docs/lifecycle)
- e.g.
  - delete objects older than XXX days
  - delete objects created before \<Date\>
  - keep N latest version only
- ```shell
  gsutil lifecycle set <config-json-file> gs://bucket
  ```
- config file example
  - ```shell
    # Move object from multi regional to COLDLINE in 30 days and delete after 300 days after creation
    {
      "lifecycle": {
        "rule": [
          {
            "action": {
              "type": "SetStorageClass",
              "storageClass": "COLDLINE"
            },
            {
              "condition": {
                "age": 30,
                "matchesStorageClass": ["MULTI_REGIONAL", "STANDARD", "DURABLE_REDUCED_AVAILABILITY"]
              }
            }
          },
          {
            "action": {
              "type": "Delete"
            },
            "condition": {
              "age": 270,
              "storageClass": "COLDLINE"
            }
          }
        ]
      }
    }
    ```

## Database Services

- Data storage comparison
  - |               |         Cloud Datastore          |                       Bigtable                       |    Cloud Storage    |        Cloud SQL        |             Cloud Spanner              |                BigQuery                 |
    | :-----------: | :------------------------------: | :--------------------------------------------------: | :-----------------: | :---------------------: | :------------------------------------: | :-------------------------------------: |
    |     Type      |          NoSQL Document          |                  NoSQL wide-column                   |      Blobstore      | Relational SQL for OLTP |        Relational SQL for OLTP         |         Relational SQL for OLAP         |
    |  Transaction  |                Y                 |                      Single-row                      |          N          |            Y            |                   Y                    |                    N                    |
    | Complex query |                N                 |                          N                           |          N          |            Y            |                   Y                    |                    Y                    |
    |   Capacity    |               TB+                |                         Pb+                          |         Pb+         |           TB            |                   Pb                   |                   Pb+                   |
    |   Unit Size   |            1Mb/entity            |                ~10Mb/cell, ~100Mb/row                |     5TB/object      |  determined by engine   |              10240MiB/row              |                10Mb/row                 |
    |   Best for    |     Semi-structured app data     |          analytical data, heavy read/write           |                     |         Web app         |            Large-scale app             | interactive querying, offline analytics |
    |   Use Case    |               App                |                     IoT, AddTech                     | Images, media files |                         | High I/O, global consistency is needed |             Data warehouse              |
    |               | SQL-like query, free daily quota | Same API with HBase, user can increase # of instance |
- Decision chart
  - data structured?
    - No -> **Cloud Storage**
    - Yes -> Analytic workload?
      - Yes -> need updates or low latency
        - Yes -> **BigTable**
        - No -> **BigQuery**
      - No -> Relational data?
        - No -> **Cloud Firestore**
        - Yes -> Need horizontal scale
          - Yes -> **Cloud Spanner**
          - No -> **Cloud SQL**

### Bigtable

- Feats
  - scale to PB
  - Low latency (sub 10ms)
  - Auto update index for frequently accessed data -> balance workload between nodes

### Cloud Spanner

- Feats
  - scale to PB
  - Strong consistency
  - HA

### Cloud SQL

- MySQL/PostgreSQL
- Auto replication
  - Vertical scaling (machine type)
  - (weak) Horizontal scaling (# of instaces)
- [`gcloud sql`](https://cloud.google.com/sdk/gcloud/reference/sql)

### Cloud Firestore

- 2 modes:
  - Datastore mode -> server
  - Native mode -> mobile & web app

## Network Resources

### VPC

- [Docs](https://cloud.google.com/vpc/docs/using-vpc)
- Contained in a project
- **VPC has global scope** but **subnets are regional** (and cross zones)
- Important features
  - built-in route table
  - built-in global distributed firewall

#### Netowrks

- Default quota: 5 networks in a project
- Has no IP range
- Global and span all regions
- Contains **subnetworks**
- Types:
  - Default
    - one subnet per region
    - default firewall rules: allow all ingress from ICMP, RDP & SSH and all internal traffic
  - auto-mode
    - one subnet per region
    - regional IP alloc. (cannot overlapped with other subnets in the same network)
    - /20 mask (can be expanded up to /16)
    - all in 10.128.0.0/9
    - default firewall rules: deny all ingress, allow all egress
  - custom-mode
    - no default subnets
    - full control of IP ranges
    - regional IP alloc.
- Conversion
  - OK: default/auto -> custom
  - No: custom -> default/auto
- Communication
  - VMs in same network: using internal IP
  - VMs in different network (even in same region): using external IP by default
- 4 reserved IP in each subnet
  - 1st: network gateway
  - 2nd: subnet gateway
  - 2nd-to-last & last: for broadcast
- example:
  - VMs in same subnet but different zones
    - still communicate with same subnet IP
    - single firewall rule can apply to both VMs
- IP range of a subnet cannot be shrinked

#### IP

- Each VM has internal & external IPs
  - Internal
    - Allocated from subnet to VM with DHCP
    - DHCP lease renew every 24h
    - Register to network-scope DNS with (VM name, IP)
  - External
    - Can be
      - Ephemeral (assiged from pool)
      - Reserved
    - VM does not know its external IP (mapped to internal IP)
      - Handled by using internal DNS resolver

#### Routes & Firewall Rules

- Every network has
  - routes: let instances in a network send traffic directly
  - s default route: direct packets to dest. outside the network

#### Pricing

- Network
  - Ingress: no charge
  - Egress with no charge:
    - to same zone via internal IP
    - to Google products (YouTube, map, etc.)
    - to different GCP services within same region
  - Egress with chareg (\$0.01 per GB):
    - to instances in another zone in the same region
    - to same zone via external IP
    - between regions in US & Canada
  - Egress between regions outside US & Canada: varies by region
- External IP address
  - static IP (assigned but unused): \$0.01 per hour
  - static/ephemeral IP used on standard VM: \$0.004 per hour
  - static/ephemeral IP used on preemptible VM: \$0.002 per hour
  - static/ephemeral IP attached to forward rules: no charge

### Cloud Load Balancing

- [Brief Intro](https://www.coursera.org/learn/preparing-cloud-associate-cloud-engineer-exam/lecture/BvaXg/planning-and-configuring-network-resources)
- [Cloud LB docs](https://cloud.google.com/load-balancing/docs)

|             |         HTTP(S)         |     SSL Proxy     |                     TCP Proxy                      |                Network UDP/TCP                 | Internal UDP/TCP |     Internal HTTPS      |
| :---------: | :---------------------: | :---------------: | :------------------------------------------------: | :--------------------------------------------: | :--------------: | :---------------------: |
|    types    |       HTTP/HTTPS        | TCP with SSL load | TCP without SSL load / doesn't preserver client IP | TCP/UDP without SSL load / preserver client IP |    TCP or UDP    |      HTTP or HTTPS      |
|    scope    |    global, IPv4/IPv6    | global, IPv4/IPv6 |                 global, IPv4/IPv6                  |                 regional, IPv4                 |  regional, IPv4  |     regional, IPv4      |
|    ex/in    |        external         |     external      |                      external                      |                    external                    |     internal     |        internal         |
| port for LB | HTTP:80/8080, HTTPS:443 |     specific      |                      specific                      |                      Any                       |       Any        | HTTP:80/8080, HTTPS:443 |

### Interconnections

|       Connection       |                         Provides                         |            Capacity             |           Requirements            | Access Types |
| :--------------------: | :------------------------------------------------------: | :-----------------------------: | :-------------------------------: | :----------: |
|    IPsec VPN tunnel    | encrypted tunnel to VPC networks throughpublic internet  |      1.5-3Gbps per tunnel       |        on-prem VPN-gateway        | internal IP  |
| Dedicated Interconnect |       dedicated, direct connection to VPC netwroks       | 10Gbps per link (up to 8 links) | connection in colocation facility | internal IP  |
|  Partner Interconnect  |  dedicated connection to VPC through a service provider  |  50Mbps-10Gbps per connection   |         service provider          | internal IP  |
|     Direct Peering     |    dedicated, direct connection to Google's netwroks     |        10 Gbps per link         |      Connection in GCP PoPs       |  public IP   |
|    Carrier Peering     | Peering through Google's public network through provider |             varies              |         service provider          |  public IP   |

- VPN
  - low volume data connections (MTU < 1460 bytes)
  - static routes & dynamic routes (Cloud Router, border gateway protocol (BGP))
- Interconnect
  - |                    |       dedicated        |        shared        |
    | :----------------: | :--------------------: | :------------------: |
    | L3 (via public IP) |     Direct Peering     |   Carrier Peering    |
    |   L2 (via VLAN)    | Dedicated Interconnect | Partner Interconnect |
  - Dedicated Interconnect: Need to provision an cross-connect between Google network & own router, & establish BGP session between Cloud Router & on-premises router
  - Partner Interconnect: Need to connect to supported provider
- Peering
  - No SLA
  - Reach all Google services (GSuite, YouTube)
  - Google's edge points of presence (PoPs)
- Shared VPCs
  - Multiple projects share one VPC
    - One host project
    - Multiple service projects
  - Centralized
- VPC Network Peering
  - Connection accross two VPC networks (no matter they are within same project/organization or not)
  - Decentralized

## Stackdriver

- Monitoring
- Logging
- Debug
- Error reporting
- Trace

* [Stackdriver Fundamentals Quest](https://www.qwiklabs.com/quests/35)
* [Docs - Intro to alerting](https://cloud.google.com/monitoring/alerts/)
* [Docs - Managing alerting policies](https://cloud.google.com/monitoring/alerts/using-alerting-ui)

## Big Data Platform

### Cloud Dataproc

- Based on Hadoop
- Scale without terminating jobs
- Reduce cost with preemptible instances

### Cloud Dataflow

- ETL pipeline
- Data analysis
  - batch computation
  - continuous computation using streaming

### BigQuery

- data warehouse
- no cluster maintenance is required
- fast query
- pay separately for storage & queries
- reduce cost automatically if sustained usage
- ```shell
  # show process bytes -> check price with price calculator
  bq query --dry_run
  ```

### Cloud Pub/Sub

- "At least once" delivery

### Cloud Datalab

- Built on Python Jupyter

## Cloud Marketplace

- Tool for fast deploying functional packages, e.g. LAMP, Wordpress
- GCP doesn't update/fix the services that are already deployed

## Deployment Manager

- [Docs](https://cloud.google.com/deployment-manager/docs/)
- [Docs - fundamentals](https://cloud.google.com/deployment-manager/docs/fundamentals)

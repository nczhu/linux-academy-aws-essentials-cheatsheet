## **1. Create an AWS Account**

## **2. IAM: Identity and Access Management**

- **IAM**: where we manage users and access controls

Don't give individual users permissions. Always create group level permissions and add users to groups.

- **Policies**: e.g. admin, EC2 full, permissions that users can get access to

**Attaching AWS services to each other**

- **Role**: attached to services , that need access to policies, e.g. . You can attach roles to policies. **This is how I connect 2 AWS services (e.g. EC2 <> S3), or an external service, to AWS.**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e088b7e-dba7-45ff-810a-71ed9ce97edf/Screenshot_2020-04-26_at_19.01.47.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e088b7e-dba7-45ff-810a-71ed9ce97edf/Screenshot_2020-04-26_at_19.01.47.png)

## **3. VPC: Network services**

- Availability zones: redundant data centers for a region for high availability/redundancy
- **Virtual Private Cloud**: a private section of AWS that I control, into which I can put resources like EC2 instances and databases.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcc4b7b9-de0e-454a-8e05-c750bc1c59a4/Screenshot_2020-04-26_at_19.21.41.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcc4b7b9-de0e-454a-8e05-c750bc1c59a4/Screenshot_2020-04-26_at_19.21.41.png)

- **IGW**: Internet Gateways. Hardware/software that provides private network with a **route** to outside world. Provides connectivity for my VPC into the internet.
    - **AWS IGW**: highly available and redundant. By default, attached to our VPC. Only one VPC to one IGW.
- **Route tables**: a set of rules, routes, that determine where network traffic is directed. Default VPC already has a main route table. looks like: 172.16.0.0. You can have multiple active route tables for each VPC.
    - Destinations: 172.31.0.0 - 16 anything in this range is local subnet.
    - Default 0.0.0.0 means all other IP ranges. i.e. anything that's not local subnet goes through internet gateway.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1a9781d5-151d-4686-9381-347f161a8d43/Screenshot_2020-04-26_at_20.20.01.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1a9781d5-151d-4686-9381-347f161a8d43/Screenshot_2020-04-26_at_20.20.01.png)

- **NACL**: Network Access Control List, works like a (optional) firewall that controls traffic in/out of one or more **subnets**. Decides whether to block or allow certain communication.

    Not necessarily attached to a VPC.

    - AWS: under security → Network ACLS. Associated with subnets → Edit inbound/outbound rules. Allow specific IPs in and out.
    - Rules: **processed in order from lowest to highest**. First matching rule is processed.
    - 6 default subnets pointed to one NACL. A subnet can only be pointed to one NACL at a time.
- **Subnets**: you can add one or more subnets in each Availability zone.
    - Subnets contain resources. You provision AWS resources per subnet, e.g. an EC2 instance, a RDS, etc.
    - Needs to be attached to a **route table**. Associated with main route table by default.
    - **Public vs private subnet**: public subnet has a route to the internet, it has a gateway. Private subnets are not connected to a gateway.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f169aee5-ea97-4ded-9171-5e7b7bd267d9/Screenshot_2020-04-26_at_21.20.26.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f169aee5-ea97-4ded-9171-5e7b7bd267d9/Screenshot_2020-04-26_at_21.20.26.png)

- **Availability zones**: When **you create a VPC, it spans all availability zones in the region**. Highly available and fault tolerant
    - Avail zones are engineered to be isolate from failures in other avail zones.
    - **Recommended**: spin up EC2 instances in multiple availability zones. (Data would need to be replicated across), e.g.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/704bdfa7-bae5-456c-8804-66375ccc4291/Screenshot_2020-04-26_at_21.38.05.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/704bdfa7-bae5-456c-8804-66375ccc4291/Screenshot_2020-04-26_at_21.38.05.png)

An implementation of this diagram is: 
- 1 IGW attached to VPC
- 1 route table with route to internet
- 1 route table without route to internet
- A few private subnets, each in a different availability zone
- A few public subnets, each in a different availability zone

## **4. EC2: Compute services**

- **EC2**: computing capacity. Can use EC2 to launch as many VCs as i need, configure security and networking, and manage storage.
    - EC2 has all of the following: EBS (elastic block storage)

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee6796a3-fd52-412a-986f-6fdfc9a0c2c7/Screenshot_2020-04-28_at_16.37.11.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee6796a3-fd52-412a-986f-6fdfc9a0c2c7/Screenshot_2020-04-28_at_16.37.11.png)

- **Pricing**: on-demand, bill by hour (expensive but flexible), reserved for 1+ years (at discounts), spot- biding on [unused] instances (prices fluctuate, charged by minute). Charged for :
    - CPU: general purpose, acclerated computing
    - EBS optimized
    - AMI: like OS Linux or Window
    - Data transfer
    - Region

- **AMIs: Amazon Machine Images - a template for launching EC2 instances**. a preconfig package required to launch a EC2 that includes OS, software packages, and other required settings.
    1. Root Volume Template: contains the OS
    2. Launch Permissions: who is allowed to luanch it
    3. Block Device mappings
- Types of AMIs:
    - Community AMI: Linux, Redhat, Ubuntu, Windows server, only the OS, no addn software pckgs
    - AWS Marketplace AMIs: come packaged with addn licensed software from vendors
    - My AMIs: custom ones I made myself

- **Instance type**: CPU of the EC2 instance. Each type has diff compute, memory and storage capabilities. Components:

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b922c977-d70c-446e-bde8-5fa202c6d05d/Screenshot_2020-04-28_at_16.53.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b922c977-d70c-446e-bde8-5fa202c6d05d/Screenshot_2020-04-28_at_16.53.21.png)

    - Family: general or optimized. Type - affects vCPU assigned. Storage: EBS, SSD.

- **EBS: like a hard drive attached to EC2 instances**, its a storage volume for a EC2 instance. Persists independently from any EC2 instance as long as they are in the same Avail Zone.
    - **IOPS**: input outputs per second, amount of data that can be written/read from storage per second. SSD are small. HDD are larger. Determined by EBS volume size
    - Every EBS instance must have a **root volume,** like a default hard drive. Check: `delete on termination`. Create custom volumes in Elastic Block Store > Volumes

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/093cf3de-0333-4656-bbfc-23fd56dbdc66/Screenshot_2020-04-28_at_16.59.55.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/093cf3de-0333-4656-bbfc-23fd56dbdc66/Screenshot_2020-04-28_at_16.59.55.png)

    - **Snapshot**: image of an EBS volume that can be stored as a backup, or used to create a dupe for EBS volume. It's like a cache, can't be directly attached to EC2, can only be used to restore.

- **Security groups**: a virtual firewall that allows/denies traffic, similar to NACLs, but it's at the instance level instead of subnet level (NACL). A single instance can have multiple security groups.
    - Key diff: We can't create deny rules for sec groups though. No rule numbers. Security groups are **stateful:** traffic is allowed to enter into sec group is allowed to returned to its source.
    - Be careful about conflicts btw NACL and security groups.
    - **When you create a new SG, all inbound traffic is denied. All outbound traffic is allowed.**
    - Default sec group is auto created: all traffic is allowed inbound and outbound.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f8dde35d-2a6f-40e9-af25-2325671adbc2/Screenshot_2020-04-28_at_17.09.30.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f8dde35d-2a6f-40e9-af25-2325671adbc2/Screenshot_2020-04-28_at_17.09.30.png)

- **IP Addressing**: providing EC2 instance with public IP address.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33d344b7-98a8-4c61-983b-052b48a4af81/Screenshot_2020-04-28_at_17.23.16.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33d344b7-98a8-4c61-983b-052b48a4af81/Screenshot_2020-04-28_at_17.23.16.png)

### **Launching EC2 instance steps:**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71775341-585a-42be-bcda-a1071aa7f027/Screenshot_2020-04-28_at_17.27.01.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71775341-585a-42be-bcda-a1071aa7f027/Screenshot_2020-04-28_at_17.27.01.png)

- Bash script goes into Configure Instance > advanced details > user data > as text
- Check setup by ssh'ing in, and check with `ifconfig`

## **5. S3: Storage services**

- **Simple Storage Device**: virtual hard drive to store files, any type of file.
- **Bucket**: root level folder. Can contain sub **folders**
    - ALL bucket names have to be globally unique! all lower case. 3-63 chars in length
- Note: Some AWS services only work in the same region

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27a555e9-72da-438c-865a-2feee689e789/Screenshot_2020-04-30_at_14.14.43.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27a555e9-72da-438c-865a-2feee689e789/Screenshot_2020-04-30_at_14.14.43.png)

- **Storage classes:** classification of object in S3
    - Each obj must be assigned to obj class, e.g. standard, intelligent tiering, etc. to help differentiate access frequency vs long lived data, etc.
    - Standard: most expensive, 11-nine durability, highly available
    - Intelligent tiering: aws dynamically moving things to-fro tiers
    - Standard-infrequent: infreq access objs that need to be immediately available when accessed.
    - One zone infreq: non-critical, reproduceable objects, restricted to a single availability zone.
    - Glacier / deep archive: long term archival storage, can take hours to retrieve. Lowest cost. For highly regulated industries that need to keep records for 10+ years.
- **Durability**: % chance over 1 year time that a file in S3 will not be lost.
- **Availability**: % chance over 1 year time that a file in S3 will be accessible.

- **Object Lifecycle**: rules that automate the migration of objs storage class to a different storage class, based on time intervals. Lets me change file storage class to meet usage needs and keep costs low.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3ec7792-1be0-4e9e-8d05-c7707f1db7e6/Screenshot_2020-04-30_at_14.37.24.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3ec7792-1be0-4e9e-8d05-c7707f1db7e6/Screenshot_2020-04-30_at_14.37.24.png)

    - S3 allows large files to break up into a multi-part upload. S3 lets us auto-delete incomplete multi-part upload orphans.

- **Permissions**: can be set on bucket or obj level.
    - **ACL**: Block pub access by default. To give team access, you have to make bucket public, and then make the obj public. **List objects**: so people can see what's in our bucket.

- **Versioning**: keeps track / stores all versions of a bucket's objs so I can use an older version. On the bucket level.
    - On or OFF, once it's on, you can only suspend. cannot be turned OFF again.
    - Bucket > Properties > Versioning
    - Now I can access various versions for all my objs in the budget

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfb02f60-11d1-4625-8b44-45503e486474/Screenshot_2020-04-30_at_14.47.37.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfb02f60-11d1-4625-8b44-45503e486474/Screenshot_2020-04-30_at_14.47.37.png)

## **6. RDS (sql) / Dynamo (nosql): Database services**

- Important: **RDS should not accessible by the internet**
- **RDS**: SQL db service with options like: Amazon aurora (best performance characteristics, no free tier), MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL. AWS manages the underlying DB.
    - RDS pricing: depends on instance size, terms: on demand or reserved, db storage total size, and data transfer rate.
- **DynamoDB**: noSQL, doesn't have other nosql options; meant to replace mongo/cassandra
    - Pricing: throughput capacity, data storage, streams, reserved capacity, data transfer rate

**Provisioning an RDS**

- **RDS needs to be in a private subnet, connected to route table to EC2 instances, not the public internet.**

    So the Dev team connects to EC2, which then is connected to RDS

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36a400c9-d7f3-4103-a841-2e576166f58a/Screenshot_2020-04-30_at_15.33.08.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36a400c9-d7f3-4103-a841-2e576166f58a/Screenshot_2020-04-30_at_15.33.08.png)

- **SSH tunneling**: gives us ability to connect through the internet, to EC2, though route table, to RDS instances.
- **To create an RDS:**
1. First define subnet groups that will contain the RDS instances. Then create the RDS. 
2. RDS will create a security group for this subnet by default. Enable ssh access.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84dec42c-47b0-483a-a81e-042bfea26aaf/Screenshot_2020-04-30_at_15.40.54.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84dec42c-47b0-483a-a81e-042bfea26aaf/Screenshot_2020-04-30_at_15.40.54.png)

3. AWS console cannot connect to RDS db. have to download mysql workbench: Configs: 
    1. Connection method: TCI/IP over SSH
    2. SSH Hostname: EC2 instance IP address
    3. SSH username: ec2-user
    4. SSH pw: use ec2 keyfile
    5. mysql hostname: endpoint
    6. mysql username: admin
    7. mysql pw: the pw i created when creating RDS

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e33303f3-905c-44c9-a18d-0739111600b7/Screenshot_2020-04-30_at_16.00.23.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e33303f3-905c-44c9-a18d-0739111600b7/Screenshot_2020-04-30_at_16.00.23.png)

## **7. SNS: Simple Notification**

## **8. Cloudwatch/CloudTrail: Management Tools**

## **9. ELB / auto-scaling**

## **10. Route 53: domains, DNS**

## **11. Lambda: serverless compute**
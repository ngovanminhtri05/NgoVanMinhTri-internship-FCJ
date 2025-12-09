---
title: "University of California Irvine Backs Up Petabytes of Research Data to AWS"
date: 2025-05-29
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# University of California Irvine Backs Up Petabytes of Research Data to AWS

**By Philip Papadopoulos, Abhijeet Lokhande, Evan Wood, Francisco Ramon Lopez, and Nicholas Santucci | 29 MAY 2025**

**Categories:** Amazon Athena, Amazon CloudWatch, Amazon DynamoDB, Amazon EventBridge, Amazon S3 Glacier Deep Archive, Amazon Simple Notification Service (SNS), Amazon Simple Storage Service (S3), AWS Lambda

---

## Editor's Note

AWS is not responsible for UCI's public GitHub repo linked in this post, which has been provided so that interested parties can explore the solution described in this post in more detail.

---

The University of California, Irvine (UCI) is a public land-grant research university with troves of research data stored on servers in lab environments on about 1500 faculty-research lab environments on campus. UCI needed a solution to address the practical and economic challenge of providing centralized backups for these independently-administered servers.

The goal for the UCI Research Cyber Infrastructure Center (RCIC) is to provide the tooling and backup storage capacity server owners need to perform backups while also providing more data protection in the event of deletions, accidental or otherwise. The UCI RCIC surveyed the existing backup solutions deployed independently around campus and found none that meet their cost requirements, dynamic range of scale, and accommodations for the reality of central as opposed to distributed system administration. Existing outliers of systems can have as much as a Petabyte on single server and upwards of one billion files. UCI had about 100 servers with an estimated total storage of 10 PB that needed to be backed up.

In this post we walk through the design choices for UCI's centralized backup solution, including how Amazon S3 buckets, AWS Identity and Access management (IAM) roles, and AWS accounts are programmatically created per-server to provide the proper isolation. We will also provide an overview of how specific requirements are mapped to AWS services and rclone invocation. With this solution, UCI was able to backup over 5 PB of data (over 250 million files) with an efficient, scalable system that featured a compact and easily maintainable amount of code.

---

## Solution Overview

The UCI RCIC developed a scalable backup solution to address the diverse needs of their 1500 faculty research labs. The implementation framework centers on a custom-developed system that uses [rclone](https://rclone.org/) for data movement while incorporating multiple AWS services for security and data management. This solution was specifically designed to handle the wide variance in server sizes across campus, ranging from 1 TB to 2 PB, with a total storage requirement of approximately 10 PB across 100 servers.

The implementation architecture establishes clear separation between system administration and cloud administration roles, enhancing security through segregation of duties. On-premises servers (Lab1 and LabN) use rclone to perform daily backups to S3 buckets. The S3 buckets are configured with S3 Lifecycle policies that move data to S3 Glacier Deep Archive for long-term storage. The system is monitored using Amazon CloudWatch with custom dashboards and alarms, while notifications are handled through Amazon Simple Notification Service (Amazon SNS). Additional AWS services and features like Amazon DynamoDB, AWS Lambda, AWS Step Functions, S3 Batch Operations, and Amazon EventBridge are used to control access and describe policy.

### Architecture Diagrams

![Figure 1: High-level view of backup to Amazon S3 using rclone. A myriad of AWS services are used to control access and describe policy. A custom Python3 wrapper around the open source tool rclone streamlines the description of backup jobs.](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/29/UCI-Done-Page-1.png)

**Figure 1:** High-level view of backup to Amazon S3 using rclone. A myriad of AWS services are used to control access and describe policy. A custom Python3 wrapper around the open source tool rclone streamlines the description of backup jobs.

---

### Key Components

To maintain cost efficiency while ensuring data protection, UCI RCIC developed a custom Python wrapper that streamlines backup job definitions and execution. This wrapper interfaces with AWS services including S3, IAM, Amazon CloudWatch, and Amazon SNS to provide comprehensive monitoring and management capabilities.

![Figure 2: A custom Python wrapper around the open source tool rclone streamlines the description of backup jobs](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/29/Figure-2-A-custom-Python-wrapper-around-the-open-source-tool-rclone-streamlines-the-description-of-backup-jobs.png)

**Figure 2:** A custom Python wrapper around the open source tool rclone streamlines the description of backup jobs

The solution incorporates three primary operational components:
- **Daily incremental backups** for efficient data protection
- **Weekly deep syncs** for comprehensive verification
- **Automated lifecycle management** for cost optimization

Implementing this tiered approach allows UCI to successfully balance the competing demands of performance, cost, and data security while maintaining system availability for research operations.

### Feature Implementation Matrix

| Feature/Capability | Technology/Service | Control |
| --- | --- | --- |
| Data movement/backup | rclone | sysadmin |
| Backup job definition | Custom yaml file | sysadmin |
| Backup job execution | UCI-Developed code wrapping rclone | sysadmin (in GitHub) |
| Backup job scheduling | Unix Cron, Windows Scheduled tasks | sysadmin |
| Deletion of files on backup | rclone running in sync mode | sysadmin |
| File overwrite/deletion protection | S3 Versioning | cloudadmin |
| Permanent deletion of backup | S3 Lifecycle policy | cloudadmin |
| Early deletion of data | S3 Admin – MFA required | cloudadmin |
| Backup destination provision | Custom scripts that target S3 and apply policy | cloudadmin |
| IP network limitations | IAM access policy | cloudadmin |
| Overview and per-server view | Cloudwatch dashboards | cloudadmin |
| Quotas and activity alarms | Cloudwatch alarms | cloudadmin |
| Restore files | S3 Glacier restore + custom python | cloudadmin/sysadmin |

---

## Approach to Centralized Backups

### A. Core Data Movement

#### Why rclone?

Rclone offers support for both Amazon S3 storage and local file systems, with built-in handling of S3 object versioning and direct integration with cloud storage APIs. The tool employs an incremental file system exploration methodology, enabling memory-efficient processing of large file systems, which has been verified with systems containing over 50 million files. When rclone needs to synchronize the contents of the local file system with the copy in Amazon S3, it explores the local file system incrementally. UCI has single backup jobs with over 50 million files and rclone fully explores the file system without exhausting system memory.

Rclone is also natively multi-threaded. For example, when making the first "full" backup of a 1 PB file server, UCI had "scaled back" rclone so that it did not saturate the 10 GbE link and the file server was still capable of serving files. In short, it's performant but can be governed so that it doesn't make the system unavailable.

UCI developed a small Python script (gen-backup.py in UCI's public [GitHub](https://github.com/RCIC-UCI-Public/rcs3) repository) that wraps Rclone. This code interprets a YAML-formatted file that defines one or more backup jobs.

#### Sample Backup Job Definition

```yaml
## Path is common to the jobs relative to the path
---
srcpaths:
- path: /datadir
  ## Local decision to exclude .git subdirs
  exclude_global:
    - .git/**

  ## Patterns from a file to exclude
  exclude_file: common_excludes.yaml

  jobs:
    - name: backup1
      subdirectories:
        - DataImages
```

In a nutshell, the job called "backup1" replicates all files in the path `/datadir/DataImages` and excludes common file patterns. To run all backup jobs, only the following command needs to be issued:

```bash
gen-backup.py run
```

There are numerous options to gen-backup to govern parallelism, which jobs are run, and in what mode they are run.

#### Data Transfer Implementation Steps

1. **Configuration:** UCI configured individual backup profiles for each research lab server
2. **Job definition:** Created standardized YAML templates for consistent backup definitions
3. **Execution:** Implemented automated backup processes through custom wrapper
4. **Monitoring:** Established lab-specific monitoring protocols
5. **Optimization:** Developed transfer rate controls based on server capacity
6. **Scheduling:** Created lab-specific backup schedules based on data change rates

This structured approach ensures consistent, reproducible backup operations while maintaining system performance and reliability within defined operational parameters.

### B. Storage Quota for Labs

The central campus IT team manages backup costs, necessitating implementation of specific controls to prevent cost overruns. To achieve this, the following factors needed to be understood for each server:

- How much data in terms of volume (in terabytes) is stored?
- How many files are in the system (in 1 million object increments)?
- Does access to the backup S3 bucket look "abusive" (too many transactions)?
- Has a system made no API calls to the bucket (backup could be off on the server)?

Each one of these bullets is configured as a CloudWatch alarm. Both the cloudadmin and the appropriate sysadmin(s) are notified whenever any of these alarms are triggered using Amazon SNS. There is a dashboard that allows cloudadmins to see the state of these alarms for all servers that are being backed up.

![Figure 4: Alarms set up per storage server: storage and object quotas and activity monitoring](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/23/image-3-6.png)

**Figure 4:** Alarms set up per storage server: storage and object quotas and activity monitoring

One such alarm monitored API calls, which was expressed as a ratio of requests to objects. If this ratio exceeds 3.5, then the alarm is triggered. 3.5 was chosen as an engineering cushion to not alarm under most operating circumstances.

Over the almost year-long period of experience, these alarms have proven useful to alert UCI on various states—especially on terabytes and number of objects limits.

### C. Monitoring and Visibility

The monitoring system needed to accommodate 50-100 servers operating on independent backup schedules. With end systems being managed by disparate and disjoint administrators, only the data in AWS can be used as a central aggregation point. For cost containment, UCI is most concerned with averages. Some servers may have many small files or significant data churn (resulting in higher percentages of non-current objects) and thus are "more expensive per terabyte." These are balanced by other servers that have relatively large files and/or low data churn and thus are "less expensive per terabyte."

UCI built a custom dashboard in CloudWatch that combines metrics from all backup buckets, teases out different API calls to estimate ongoing cost. The top widget shows 2780 TB total storage across 186 million files. Of this total storage 2690 TB is in S3 Glacier Deep Archive, and 454 TB is held as deleted or overwritten files that have not been permanently deleted through an S3 Lifecycle policy. The last figure indicates a 16.3% overhead for retaining deleted/changed objects.

![Figure 5: Cost-Estimates dashboard showing detailed storage breakdown, API activity, and storage costs with Glacier Deep Archive configuration](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/23/image-4-6.png)

**Figure 5:** Overall storage, API, and cost utilization across all servers

The weekly spikes on the API chart are from a weekly deep sync. A careful inspection also shows a ramp up of standard storage 2 TB at beginning of the monitoring window and 88.1 TB at the end. This is explained by the onboarding of a new 1 PB server.

The dominant fraction of UCI's monthly backup bill is owed to the cost of storage itself, which accounts for about 80% of monthly cost. An analysis of the July 2024 bill showed:
- 80% storage
- 1.5% CloudWatch
- 18.5% API

An additional cost not shown that is important to be cognizant of is the S3 Lifecycle transition cost when data is moved from Amazon S3 Standard (where it is initially placed) to S3 Glacier Deep Archive.

#### Per-Server Monitoring

The overview dashboard is helpful, but per-server high-level views are also critical to insight. A second cost estimate dashboard was configured to get this detailed insight. Systems at UCI range in capacity from 11 TB to 1040 TB and object counts in the range of 652,000 to 59.4 million. This dashboard replicates the top line information of the roll-up view, but it is tailored to each system.

**Figure 6:** Per-server storage utilization and cost

All dashboards shown are built programmatically so that they are easily replicated in another environment.

### D. Performant Backups

When dealing with large data servers, volume (terabytes) and number of files (number of objects) are critical factors that affect performance. It's perhaps easiest to illustrate these orders of magnitude with some examples:

- 1024 TB @ 1 gbps – 10 million seconds to transmit (100 Days)
- 1024 TB @ 10 gbps – 1 million seconds to transmit (10 days)
- 50 million file syncs at 100 checks/second = 500,000 seconds (5.8 days)
- 50 million file syncs at 1000 checks/second = 50,000 seconds (13.8 hours)

For backup to the cloud to be practical on 1 PB servers, the first copy of data needs to move at rates far exceeding 1 gbps. When evaluating existing software, it becomes evident that any backup strategy that necessitates routine "full" backups of these kinds of servers are impractical. Although seeding a large backup can take significant time (weeks in extreme cases) and is unavoidable, ongoing backup needs to be "forever incremental."

The second metric (checks/sec) is not so obvious at first glance. To synchronize the contents of two file "systems," the backup software must verify that everything that is local is on the backup. Furthermore, everything that has been deleted from the local must also be deleted from the backup. Rclone performs this with "checkers" that check whether the local and cloud copies are in sync. Because each check is an over-the-network API call with tens of milliseconds per call, many such checks must occur in parallel. Rclone does this natively and UCI routinely observes approximately 2000 checks/second.

Alternatively, an organization could keep a local database of the state of each file and reduce the regular "sync." That's a tradeoff between space (a local database) and time (weekly sync check). Many commercial backup programs do this (and must always have a way of re-establishing truth if the local database is corrupted). The management of such a database increases complexity.

### E. Low Cost

Cost optimization was a key requirement, necessitating careful analysis of AWS feature usage for cost containment. UCI had to understand some of the basics of how rclone interfaces with Amazon S3, and then do the multiplication. After examining practical performance issues, UCI settled on weekly "deep syncs" of file systems. The deep syncs resulted in a large number of API calls and tax the local file system. Rclone has a mode called a "top up" sync. It only considers files that are new or newly written for comparison to the cloud copy. Systems run top-ups six days/week and a deep sync one day/week.

### F. Password Rotation and Detecting Non-Running

The backup script (gen-backup.py) must be able to run unattended from a task scheduler (for example cron on Linux-like systems, Task Scheduler on Windows). To do this, rclone must have access to valid credentials. This provides somewhat of an engineering conundrum: temporary (short-lived) keys might expire before a backup is completed while long-lived credentials pose their own risks. Rclone to S3 for large backup (RCS3) "splits the middle": credentials (specific to the backup process itself) are long-lived but are expired after the completion of every successful invocation of the backup program (gen-backup.py). Under normal operation, this occurs daily.

When a bucket is set up for backup, a service account (backup user) is created and this account must have the privilege to create a new access key/secret access key pair just for itself. This compromise works very nicely. When rclone starts, the key being used doesn't expire while the backup is ongoing, thus preventing wasteful restarts of partially-completed backups. When completed, the key that was just used is expired and a new one is created. Under normal operations the key has a lifetime of 24 hours.

One can use the fact that normal operations should see a key update every day. This triggers an alarm if a particular key has an age of more than 48 hours. This indicates that either: a backup is taking longer or a backup isn't getting to the key rotation step. The latter case is always a problem. The root cause of the alarm necessitates the sysadmin to investigate.

---

## Conclusion

This comprehensive backup solution developed by UCI RCIC demonstrates how AWS services can be effectively leveraged to address the complex challenge of backing up massive amounts of research data across numerous faculty labs. Through the strategic implementation of rclone, custom Python wrappers, and various AWS services including S3, IAM, CloudWatch, and SNS, UCI successfully created a scalable system capable of backing up over 5 PB of data comprising more than 250 million files.

The solution's architecture effectively balances critical requirements including cost efficiency, performance optimization, security controls, and monitoring capabilities while maintaining a clear separation between system and cloud administration roles.

### Key Benefits

- **Cost Savings:** Intelligent storage tiering and lifecycle policies
- **Security:** Segregation of duties and strict access controls
- **Monitoring:** Comprehensive capabilities with both high-level and detailed per-server insights
- **Scalability:** Handles servers ranging from 1 TB to 2 PB while maintaining system performance

For organizations looking to implement a similar solution, UCI's approach demonstrates that with careful planning and the right combination of tools and services, even the most demanding backup requirements can be met effectively.

### Recommended Resources

- [UCI's public GitHub repository](https://github.com/RCIC-UCI-Public/rcs3)
- [RCS3 Documentation](https://rcs3.readthedocs.io/en/latest/about/index.html)

The documentation provides an overview of the two administrative sides of backup, and what is needed in terms of software (Python, Boto3 library, and rclone), initializing the cloud environment for backup, onboarding servers, creating backup jobs, defining quotas, and updating dashboards.

### Future Roadmap

RCS3 has demonstrated the effectiveness of managing Petabyte-scale backups while maintaining cost efficiency. Our near-term to do list includes:

- Unassisted (no cloudadmin intervention) restore operations
- Documentation of various backup job options that can deal with common issues
- Details on setting up all the system variants that have been encountered so far

---

## About the Authors

### Philip Papadopoulos
Philip Papadopoulos is the Director of the Research Cyberinfrastructure Center (RCIC) at the University of California, Irvine (UCI). RCIC serves the campus research community via a mid-scale (roughly 11K cores and 150 GPUs) computing cluster and about 10 PB of parallel storage. Dr. P. started working in the parallel/distributed computing community at Oak Ridge National Laboratory as part of the PVM (precursor to MPI) in the late 1990s. Prior to UCI, he spent nearly twenty years at the San Diego Supercomputer Center building supercomputing systems and developing the Rocks cluster toolkit.

### Abhijeet Lokhande
Abhijeet Lokhande is a Senior Solutions Architect at AWS, where he plays a pivotal role in empowering India's most innovative startups. Working within the AWS Startups team, he serves as a strategic advisor, helping emerging companies transform their ambitious visions into scalable cloud architectures. With deep expertise in security and compliance, Abhijeet guides founders through the complexities of building secure, enterprise-grade applications on AWS.

### Evan Wood
Evan is a Solutions Architect working with the Amazon Web Services (AWS) Worldwide Public Sector team. He works with the Department of Energy in the Federal Civilian space, helping them accelerate their Cloud journey. Evan specializes in IoT deployments and databases.

### Francisco Ramon Lopez
Francisco is an IT professional working in the Research Cyberinfrastructure Center (RCIC) at the University of California, Irvine. He has 25+ years of experience with UNIX systems and began working with AWS in 2015.

### Nicholas Santucci
Nick Santucci is a High-Performance Computing (HPC) Systems Engineer at UC Irvine's Research Cyberinfrastructure Center (RCIC), part of the Office of Data and Information Technology (ODIT). He specializes in software integration, cluster computing, and high-performance storage, supporting the RCIC's mission to provide scalable computing and storage resources for the campus research community. Before joining UC Irvine to work in high performance computing/research computing, he spent a decade at DIRECTV, where he developed and migrated automated monitoring systems for broadcast operations, shifting the practice from tool-driven monitoring to data-driven IT analytics. Nick holds a bachelor's degree in information and computer science from UC Irvine, with a specialization in network and distributed systems.

---

**Tags:** Amazon Athena, Amazon CloudWatch, Amazon DynamoDB, Amazon EventBridge, Amazon S3 Batch Operations, Amazon S3 Glacier Storage Classes, Amazon Simple Storage Service (Amazon S3), Amazon SNS, AWS Cloud Storage, AWS Lambda

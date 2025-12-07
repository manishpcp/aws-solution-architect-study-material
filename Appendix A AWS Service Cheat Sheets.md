# Appendices

# Appendix A: AWS Service Cheat Sheets

## Compute Services Quick Reference

### Amazon EC2 (Elastic Compute Cloud)

```
SERVICE OVERVIEW:
Virtual servers in the cloud
Pay for compute capacity by the hour/second
Full control over OS and configuration

INSTANCE FAMILIES:
T-Series (Burstable):
- t3.nano to t3.2xlarge
- Baseline CPU with burst credits
- Use case: Variable workloads
- Cost: $0.0052/hr (t3.micro)

M-Series (General Purpose):
- m5.large to m5.24xlarge
- Balanced compute, memory, network
- Use case: Web servers, applications
- Cost: $0.096/hr (m5.large)

C-Series (Compute Optimized):
- c5.large to c5.24xlarge
- High performance processors
- Use case: Batch processing, HPC
- Cost: $0.085/hr (c5.large)

R-Series (Memory Optimized):
- r5.large to r5.24xlarge
- Memory-intensive applications
- Use case: Databases, caches
- Cost: $0.126/hr (r5.large)

P-Series (GPU):
- p3.2xlarge to p3.16xlarge
- NVIDIA Tesla V100 GPUs
- Use case: ML training, rendering
- Cost: $3.06/hr (p3.2xlarge)

PRICING MODELS:

On-Demand:
- Pay by hour/second
- No commitment
- Most expensive
- Use: Unpredictable workloads

Reserved Instances:
- 1 or 3 year commitment
- Up to 72% discount
- Payment: All/Partial/No upfront
- Use: Steady-state workloads

Spot Instances:
- Bid on spare capacity
- Up to 90% discount
- Can be interrupted
- Use: Fault-tolerant workloads

Savings Plans:
- 1 or 3 year commitment
- Up to 72% discount
- Flexible across instance types
- Use: Variable but consistent usage

KEY LIMITS:
- Default: 20 instances per region
- Request increase: Up to 1000s
- Max EBS volumes per instance: 28
- Max network interfaces: Varies by type
- Max instance store: Varies by type

EXAM TIPS:
✓ Burstable (T) for variable workloads
✓ Reserved for 24/7 predictable
✓ Spot for batch/fault-tolerant
✓ Placement groups for HPC
✓ Enhanced networking for high throughput
```


### AWS Lambda

```
SERVICE OVERVIEW:
Serverless compute (no servers to manage)
Pay only for compute time consumed
Automatic scaling from zero to thousands

CONFIGURATION:

Memory: 128 MB to 10,240 MB (10 GB)
- CPU scales proportionally with memory
- More memory = faster execution (but higher cost)

Timeout: 1 second to 15 minutes (900 seconds)
- Default: 3 seconds
- Set based on function needs

Ephemeral Storage: 512 MB to 10,240 MB
- Temporary storage at /tmp
- Cleared between invocations

Concurrency:
- Default: 1000 concurrent executions per region
- Reserved concurrency: Guarantee for function
- Provisioned concurrency: Pre-warmed instances

INVOCATION TYPES:

Synchronous:
- Caller waits for response
- Use: API Gateway, ALB, direct invoke
- Retry: Client responsibility

Asynchronous:
- Lambda queues event and returns immediately
- Use: S3, SNS, EventBridge
- Retry: Automatic (2 retries)

Event Source Mapping:
- Lambda polls event source
- Use: SQS, Kinesis, DynamoDB Streams
- Batch processing

PRICING:
Requests: $0.20 per 1M requests
Compute: $0.0000166667 per GB-second
- 1GB memory, 1 second = $0.0000166667
- 128MB memory, 100ms = $0.0000002083

Free Tier (monthly):
- 1M free requests
- 400,000 GB-seconds compute

Example Cost:
1M invocations, 512MB, 200ms average:
Requests: $0.20
Compute: 1M × 0.5GB × 0.2s × $0.0000166667 = $1.67
Total: $1.87/month

KEY LIMITS:
- Deployment package: 50 MB (zipped), 250 MB (unzipped)
- Environment variables: 4 KB total
- Layers: 5 per function
- Concurrent executions: 1000 per region (soft limit)
- Invocation payload: 6 MB (sync), 256 KB (async)

EXAM TIPS:
✓ 15-minute max timeout
✓ Stateless (use S3/DynamoDB for state)
✓ Cold starts (provisioned concurrency solves)
✓ EventBridge for cron jobs
✓ DLQ for failed async invocations
```


### Amazon ECS (Elastic Container Service)

```
SERVICE OVERVIEW:
Managed container orchestration
Run Docker containers at scale
Two launch types: EC2 and Fargate

LAUNCH TYPES:

EC2 Launch Type:
- You manage EC2 instances
- More control, lower cost
- Responsible for scaling, patching
- Use: Cost optimization, custom requirements

Fargate Launch Type:
- AWS manages infrastructure
- Serverless containers
- Pay only for vCPU and memory
- Use: Operational simplicity

TASK DEFINITION:
Defines container configuration
- Docker image
- CPU/Memory requirements
- Port mappings
- Environment variables
- Logging configuration

Example:
{
  "family": "web-app",
  "cpu": "256",
  "memory": "512",
  "containerDefinitions": [{
    "name": "web",
    "image": "nginx:latest",
    "portMappings": [{
      "containerPort": 80
    }]
  }]
}

SERVICES:
Maintains desired count of tasks
- Auto Scaling integration
- Load balancer integration
- Rolling deployments
- Service discovery

PRICING (Fargate):
vCPU: $0.04048 per vCPU per hour
Memory: $0.004445 per GB per hour

Example Task (0.25 vCPU, 0.5 GB):
Hourly: 0.25 × $0.04048 + 0.5 × $0.004445 = $0.0123
Monthly (24/7): $8.93

KEY LIMITS:
- Tasks per service: 10,000
- Services per cluster: 5,000
- Container instances per cluster: 5,000
- Task definition size: 64 KB

EXAM TIPS:
✓ Fargate for serverless containers
✓ EC2 for cost optimization at scale
✓ Service discovery with Cloud Map
✓ ECS Anywhere for on-premises
✓ Task roles for IAM permissions
```


## Storage Services Quick Reference

### Amazon S3 (Simple Storage Service)

```
SERVICE OVERVIEW:
Object storage service
11 nines durability (99.999999999%)
Unlimited storage capacity

STORAGE CLASSES:

S3 Standard:
- Availability: 99.99%
- Use: Frequently accessed data
- Retrieval: Instant
- Cost: $0.023/GB/month

S3 Intelligent-Tiering:
- Automatic cost optimization
- Monitors access patterns
- Moves between tiers automatically
- Cost: $0.023/GB + $0.0025/1000 objects

S3 Standard-IA (Infrequent Access):
- Availability: 99.9%
- Use: Long-lived, less frequent access
- Retrieval: Instant, retrieval fee applies
- Cost: $0.0125/GB + $0.01/GB retrieval

S3 One Zone-IA:
- Single AZ storage
- Use: Reproducible data
- Cost: $0.01/GB + $0.01/GB retrieval

S3 Glacier Instant Retrieval:
- Archive with instant access
- 90-day minimum storage
- Cost: $0.004/GB + retrieval fees

S3 Glacier Flexible Retrieval:
- Archive storage
- Retrieval: Minutes to hours
- Cost: $0.0036/GB + retrieval fees

S3 Glacier Deep Archive:
- Lowest cost archive
- Retrieval: 12 hours
- Cost: $0.00099/GB + retrieval fees

FEATURES:

Versioning:
- Keep multiple versions of objects
- Protect against accidental deletion
- Can be suspended (not disabled)

Replication:
- Cross-Region Replication (CRR)
- Same-Region Replication (SRR)
- Requires versioning enabled

Object Lock:
- WORM (Write Once Read Many)
- Governance mode: Admin can override
- Compliance mode: Cannot be deleted

Lifecycle Policies:
- Transition objects between classes
- Expire objects after days
- Example: Standard → IA (30 days) → Glacier (90 days)

S3 Transfer Acceleration:
- Use CloudFront edge locations
- 50-500% faster uploads
- Additional cost: $0.04/GB

SECURITY:

Bucket Policies:
- Resource-based permissions
- Allow/Deny access
- Conditions: IP, VPC, MFA

Access Control Lists (ACLs):
- Legacy permission mechanism
- Bucket-level or object-level
- AWS recommends policies over ACLs

Block Public Access:
- Account-level or bucket-level
- Prevents accidental public exposure
- Overrides bucket policies

Encryption:
- SSE-S3: S3-managed keys
- SSE-KMS: KMS-managed keys
- SSE-C: Customer-provided keys
- Client-side encryption

KEY LIMITS:
- Bucket name: 3-63 characters
- Buckets per account: 100 (soft limit)
- Object size: 5 TB max
- Single PUT: 5 GB max (use multipart > 100 MB)
- Parts in multipart: 10,000 max

EXAM TIPS:
✓ Standard for frequent access
✓ Intelligent-Tiering for unknown patterns
✓ Glacier for archives
✓ Cross-region replication for DR
✓ Versioning for data protection
✓ Object Lock for compliance
```


### Amazon EBS (Elastic Block Store)

```
SERVICE OVERVIEW:
Block-level storage for EC2
Persistent storage (survives instance stop)
Automatically replicated within AZ

VOLUME TYPES:

gp3 (General Purpose SSD):
- Size: 1 GB to 16 TB
- Baseline: 3,000 IOPS, 125 MB/s
- Max: 16,000 IOPS, 1,000 MB/s
- Use: Boot volumes, general workloads
- Cost: $0.08/GB/month + $0.005/IOPS (>3000)

gp2 (General Purpose SSD):
- Size: 1 GB to 16 TB
- IOPS: 3 IOPS per GB (min 100, max 16,000)
- Burst: 3,000 IOPS for smaller volumes
- Use: Legacy general purpose
- Cost: $0.10/GB/month

io2 Block Express (Provisioned IOPS):
- Size: 4 GB to 64 TB
- IOPS: Up to 256,000
- Throughput: Up to 4,000 MB/s
- Use: Critical databases, high IOPS
- Cost: $0.125/GB + $0.065/IOPS

io2 (Provisioned IOPS):
- Size: 4 GB to 16 TB
- IOPS: Up to 64,000 (io2 Block Express: 256,000)
- Durability: 99.999%
- Use: I/O intensive databases
- Cost: $0.125/GB + $0.065/IOPS

st1 (Throughput Optimized HDD):
- Size: 125 GB to 16 TB
- Throughput: 500 MB/s max
- Use: Big data, log processing
- Cost: $0.045/GB/month

sc1 (Cold HDD):
- Size: 125 GB to 16 TB
- Throughput: 250 MB/s max
- Use: Cold data, infrequent access
- Cost: $0.015/GB/month

FEATURES:

Snapshots:
- Incremental backups to S3
- Cross-region copy supported
- Restore to any AZ
- Cost: $0.05/GB/month

Encryption:
- AES-256 encryption
- Transparent (no performance impact)
- Uses AWS KMS
- Cannot change after creation

Multi-Attach (io1/io2 only):
- Attach to up to 16 instances
- Must be in same AZ
- Use: Clustered applications

KEY LIMITS:
- Volumes per instance: 28 (depends on instance type)
- IOPS per instance: Varies by type
- Throughput per instance: Varies by type
- Snapshot copies: 5 concurrent per region

EXAM TIPS:
✓ gp3 default choice (best price/performance)
✓ io2 for consistent high IOPS
✓ HDD for throughput, not IOPS
✓ Snapshots for backups and DR
✓ Encryption in-transit requires instance support
```


### Amazon EFS (Elastic File System)

```
SERVICE OVERVIEW:
Managed NFS file system
Elastic (grows/shrinks automatically)
Multi-AZ by default

STORAGE CLASSES:

Standard:
- Frequently accessed files
- Multi-AZ redundancy
- Cost: $0.30/GB/month

Infrequent Access (IA):
- Files not accessed for 30+ days
- Lower storage cost
- Retrieval fee applies
- Cost: $0.025/GB/month + $0.01/GB access

PERFORMANCE MODES:

General Purpose:
- Default mode
- Max 7,000 IOPS
- Lowest latency
- Use: Web serving, content management

Max I/O:
- Higher aggregate throughput
- Slightly higher latency
- Use: Big data, media processing

THROUGHPUT MODES:

Bursting:
- Throughput scales with size
- 50 MB/s per TB stored
- Burst to 100 MB/s
- Use: Variable workloads

Provisioned:
- Fixed throughput regardless of size
- Additional cost
- Use: Consistent high throughput

Enhanced (Elastic):
- Automatic scaling
- Up to 3 GB/s read, 1 GB/s write
- Pay for what you use
- Use: Unknown or varying throughput

FEATURES:

Lifecycle Management:
- Automatically move to IA
- Configurable: 7, 14, 30, 60, 90 days
- Transition back on access

Replication:
- Same-region or cross-region
- Continuous replication
- Point-in-time restore

Access Points:
- Application-specific entry points
- Enforce user identity
- Enforce root directory

PRICING:
Standard: $0.30/GB/month
IA: $0.025/GB/month
Throughput (provisioned): $6.00/MB/s/month

Example:
100 GB Standard, 10 GB IA, 5 MB/s provisioned:
Storage: 100 × $0.30 + 10 × $0.025 = $30.25
Throughput: 5 × $6.00 = $30.00
Total: $60.25/month

KEY LIMITS:
- File systems per region: 1,000
- Mount targets per file system: 400
- Throughput: Up to 10 GB/s
- Concurrent NFS connections: 1,000s

EXAM TIPS:
✓ Multi-AZ shared file system
✓ NFS protocol (Linux)
✓ Auto-scaling (pay for what you use)
✓ IA for cost optimization
✓ VPC mount targets for access control
```


## Database Services Quick Reference

### Amazon RDS (Relational Database Service)

```
SERVICE OVERVIEW:
Managed relational database
Automated backups, patching, scaling
Supports: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server

INSTANCE TYPES:

db.t3.micro: 2 vCPU, 1 GB RAM - $0.017/hr
db.t3.small: 2 vCPU, 2 GB RAM - $0.034/hr
db.m5.large: 2 vCPU, 8 GB RAM - $0.192/hr
db.r5.large: 2 vCPU, 16 GB RAM - $0.240/hr

DEPLOYMENT OPTIONS:

Single-AZ:
- Single instance in one AZ
- Lowest cost
- Use: Dev/test environments

Multi-AZ:
- Primary + standby in different AZ
- Synchronous replication
- Automatic failover (1-2 minutes)
- Zero data loss
- Use: Production databases

Read Replicas:
- Asynchronous replication
- Up to 5 replicas
- Offload read traffic
- Can be in different region
- Use: Read scaling, analytics

FEATURES:

Automated Backups:
- Daily full backup
- Transaction logs every 5 minutes
- Retention: 0-35 days (default 7)
- Point-in-time recovery
- No performance impact

Manual Snapshots:
- User-initiated backups
- Retained until deleted
- Can copy across regions
- Can share across accounts

Encryption:
- At rest: AES-256 with KMS
- In transit: SSL/TLS
- Must enable at creation
- Cannot change after creation

Enhanced Monitoring:
- Real-time OS metrics
- CPU, memory, disk I/O
- 1-second granularity
- Stored in CloudWatch Logs

Performance Insights:
- Database performance monitoring
- Identify slow queries
- Visualize database load
- 7 days free, pay for longer retention

STORAGE:

General Purpose (gp3):
- 20 GB to 64 TB
- 3,000 IOPS baseline
- Use: Most workloads

Provisioned IOPS (io1):
- 100 GB to 64 TB
- Up to 256,000 IOPS
- Use: I/O intensive workloads

Magnetic (legacy):
- Not recommended for new workloads

PRICING EXAMPLE (MySQL):
db.m5.large, Multi-AZ, 100 GB gp3:
Instance: $0.192/hr × 2 (Multi-AZ) × 730 = $280/month
Storage: 100 GB × $0.23 × 2 = $46/month
Backup: 100 GB × $0.095 = $9.50/month
Total: ~$335/month

KEY LIMITS:
- Database instances per region: 40
- Read replicas per primary: 5
- Manual snapshots: 100 per region
- Parameter groups: 50 per region

EXAM TIPS:
✓ Multi-AZ for high availability
✓ Read replicas for scaling reads
✓ Automated backups for PITR
✓ Encryption cannot be added later
✓ Cross-region replicas for DR
```


### Amazon DynamoDB

```
SERVICE OVERVIEW:
Fully managed NoSQL database
Single-digit millisecond latency
Automatic scaling, replication

CAPACITY MODES:

On-Demand:
- Pay per request
- Auto-scales instantly
- No capacity planning
- Use: Unpredictable traffic
- Cost: $1.25/million writes, $0.25/million reads

Provisioned:
- Specify RCU/WCU
- Auto Scaling optional
- 20% cheaper than on-demand (if predictable)
- Use: Predictable traffic
- Cost: $0.00065/RCU-hour, $0.00013/WCU-hour

Capacity Units:
RCU (Read Capacity Unit): 1 strongly consistent read/sec of 4 KB
WCU (Write Capacity Unit): 1 write/sec of 1 KB

Example:
10 items/sec, 8 KB each, strongly consistent reads:
RCU needed: 10 × (8/4) = 20 RCU

10 items/sec, 3 KB each, writes:
WCU needed: 10 × 3 = 30 WCU

FEATURES:

Global Tables:
- Multi-region, multi-active
- < 1 second replication
- Conflict resolution (last write wins)
- Use: Global applications

DynamoDB Streams:
- Ordered stream of item changes
- 24-hour retention
- Trigger Lambda functions
- Use: Change data capture, analytics

DynamoDB Accelerator (DAX):
- In-memory cache
- Microsecond latency
- Write-through cache
- Fully managed
- Cost: $0.28/hr (dax.r4.large)

Point-in-Time Recovery (PITR):
- Continuous backups
- Restore to any point (35 days)
- Per-second granularity
- Cost: $0.20/GB/month

INDEXES:

Global Secondary Index (GSI):
- Different partition + sort key
- Queries across entire table
- Eventual consistency
- Separate RCU/WCU

Local Secondary Index (LSI):
- Same partition key, different sort key
- Queries within partition
- Strong or eventual consistency
- Shares RCU/WCU with table

PRICING EXAMPLE:
On-demand, 10M writes/month, 50M reads/month, 5 GB:
Writes: 10M × $1.25/M = $12.50
Reads: 50M × $0.25/M = $12.50
Storage: 5 GB × $0.25 = $1.25
Total: ~$26/month

KEY LIMITS:
- Item size: 400 KB max
- Table name: 3-255 characters
- Tables per region: 2,500
- Partition key: 2048 bytes max
- GSI per table: 20

EXAM TIPS:
✓ On-demand for unknown traffic
✓ Provisioned for predictable traffic
✓ GSI for flexible queries
✓ DynamoDB Streams for events
✓ DAX for read-heavy caching
✓ Global Tables for multi-region
```


### Amazon Aurora

```
SERVICE OVERVIEW:
MySQL/PostgreSQL-compatible database
5× faster than MySQL, 3× faster than PostgreSQL
Automatic storage scaling (10 GB to 128 TB)

ARCHITECTURE:

Storage:
- 6 copies across 3 AZs
- Self-healing (bit corruption detected/repaired)
- Continuous backup to S3
- 99.99% availability

Compute:
- Primary instance (read/write)
- Up to 15 read replicas
- Replicas can be promoted
- Auto Scaling for replicas

FEATURES:

Aurora Serverless:
- Auto-scaling database
- Scale to zero when idle
- Pay per second
- Use: Intermittent workloads
- Cost: $0.06/ACU-hour (Aurora Capacity Unit)

Aurora Global Database:
- Cross-region replication
- < 1 second replication lag
- Up to 5 secondary regions
- Fast failover (< 1 minute)
- Use: Global applications, DR

Aurora Multi-Master:
- Multiple write instances
- Continuous availability
- Application-level conflict resolution
- Use: Sharded applications

Backtrack:
- Rewind database to point in time
- No restore required (instant)
- Up to 72 hours
- Use: Undo mistakes

Cloning:
- Fast database copy
- Copy-on-write (efficient)
- Use: Dev/test environments

PRICING:
Instance: Same as RDS (db.t3, db.r5, etc.)
Storage: $0.10/GB/month (auto-scales)
I/O: $0.20/million requests
Backup: $0.021/GB/month (beyond storage size)

Serverless v2:
ACU (Aurora Capacity Unit): $0.12/ACU-hour
Min: 0.5 ACU, Max: 128 ACU
Example: Average 2 ACU × 730 hrs = $175/month

Example (MySQL-compatible):
db.r5.large, 100 GB data, 10M I/O:
Instance: $0.29/hr × 730 = $212/month
Storage: 100 × $0.10 = $10/month
I/O: 10M × $0.20/M = $2/month
Total: ~$224/month

KEY LIMITS:
- Database size: 128 TB
- Read replicas: 15
- Endpoints: 5 custom
- Backtrack window: 72 hours
- Global Database regions: 5

EXAM TIPS:
✓ Aurora for high performance
✓ Serverless for variable workloads
✓ Global Database for multi-region
✓ Read replicas for scaling reads
✓ Automatic storage scaling
✓ Self-healing storage
```


## Networking Services Quick Reference

### Amazon VPC (Virtual Private Cloud)

```
SERVICE OVERVIEW:
Isolated virtual network
Complete control over IP ranges, subnets, routing
Logically isolated from other VPCs

COMPONENTS:

CIDR Blocks:
- Primary: /16 to /28 (e.g., 10.0.0.0/16)
- Secondary: Up to 5 additional
- Cannot overlap with other VPCs if peering

Subnets:
- Subdivision of VPC
- Each in single AZ
- Public: Route to internet gateway
- Private: No route to internet
- 5 IPs reserved per subnet (AWS)

Route Tables:
- Controls traffic routing
- Each subnet associated with one table
- Default: Local route (VPC CIDR)

Internet Gateway:
- Allows internet access
- Horizontally scaled, redundant
- 1:1 NAT for public IPs

NAT Gateway:
- Allows private subnet internet access
- Managed NAT service
- Per AZ deployment
- Cost: $0.045/hr + $0.045/GB processed

NAT Instance:
- EC2 instance as NAT
- Self-managed
- Lower cost but requires management

VPC Peering:
- Connect two VPCs
- Private IP communication
- Same or different regions
- No transitive peering

Transit Gateway:
- Hub for multiple VPCs
- Simplifies network topology
- Inter-region peering
- Cost: $0.05/hr + $0.02/GB

SECURITY:

Security Groups:
- Stateful firewall
- Instance level
- Allow rules only
- Default: Deny all inbound, allow all outbound

Network ACLs:
- Stateless firewall
- Subnet level
- Allow and deny rules
- Default: Allow all

VPC Flow Logs:
- Capture IP traffic
- CloudWatch Logs or S3
- Analyze traffic patterns
- Cost: Standard CloudWatch Logs pricing

CONNECTIVITY:

VPN:
- Site-to-Site VPN: On-premises to VPC
- Client VPN: Remote users to VPC
- Cost: $0.05/hr + $0.09/GB transferred

Direct Connect:
- Dedicated connection
- 1 Gbps or 10 Gbps
- Consistent bandwidth
- Cost: Port hour fee + data transfer

PrivateLink:
- Private access to services
- No internet gateway needed
- Powered by NLB
- Cost: $0.01/hr + $0.01/GB processed

PRICING:
VPC: Free
NAT Gateway: $0.045/hr + $0.045/GB
VPN: $0.05/hr + data transfer
VPC Peering: Data transfer only
Transit Gateway: $0.05/hr + $0.02/GB

KEY LIMITS:
- VPCs per region: 5 (soft limit)
- Subnets per VPC: 200
- Route tables per VPC: 200
- Security groups per VPC: 2,500
- Rules per security group: 60
- VPC peering connections: 125

EXAM TIPS:
✓ Public subnet has IGW route
✓ Private subnet uses NAT Gateway
✓ Security Groups: Stateful
✓ NACLs: Stateless
✓ VPC Peering: No transitive routing
✓ Transit Gateway: Hub-and-spoke
```

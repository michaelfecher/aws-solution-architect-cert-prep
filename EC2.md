# EC2
elastic computing cloud is a web service that provides resizeable compute capacity.
pay per use for capacity

## EC2 options:
on demand:
- fixed rate by hour or by second with no commitment.
- low costs and flexibility without upfront payment or long term commitment
- applications with short-term, spiky and unpredictable workloads that can not be interrupted
- applications being developed or tested for the first time

reserved instances (RIs): hourly change by instance; contract for capacity reservation for 1 or 3 years
- applications with steady state with predictable usage
- applications that reserved capacity:
- upfront payments to reduce total computing costs:
-- standard reserved instances (up to 75% off on-demand costs)
-- convertiable RIs (up to 54% off on-demand costs) 
-- scheduled RIs for launching within a time window

spot: bidding for instance capacity (pay for what you bid on)
- applications that have flexibile start and end times
- applications that are only feasible at very low computing prices
- users with an urgent need for large amounts of additional computing capacity
- if EC2 terminates the spot instance, then a charge of the partial hour is not charged... 
- if the spot instance is user terminated, then the partial hour is charged

dedicated hosts: physical dedicated ec2 servers for usage. bring existing licenses to reduce costs
- regulatory requirements that may not support multi-tenant virtualization
- great for licensing
- can be purchased on demand (hourly)
- can be purchased as a reservation for up to 70% off the on-demand price

## EC2 instance types:
Letters, not the numbers are relevant

Memorizing Edward norton (from Fightclub) FIGTH(s) against Dr. McP(i)x  
F FPGA  
I IOPS  
G Graphics  
H High Disk Throughput  
T cheap disk general purpose (like t2.micro)  
D density  
R RAM  
M main choice for general purpose  
C compute  
P graphics  
X extreme memory  

## EBS (Elastic Block Store)
Storage volume, can be attached to EC2 instances  
When attached, then FS on top of those can be created.  
Block Device.  
Placed in a specified AZ with replication.  

## EBS Types
GP2 SSD = General Purposed, max. 10k IOPS, balanced price and performance  
IO1 SSD = Intesive Applications as large relational or NoSQL DBs, more than 10k IOPS  
ST1 HDD = Big Data, DWH, Log Processing.. can not be a boot volume; frequently accessed, throughput intensive  
SC1 HDD = Lowest Cost Storage, File Server.. infrequently access, e.g. File Server, can not be a boot volume  
Magnetic HDD = Previous Generation, can be a boot volume  

Termination protection is turned off by default.  
On an EBS backed instance, the default action is for the root EBS to be deleted after instance is terminated.  
EBS Root volume cannot be encrypted by default, only via BitLocker or an own AMI with an encrypted root EBS volume.  
Additional volumes can be encrypted.  

## EBS Extended
Volume exist on EBS (Virtual Hard Disk).  
Volume and EC2 instance need to be in the same AZ (obviously).
It's also possible to migrate or copy the EC2 instance directly to another AZ.  
EBS volume size change on the fly, including changing size and storage type.

### Snapshots
Snapshots are point in time copies of columes and are incremental.
Snapshots exist on S3.  
Stop the EC2 instance before taking a snapshot, but it works also with a running instance.  
Amazaon Machine Images (AMI) can be created from EBS-backed instances and snapshots.

Moving a volume from one AZ/Region to another:
- create a snapshot of the volume
- copy the snapshot to the new AZ/region
- create a new image (AMI) from the snapshot

### Volume vs Snapshots - Security
Snapshots of encrypted volumes are encrypted automatically.  
Volumes restored from encrypted snapshots are encrypted automatically.  
Share snapshots, but only if they are unencrypted.  
Unencrypted snaps can be shared with other AWS accounts or made public.

## RAID, Volumes & Snapshots
Multiple Volumes can form a (Software) RAID.  
RAID = Redundant Array of Independent Disks
- RAID 0: Striped, no Redundancy, Good Performance (e.g. Gaming PC)
- RAID 1: Mirrored, Redundancy 
- RAID 5: Good for reads, bad for writes, AWS doesn't recommend this
- RAID 10: Striped & Mirrored, Good Redundancy, Good Performance

Usudally Raid0 or Raid10 is used..  

It's also possible to create snapshots of a RAID.

## EBS vs Instance Store
All AMIs are categorized as either backed by Amazon EBS or backed by instance store.  
Both are volumes.  
### EBS volume
The root device of an AMI is a EBS volume from an EBS snapshot.
Faster provision time than Instance Stores.
Newer than Instance Store Volumes.
EBS backed instances can be stopped and you will not lose data if stopped.
On Termination, you got the option to keep the root device volume (not by default).

### Instance Store volume
root device of an AMI is an instance store volume from a *template* stored in S3.  
Also called Ephemeral Storage.
Instance Stores can not be stopped. If the underlying host fails, you'll lose the data.
No option to keep the root device volume on instance termination.

## Security Groups
All Inbound traffic is blocked by default.  
All Outbound traffic is allowed. It's also is implicitly set if an inbound rule is applied (Stateful).
ACLs on the other side are stateless, means explicit setting for the outbound rule.  
Changes to Security Groups are applied immediately.  
Security Groups can contain 0-n EC2 instances.  
An EC2 instance can have multiple Security Groups.  
IP address blocking can be done with Network Access Control Lists, NOT with Security Groups.  
You can specifiy Allow rules, but not Deny rules. Denial is done via removing the "Allowing".

## Elastic Load Balancers (ELB)
3 Types. 
Load Balancing error can be 504 (Gateway timeout), which is possibly an application error after the LB or with the infrastructure.
Instances monitored by ELB are reported in the following state:
- InService
- OutOfService

Health Checks are used to check the instance state by pinging.
All ELB have an own DNS name and NO public IP.

### Application Load Balancers
best suited for HTTP/HTTPS balancing traffic.  
Operate at layer 7 and therefore application-aware.    
Intelligent via advanced request routing.  

### Network Load Balancers
suited for TCP traffic achieving extreme performance (layer 4).  
Handling millions of requests per second while maintaining ultra-low latency.

### Classic Load Balancers
Legacy.  
Amazon recommends to use Application Load Balancer.
Layer-7 and Layer-4 balancing possible.  

### X-Forwarded-For Header
passes the public IP address to the EC2 address of the client over the Load Balancer via HTTP.

## CloudWatch
for performance monitoring AWS resources.  
Dashboards  
Alarms  
Events - respond to state changes of the AWS resources  
Logs - aggregate, store and monitor logs by installing an agent on the EC2 server
CloudTrail is for auditing.. monitor the whole AWS environment

## AWS CLI
If using AWS CLI in an EC2 instance directly and not on a local computer, assign a role with the needed policies to the EC2.  
This avoids storing credentials on the EC2 directly.

When accessing a S3 bucket, then make use of the `--region` argument and provide the region of the requested S3 bucket.
If not provided, then the EC2 instance maybe can not access the bucket, because it's in a different AZ/region at the moment of time.
So without the argument it's not predictable if it's working.

## EC2 Metadata
Extracted from [UserGuide EC2 MetaData](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html).  
To view all categories of instance metadata from within a running instance, use the following URI: 
```
curl http://169.254.169.254/latest/meta-data/
```
With the help of this, information about the EC2 instance can be extracted and proceeded further, e.g. storing in a S3 bucket, from the bucket to a lambda...

# Launch Configurations & Auto Scaling
Launch configuration is basically the provisioning (selection of AMI, type).  
It's needed for the Auto Scaling.

In the Auto Scaling, we can define Launch configuration, the network and the subnet. So the subnet is the key aspect here, because
it reflects the AZ/regions. 
So when having auto scaling wanted, use as much subnets as possible. Otherwise it's pointless.

Scaling policies are basically threshold based scaling options, like CPU spikes over 90% for X minutes.
There are two options:
- Decreasing policy: This describes the condition to be true in order to start/add new instances
- Increasing policy: This describes the condition in order to shutdown/remove instances

After setting the Auto scaling and the policies, the EC2 instances start up automatically.
When deleting the Auto scaling setting, the EC2 instances will be removed automatically.


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

## EBS
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

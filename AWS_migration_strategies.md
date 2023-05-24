# AWS migration strategies

A migration strategy is the approach used to migrate a workload into the AWS Cloud.  
There are seven migration strategies for moving applications to the cloud, known as the 7 Rs.

<br>

## 7 Rs:
* Retire
* Retain
* Rehost
* Relocate
* Repurchase
* Replatform
* Refactor or re-architect

Common strategies for large migrations include rehost, replatform, relocate, and retire.  
Refactor is not recommended for large migrations because it involves modernizing the application during the migration.

<br>

##  Important information
* Zombie applications: average CPU and memory usage below 5 %
* Idle applications: average CPU and memory usage between 5 and 20 % over a period of 90 days
* Cloud optimization can save time and money

<br>

## Retire
* For decommissioning or archiving applications
* Use cases:
  - no business value in retaining or moving to cloud
  - eliminate costs
  - reduce security risks
  - zombie or idle applications (low performance)
  - no inbound connection for the last 90 days

<br>

## Retain
* For keeping in source environment, not ready to migrate
* Use cases:
  - remain in compliance with data residency requirements
  - high risk
  - dependencies / migrate other apps first
  - recent upgrade --> postpone migration
  - no business value
  - plans to migrate to (and having to wait for) SaaS (vendor-based apps)
  - unresolved physical dependencies (e.g. hardware that does not have a cloud equivalent)
  - mainframe, mid-range or non-x86 Unix apps
  - performance (e.g. keep zombie or idle apps)

<br>

## Rehost
* Lift and shift  
  --> move apps from source environment/on-premise to AWS cloud without changes
* Possible to migrate a large number of machines from multiple source platforms (physical, virtual, cloud)
* Helps to scale apps
* Apps are easier to optimize or re-architect
* Automate rehosting with:
  - AWS Application Migration Service
  - AWS Cloud Migration Factory Solution
  - VM Import/Export

<br>

## Relocate
* 

<br>

## Refactor
* The most complex of the migration strategies  
  It can be complicated to manage for a large number of applications  
  --> Not recommended for large migrations because it involves modernizing the application during the migration
  
  
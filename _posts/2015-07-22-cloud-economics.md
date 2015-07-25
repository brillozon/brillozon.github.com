---
layout: post
title: "Cloud Economics"
description: "Simple Cloud Technology Selection Criteria"
category: Cloud Computing
tags: [cloud]
---
{% include JB/setup %}

This post reviews the criteria for selecting a cloud computing technology
and shows a couple of examples.  Whenever I try to explain the choices
involved, I usually manage to forget one or two.  So this is an attempt
to capture the entirety so that I have a better chance of remembering.
Or I can give this link out.  :)

The bottom line is that outsourcing computing and storage to the cloud has
benefits.  Storage is very expensive, but those costs allow the servers
to be used efficiently.  Startups along with application development
and testing are likely to derive the greatest benefit from moving to an
external cloud.  Cloud computing is also beneficial if the application
capacity planning has enough overhead included to push the private data
center costs over what the monthly cloud costs are.

### Using a Cloud

There are at least two reasons a company would want to use cloud
computing.  The first case &ndash; what is usually assumed &ndash; is
that the cloud will be used to provide a production service.  The other
case, which is also an efficient use of cloud computing, is to use the
cloud resources for application development.

When using cloud computing resources to provide production services, an
additional benefit is that applications can elastically add more resources
as load increases, and release these resources once load eases.  In cases
where the load changes slowly or in a known time varying pattern this can
be a considerable benefit.  It allows the production services to be
provided without providing peak capacities in a dedicated server
facility.

When developing applications using cloud resources, they tend to be large
data intensive applications that scale out (horizontally) using cloud
resources only when needed.  Developing these applications in a cloud
allows the development, configuration, and deployment to be performed
and tested prior to committing resources (cash!) for the infrastructure
required to field these applications.

### A Motivating Example

Instead of deriving a large complex calculation to determine costs, I
want to make a couple of (ok, a few.  Would you believe just slightly more
than a few?) simplifying assumptions so we can get a feel for what the
magnitudes are and what the trade offs might be.

So, with that in mind, lets look at *c9.inc*, a fictional new startup
that intends to field a service that will require large processing and
storage capacities.  Lets start with the processing capacity.  Lets make
the assumption that the application architects have accurately predicted
the required resources and that a data center with 1,024 cores will
provide the anticipated needs for the near term.  Since we know this is
a data intensive application assume that the storage needs will be for
1,000 TB of data.

### Example Costs

In the following, I will make some assumptions and derive a cost for both
a cloud based and a private facility based solution that can provide the
capacity required by the example.

Since we have not identified the type of application &ndash; public
facing or internal &ndash; we really have no basis to include the data
transfer costs.  If this were a dedicated internal application that was
not intended to face the public internet, then no costs would be incurred
for data transfer charges beyond the existing internal infrastructure.
If this is a public facing application, then the data transfer charges
are very dependent on the details of the application transfers and
the external network connections and costs.  This is beyond what I am
attempting to summarize here, so I will conveniently ignore these costs.

Related to the networking costs that are being ignored are the data
transfer costs.  Since data transfers in clouds are usually only costly
when entering or leaving the cloud, these costs can be lumped in with the
network usage costs.

##### Cost of Cloud:

Using the <a href="http://calculator.s3.amazonaws.com/index.html">Simple
Monthly Calculator</a> we can determine what the costs for outsourcing
the data center to AWS would be.

For the servers, I chose the c3.8xlarge instances which have 32 cores and
60 GB RAM available.  They also have 2 320 GB SSD (ephemeral) disks
attached for scratch storage.  The on-demand hourly cost is $1.68 while
the reserved cost is only $0.628, a 63% savings.  We will assume that the
crack architects at *c9.inc* planned well and we are able to reserve the
needed instances.  The 32 cores per instance means that we will need to
provision 32 of these instances.

Lets attach 512 GB persistent (EBS) storage to each server to allow
application data and software to survive resets.  We can do this using
general purpose SSD disks.  We will use the S3 storage facility for the
required 1000 TB of storage.

The results include a cost of $41K/month for servers, $30K/month for
storage, and an additional $5K/month for business support.

**This is a total of $76K/month for outsourcing the data center.**

##### Cost of Ownership

The article
<a href="http://perspectives.mvdirona.com/2010/09/overall-data-center-costs/">
Overall Data Center Costs</a> provides a more detailed investigation of
costs, and includes links to even more detailed analysis of private data
center costs.  Using the pie chart in that article, and making
adjustments to simplify my calculations &ndash; since I sometimes attempt to do
these in my head &ndash; I end up with the following ratios:

* 60% - servers (includes storage)
* 30% - infrastructure (facilities, power, cooling, wiring, etc)
* 10% - networking equipment

From this and other articles, I will make the assumption that the
servers, storage, and networking equipment will need to be replaced every
3 years.  So costs per month can be derived by dividing total cost by 36.

Looking at recent disk costs, I will assume that we can purchase 4 TB
drives for $150, or $37.50 / TB.  If we configure the disks using RAID5
in groups of 5 disks, then we have a storage efficiency of 80%, resulting
in a need for 1.25 x 1000 TB, or 1250 TB.  This will cost a total of $47K.

Thus the storage cost for owning the data center will be $1,300/month.

The server costs will be based on obtaining the
<a href="http://www8.hp.com/us/en/products/proliant-servers/product-detail.html?oid=5409789&jumpid=reg_r1002_usen_c-001_title_r0002#!tab=specs">
HP ProLiant DL360P</a> 1U servers with 32 GB RAM.  Populating the two
sockets in the servers with 8 core Intel Xeon CPUs each will allow
64 cores per server.  This will cost about $8,400 per server, or $525
per core.

From the article
<a src="http://www.datacenterknowledge.com/archives/2008/05/30/failure-rates-in-google-data-centers/">
Failure Rates in Google Data Centers</a>, which is a bit old now, I can
see that they are seeing about a 5.5% failure rate per year.  This means
we can expect one server to fail about every 20 days.  This should allow
us to provide a very small overhead amount of servers to allow for the
downtime until a failed server is replaced.  Since I did not account for
the other types of failures mentioned, or analyze the actual MTTF or MTTR
numbers, I will simply guess that 5% overhead will be enough.

Wow.  That&apos;s not much.  It will assume that we have very good
virtualization layer and task management to allow the application to be
resilient through these failures.  Since we live in the era of big data,
this is the case.  :)  And actually, the application itself will have
been sized to allow for redundancy and resilience if it was developed for
a possible cloud environment.  If the application is based on a Hadoop
cluster with built in redundancy for storage and job management, this is
indeed the case.

This minimal capacity overhead will result in a need for 1,075 cores.
This will require 68 of the 1U servers.  We will assume that *c9.inc* will
not want to do any exceptional facility provisioning for power or cooling,
so will populate 3 racks with 23 servers each, for a total of 69 servers.
The total cost for servers will then be $579,600 to provide 1,104 cores.

Thus the server cost for owning the data center will be $31K/month.  The
total server and storage comes to about $32K/month.

Using this cost to estimate the infrastructure and networking costs
results in the following:

* $32K / month - servers and storage
* $16K / month - infrastructure
*  $5K / month - network equipment

In addition to the infrastructure we will need operational support.
Using an estimate of one system administrator per 100 servers, we will
only need 1 admin for this data center.  I will estimate that cost as
approximately $8K/month.

**The resulting total cost for owning a data center is then $61K/month.**

Note that this is approximately 80% of the cost to provision the data
center in the cloud.

### Breakeven

So.  If the cost for provisioning a private data center is less than
provisioniing those resources in a cloud, why would we use cloud
resources.

Such a good question!  :)

Lets find the break even point for building out the private data center.
This is the time when the total cost of cloud operation reaches the cost
of building the data center.  The idea is that when building a private
data center, the costs are up front and must be paid at the start of
operations, while a cloud cost accrues each month.

For storage, the cloud monthly costs are $30K and $47K is the total cost
for the data center.

The storage break even is reached at $47K / $30K = 1.5 months.

For servers, cloud monthly costs are $41K and $1,116K total cost for the
data center.

The server break even is reached at $1,116K / $41K = 27 months.

Combining the two, cloud monthly costs are $76K and $2,200K is the total cost
for the private data center.

**The combined break even is reached at $2,200K / $76K = 29 months.**

### What?

From the numbers, we can see that a startup like *c9.inc* will benefit
for the first two years of operation by provisioning their application in
the cloud.  If the anticipated lifetime of the application &ndash; or
company! &ndash; will surpass this time frame, then it makes economic
sense to create a private data center.  Caution should be used since this
involves incurring costs up front which will expose the entire investment
to risk of failure as well as the time value of the initial investment
(opportunity cost).

Another benefit of the cloud computing environment is where new
applications are being developed and tested.  These by their nature are
small and short in duration, which clearly benefits from outsourcing the
resources to the cloud rather than provisioning them for each project.

Some things to note from the above derivation include that storage in the
cloud is expensive.  If it were not for the data transfer costs, the use
of cloud servers with customer disks would be a good architecture.  But
with costs structured as they are today, the storage costs are part of
the overall cloud costs since that avoids the excessive IOPS (data
transfer) charges.

Also note that the data center ownership costs were based on an overhead
margin of only 5% for the servers.  If the application capacity planning
includes more margin than this, the relative costs of ownership versus
the cloud increases, pushing out the lifetime required to break even by
owning the data center.  The cost derivations above also do not include
any financing costs or opportunity costs, which can be significant for
any single project.

Most of these cost increases for ownership can be mitigated by sharing
the data center for additional applications and projects.


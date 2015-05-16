---
layout: post
title: "Making AWS metadata available on EC2"
description: "Extract EC2 tags for use in startup processing"
category: devops
tags: [cloud,aws,ec2,metadata,cloudformation]
---
{% include JB/setup %}

This post reviews mechanisms for obtaining metadata from AWS for an
EC2 instance that is starting.  The instance is assumed to be starting in
response to a CloudFormation event, so there is metadata associated with
the CloudFormation stack as well as the EC2 instance that we retrieve.

## Motivation

During EC2 instance startup, it would be convenient to have information
about the context in which an instance is operating so that it can
configure itself for operation correctly.  This information is available
from external sources and can be obtained using different mechanisms for
different pieces of data.  There can&apos;t be only one.

## Tools

The good news is that AWS comes with the tools to obtain the metadata.  The less
salubrious news is that there is no simple step to grab the metadata and
make it available to the instance.  So, situation normal.  The tools that
we will be using to grab the metadata include:

* the EC2 reflection interfaces at 169.254.169.254
* the AWS CLI tools
* the CloudFormation helper <code>cfn-get-metadata</code>

This post will be addressing the mechanisms used in this
[Gist](https://gist.github.com/brillozon/188fde35d270382851c6) which extracts the
metadata available for an EC2 instance and its stack.

## Using the EC2 reflection interfaces

Amazon Web Services (AWS) Elastic Compute Cloud (EC2) instances are virtual
machines that are booted within the AWS cloud and available to the
account that started the instance.  As part of the startup, information
about the running instance is made available to the instance through a
special endpoint: <code>http://169.254.169.254</code> that provides
simple GET access to the values.

The available information is described in the [AWS
documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html#instancedata-data-categories)

This information is available to anyone making an HTTP GET request
from the host itself.  This information is not visible outside the
instance.  Some of this information is available through other mechanisms
as well, an suitable security considerations should be observed.  Such as
not including sensitive content in User Data, for example.

## Using the AWS CLI tools

The AWS CLI tools are available on the Ubuntu systems by installing the
Python '<em>awscli</em>' package (<code>pip awscli</code>).  These tools can be
executed from either the EC2 host or any external host as well.  This
means that authentication and authorization are required to successfully
use these tools.

Authentication is provided by any of several different mechanisms.  If
the tool is invoked from an EC2 instance that has been imbued with an IAM
role, then that role provides the authorization for accessing information
via the command.  Otherwise, the <code>AWS Access Key ID</code> and
<code>AWS Secret Access</code>
Key values (created with the IAM user account), along with the AWS region
information should be used.  These values can be placed in the AWS config
file (<em>~/.aws/config</em>) or can be used directly on the command
line.  Use on the command line is discouraged since those values are
visible to anyone logged on.

## Using the CloudFormation helper tools

The Cloud Formation helper script <code>cfn-get-metadata</code> is used
to gather the stack metadata.  This helper script is installed using one
of the mechanisms described in the [AWS help](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-helper-scripts-reference.html)
pages.  When creating a stack with Cloud Formation, this is usually done
with the User Data script ensuring this occurs before the instance is
made available.

# Anatomy of the code

The Gist code here is described below.

{% include JB/gist gist_id="188fde35d270382851c6" %}

## What needs to be loaded first

The network interface needs to be present and running for the reflection
interface to be used.

The AWS CLI tools need to be installed and authorization provided &ndash;
either using the Access ID and Secred, or an IAM role &ndash; for these
tools to be used.  They require that the python package manager pip is
installed as well.

The Cloud Formation scripts need to be installed in order to use them to
access metadata about the AWS resources.

## Where the results go

Lines 1 through 9 of the Gist locate and create a working directory and
name output files to hold the information gathered about the instance and
the stack.  This is simple bash script code.

## Getting metadata for the current instance

Data about the current insance is gathered using the AWS CLI tool.  In
order to access this data, we need to know the AWS InstanceId.  We can
obtain the instance Id using the reflection interface.

Lines 10 through 14 show how we determine the instance Id value.  First
we setup the use of <code>curl</code> for accessing the data URL, and
then make a GET call to the <em>/latest/meta-data/instance-id</em>
location. We store the returned value both locally in a shell variable as
well as writing it to a file named <em>instanceid</em>.

Lines 16 through 19 show a call to the AWS CLI to <code>aws ec2
describe-instances</code> which returns JSON formatted data for all of
the available instances.  The <code>jq</code> command is then used to
select only the portion of the data that describes the current instance
and stores that into the <em>instancedata.json</em> file.

## Extracting values from the metadata

Lines 21 through 24 show how the Availability Zone, Stack Id, and Logical
Id values are taken from the instance data we just obtained.  This is all
done using queries of the <code>jq</code> command on the instance data.

Lines 26 through 30 show how we extract the region from the availability
zone.  This assumes that the region is simply the zone with the last
character truncated.

## Getting metadata for the CloudFormation stack

Once we have obtained the Stack Id, Logical Id, and Region from the
instance data, then we can use those values to obtain the stack data
using the CloudFormation script <code>cfn-get-metadata</code> script.
This is shown on Lines 43 and 44.

Note that we need to strip the double quotes from the values we obtained
from the instance data.  We do that here by simply executing
<code>eval echo $value</code>.


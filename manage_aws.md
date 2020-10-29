# Managing AWS
------------

### Managing Infrastructure State and Change

A great challenge in using AWS to build complex systems (and with DevOps in general) is to manage infrastructure state effectively over time. In general, this boils down to three broad goals for the state of your infrastructure:

-	**Visibility**: Do you know the state of your infrastructure (what services you are using, and exactly how)? Do you also know when you — and anyone on your team — make changes? Can you detect misconfigurations, problems, and incidents with your service?
-	**Automation**: Can you reconfigure your infrastructure to reproduce past configurations or scale up existing ones without a lot of extra manual work, or requiring knowledge that’s only in someone’s head? Can you respond to incidents easily or automatically?
-	**Flexibility**: Can you improve your configurations and scale up in new ways without significant effort? Can you add more complexity using the same tools? Do you share, review, and improve your configurations within your team?

Much of what we discuss below is really about how to improve the answers to these questions.

There are several approaches to deploying infrastructure with AWS, from the console to complex automation tools, to third-party services, all of which attempt to help achieve visibility, automation, and flexibility.

### AWS Configuration Management

The first way most people experiment with AWS is via its web interface, the AWS Console. But using the Console is a highly manual process, and often works against automation or flexibility.

So if you’re not going to manage your AWS configurations manually, what should you do? Sadly, there are no simple, universal answers — each approach has pros and cons, and the approaches taken by different companies vary widely, and include directly using APIs (and building tooling on top yourself), using command-line tools, and using third-party tools and services.

### AWS Console

-	The [AWS Console](https://aws.amazon.com/console/) lets you control much (but not all) functionality of AWS via a web interface.
-	Ideally, you should only use the AWS Console in a few specific situations:
	-	It’s great for read-only usage. If you’re trying to understand the state of your system, logging in and browsing it is very helpful.
	-	It is also reasonably workable for very small systems and teams (for example, one engineer setting up one server that doesn’t change often).
	-	It can be useful for operations you’re only going to do rarely, like less than once a month (for example, a one-time VPC setup you probably won’t revisit for a year). In this case using the console can be the simplest approach.
-	❗**Think before you use the console:** The AWS Console is convenient, but also the enemy of automation, reproducibility, and team communication. If you’re likely to be making the same change multiple times, avoid the console. Favor some sort of automation, or at least have a path toward automation, as discussed next. Not only does using the console preclude automation, which wastes time later, but it prevents documentation, clarity, and standardization around processes for yourself and your team.

### Command-Line tools

-	The [**aws command-line interface**](https://aws.amazon.com/cli/) (CLI), used via the **aws** command, is the most basic way to save and automate AWS operations.
-	Don’t underestimate its power. It also has the advantage of being well-maintained — it covers a large proportion of all AWS services, and is up to date.
-	In general, whenever you can, prefer the command line to the AWS Console for performing operations.
-	🔹Even in the absence of fancier tools, you can **write simple Bash scripts** that invoke *aws* with specific arguments, and check these into Git. This is a primitive but effective way to document operations you’ve performed. It improves automation, allows code review and sharing on a team, and gives others a starting point for future work.
-	🔹For use that is primarily interactive (not scripted), consider instead using the [**aws-shell**](https://github.com/awslabs/aws-shell) tool from AWS. It is easier to use, with auto-completion and a colorful UI, but still works on the command line. If you’re using [SAWS](https://github.com/donnemartin/saws), a previous version of the program, [you should migrate to aws-shell](https://github.com/donnemartin/saws/issues/68#issuecomment-240067034).

### APIs and SDKs

-	**SDKs** for using AWS APIs are available in most major languages, with [Go](https://github.com/aws/aws-sdk-go), [iOS](https://github.com/aws/aws-sdk-ios), [Java](https://github.com/aws/aws-sdk-java), [JavaScript](https://github.com/aws/aws-sdk-js), [Python](https://github.com/boto/boto3), [Ruby](https://github.com/aws/aws-sdk-ruby), and [PHP](https://github.com/aws/aws-sdk-php) being most heavily used. AWS maintains [a short list](https://aws.amazon.com/tools/#sdk), but the [awesome-aws list](https://github.com/donnemartin/awesome-aws#sdks-and-samples) is the most comprehensive and current. Note [support for C++](https://github.com/donnemartin/awesome-aws#c-sdk) is [still new](https://aws.amazon.com/blogs/aws/introducing-the-aws-sdk-for-c/).
-	**Retry logic:** An important aspect to consider whenever using SDKs is error handling; under heavy use, a wide variety of failures, from programming errors to throttling to AWS-related outages or failures, can be expected to occur. SDKs typically implement [**exponential backoff**](https://docs.aws.amazon.com/general/latest/gr/api-retries.html) to address this, but this may need to be understood and adjusted over time for some applications. For example, it is often helpful to alert on some error codes and not on others.
-	❗Don’t use APIs directly. Although AWS documentation includes lots of API details, it’s better to use the SDKs for your preferred language to access APIs. SDKs are more mature, robust, and well-maintained than something you’d write yourself.

### Boto

-	A good way to automate operations in a custom way is [**Boto3**](https://github.com/boto/boto3), also known as the [Amazon SDK for Python](http://aws.amazon.com/sdk-for-python/). [**Boto2**](https://github.com/boto/boto), the previous version of this library, has been in wide use for years, but now there is a newer version with official support from Amazon, so prefer Boto3 for new projects.
-	Boto3 contains a variety of APIs that operate at either a high level or a low level, here some explanation of both:
	-	The low level APIs (Client APIs) are mapped to AWS Cloud service-specific APIs, and all service operations are supported by clients. Clients are generated from a JSON service definition file.
	-	The high level option, Resource APIs, allows you to avoid calling the network at the low level and instead provide an object-oriented way to interact with AWS Cloud services.
-	Boto3 has a lot of helpful [**features**](https://boto3.readthedocs.io/en/latest/guide/index.html#general-feature-guides) like *waiters*, which provide a structure that allows for code to wait for changes to occur in the cloud, for example, when you are creating an EC2 instance and need wait until the instance is running in order to perform another task.
-	If you find yourself writing a Bash script with more than one or two CLI commands, you’re probably doing it wrong. Stop, and consider writing a Boto script instead. This has the advantages that you can:
	-	Check return codes easily so success of each step depends on success of past steps.
	-	Grab interesting bits of data from responses, like instance ids or DNS names.
	-	Add useful environment information (for example, tag your instances with git revisions, or inject the latest build identifier into your initialization script).

### General Visibility

-	🔹[**Tagging resources**](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) is an essential practice, especially as organizations grow, to better understand your resource usage. For example, through automation or convention, you can add tags:
	-	For the org or developer that “owns” that resource
	-	For the product that resource supports
	-	To label lifecycles, such as temporary resources or one that should be deprovisioned in the future
	-	To distinguish production-critical infrastructure (e.g. serving systems vs backend pipelines)
	-	To distinguish resources with special security or compliance requirements
	-	To (once enabled) [allocate cost](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html). Note that cost allocation tags only apply on a forward-looking basis; you can't retroactively apply them to items already billed.
	-	For many years, there was a notorious 10 tag limit per resource, which could not be raised and caused many companies significant pain. As of 2016, this was [raised](https://aws.amazon.com/blogs/security/now-organize-your-aws-resources-by-using-up-to-50-tags-per-resource/) to 50 tags per resource.
	-	🔹In 2017, AWS introduced the ability to [enforce tagging](https://aws.amazon.com/blogs/aws/new-tag-ec2-instances-ebs-volumes-on-creation/) on instance and volume creation, deprecating portions of third party tools such as [Cloud Custodian](https://github.com/capitalone/cloud-custodian).
	-	🔸 Tags are case sensitive; 'environment' and 'Environment' are two different tags. Automation in setting tags is likely the only sensible option at significant scale.
	-	🔸 There is a bug in the ASG console where spaces after tag names are preserved. So if you type "Name " with a space at the end you will not get the expected behavior. This is probably true in other locations and SDKs also. Be sure you do not add trailing spaces to tag keys unless you really mean it. (As of Jul 2018)

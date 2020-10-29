# AWS Open Guide - General

## What is AWS ? 

----

[AWS](https://en.wikipedia.org/wiki/Amazon_Web_Services) is the dominant public cloud computing provider.

- In general, “[cloud computing](https://en.wikipedia.org/wiki/Cloud_computing)” can refer to one of three types of cloud: “public,” “private,” and “hybrid.” AWS is a public cloud provider, since anyone can use it. Private clouds are within a single (usually large) organization. Many companies use a hybrid of private and public clouds.
- The core features of AWS are [infrastructure-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Infrastructure_as_a_service_.28IaaS.29) (IaaS) 
 that is, virtual machines and supporting infrastructure. 
 Other cloud service models include [platform-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Platform_as_a_service_.28PaaS.29) (PaaS), 
 which typically are more fully managed services that deploy customers’ applications, or [software-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Software_as_a_service_.28SaaS.29) (SaaS), which are cloud-based applications. 
 AWS does offer a few products that fit into these other models, too.
- In business terms, with infrastructure-as-a-service you have a variable cost model — 
it is [OpEx, not CapEx](http://www.investopedia.com/ask/answers/020915/what-difference-between-capex-and-opex.asp) (though some [pre-purchased contracts](https://aws.amazon.com/ec2/purchasing-options/reserved-instances/) are still CapEx).
	
##	**Main reasons to use AWS:**

-	If your company is building systems or products that may need to scale
-	and you have technical know-how
-	and you want the most flexible tools
-	and you’re not significantly tied into different infrastructure already
-	and you don’t have internal, regulatory, or compliance reasons you can’t use a public cloud-based solution
-	and you’re not on a Microsoft-first tech stack
-	and you don’t have a specific reason to use Google Cloud
-	and you can afford, manage, or negotiate its somewhat higher costs
-	... then AWS is likely a good option for your company.
-	Each of those reasons above might point to situations where other services are preferable. 
In practice, many, if not most, tech startups as well as a number of modern large companies can or already do benefit from using AWS. 
Many large enterprises are partly migrating internal infrastructure to Azure, Google Cloud, and AWS.

## What is **EC2 vs. other services:** Most users of AWS are most familiar with [EC2](#ec2)

---- 

AWS’ flagship virtual server product, and possibly a few others like S3 and CLBs. 
But AWS products now extend far beyond basic IaaS, and often companies do not properly understand or appreciate all the many AWS services and how they can be applied,
due to the [sharply growing](#which-services-to-use) number of services, their novelty and complexity, branding confusion, and fear of ⛓lock-in to proprietary AWS technology. 
Although a bit daunting, it’s important for technical decision-makers in companies to understand the breadth of the AWS services and make informed decisions. 
(We hope this guide will help.)

## what is big difference between 🚪**AWS vs. other cloud providers:** 

----

While AWS is the dominant IaaS provider (31% market share in [this 2016 estimate](https://www.srgresearch.com/articles/aws-remains-dominant-despite-microsoft-and-google-growth-surges)), there is significant competition and alternatives that are better suited to some companies. [This Gartner report](https://www.gartner.com/doc/reprints?id=1-2G2O5FC&ct=150519&st=sb) has a good overview of the major cloud players :
-	[**Google Cloud Platform**](https://cloud.google.com/). GCP arrived later to market than AWS, but has vast resources and is now used widely by many companies, including a few large ones. It is gaining market share. Not all AWS services have similar or analogous services in GCP. And vice versa: In particular, GCP offers some more advanced machine learning-based services like the [Vision](https://cloud.google.com/vision/), [Speech](https://cloud.google.com/speech/), and [Natural Language](https://cloud.google.com/natural-language/) APIs. It’s not common to switch once you’re up and running, but it does happen: [Spotify migrated](http://www.wsj.com/articles/google-cloud-lures-amazon-web-services-customer-spotify-1456270951) from AWS to Google Cloud. There is more discussion [on Quora](https://www.quora.com/What-are-the-reasons-to-choose-AWS-over-Google-Cloud-or-vice-versa-for-a-high-traffic-web-application) about relative benefits. Of particular note is that VPCs in GCP are [global by default](https://cloud.google.com/vpc/) with subnetworks per region, while AWS’ VPCs have to live within a particular region. This gives GCP an edge if you’re designing applications with geo-replication from the beginning. It’s also possible to [share one GCP VPC](https://cloud.google.com/compute/docs/shared-vpc/) between multiple projects (roughly analogous to AWS accounts), while in AWS you’d have to peer them. It’s also possible to [peer GCP VPCs](https://cloud.google.com/compute/docs/vpc/vpc-peering) in a similar manner to how it’s done in AWS.
-	[**Microsoft Azure**](https://azure.microsoft.com/en) is the de facto choice for companies and teams that are focused on a Microsoft stack, and it has now placed significant emphasis on Linux as well
-	In **China**, AWS’ footprint is relatively small. The market is dominated by Alibaba’s [Alibaba Cloud](https://www.alibabacloud.com/), formerly called [Aliyun](https://intl.aliyun.com/).
-	Companies at (very) large scale may want to reduce costs by managing their own infrastructure. For example, [Dropbox migrated](https://news.ycombinator.com/item?id=11282948) to their own infrastructure.
-	Other cloud providers such as [Digital Ocean](https://www.digitalocean.com/) offer similar services, sometimes with greater ease of use, more personalized support, or lower cost. However, none of these match the breadth of products, mind-share, and market domination AWS now enjoys.
-	Traditional managed hosting providers such as [Rackspace](https://www.rackspace.com/) offer cloud solutions as well.

## **AWS vs. PaaS:** 

----

If your goal is just to put up a single service that does something relatively simple, and you’re trying to minimize time managing operations engineering, consider a [platform-as-a-service](https://en.wikipedia.org/wiki/Platform_as_a_service) such as [Heroku](https://www.heroku.com/). The AWS approach to PaaS, Elastic Beanstalk, is arguably more complex, especially for simple use cases.
## **AWS vs. web hosting:** 

----
If your main goal is to host a website or blog, and you don’t expect to be building an app or more complex service, you may wish consider one of the myriad [web hosting services](https://www.google.com/search?q=web+hosting).
## **AWS vs. managed hosting:** 

----
Traditionally, many companies pay [managed hosting](https://en.wikipedia.org/wiki/Dedicated_hosting_service) providers to maintain physical servers for them, then build and deploy their software on top of the rented hardware. This makes sense for businesses who want direct control over hardware, due to legacy, performance, or special compliance constraints, but is usually considered old fashioned or unnecessary by many developer-centric startups and younger tech companies.

## what is **Complexity of AWS:**

----
AWS will let you build and scale systems to the size of the largest companies, but the complexity of the services when used at scale requires significant depth of knowledge and experience. Even very simple use cases often require more knowledge to do “right” in AWS than in a simpler environment like Heroku or Digital Ocean. (This guide may help!)
## AWS **Geographic locations:** 

----
AWS has data centers in [over a dozen geographic locations](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions), known as **regions**, in Europe, East Asia, North and South America, and now Australia and India. It also has many more **edge locations** globally for reduced latency of services like CloudFront.
	-	See the [current list](https://aws.amazon.com/about-aws/global-infrastructure/) of regions and edge locations, including upcoming ones.
	-	If your infrastructure needs to be in close physical proximity to another service for latency or throughput reasons (for example, latency to an ad exchange), viability of AWS may depend on the location.
	
-	⛓**Lock-in:** As you use AWS, it’s important to be aware when you are depending on AWS services that do not have equivalents elsewhere.
	-	Lock-in may be completely fine for your company, or a significant risk. It’s important from a business perspective to make this choice explicitly, and consider the cost, operational, business continuity, and competitive risks of being tied to AWS. AWS is such a dominant and reliable vendor, many companies are comfortable with using AWS to its full extent. Others can tell stories about the [dangers of “cloud jail” when costs spiral](http://firstround.com/review/the-three-infrastructure-mistakes-your-company-must-not-make/).
	-	Generally, the more AWS services you use, the more lock-in you have to AWS — that is, the more engineering resources (time and money) it will take to change to other providers in the future.
	-	Basic services like virtual servers and standard databases are usually easy to migrate to other providers or on premises. Others like load balancers and IAM are specific to AWS but have close equivalents from other providers. The key thing to consider is whether engineers are architecting systems around specific AWS services that are not open source or relatively interchangeable. For example, Lambda, API Gateway, Kinesis, Redshift, and DynamoDB do not have substantially equivalent open source or commercial service equivalents, while EC2, RDS (MySQL or Postgres), EMR, and ElastiCache more or less do. (See more [below](#which-services-to-use), where these are noted with ⛓.)
## Can you **Combining AWS and other cloud providers:** 

----

Many customers combine AWS with other non-AWS services. 
For example, legacy systems or secure data might be in a managed hosting provider, while other systems are AWS. 
Or a company might only use S3 with another provider doing everything else. 
However small startups or projects starting fresh will typically stick to AWS or Google Cloud only.

## Is that posible to **Hybrid cloud** - combile private cloud with AWS public cloud:

----
In larger enterprises, it is common to have [hybrid deployments](https://aws.amazon.com/enterprise/hybrid/) encompassing private cloud or on-premises servers and AWS.
Or other enterprise cloud providers like [IBM](https://www.ibm.com/it-infrastructure/solutions/hybrid-cloud)/[Bluemix](http://www.ibm.com/cloud-computing/bluemix/hybrid/), [Microsoft](https://www.microsoft.com/en-us/cloud-platform/hybrid-cloud)/[Azure](https://azure.microsoft.com/en-us/overview/azure-stack/), [NetApp](http://www.netapp.com/us/solutions/cloud/hybrid-cloud/), or [EMC](http://www.emc.com/en-us/cloud/hybrid-cloud-computing/index.htm).

## Who is **Major customers of AWS :** Who uses AWS and Google Cloud?

----
-	AWS’s [list of customers](https://aws.amazon.com/solutions/case-studies/) includes large numbers of mainstream online properties and major brands, such as Netflix, Pinterest, Spotify (moving to Google Cloud), Airbnb, Expedia, Yelp, Zynga, Comcast, Nokia, and Bristol-Myers Squibb.
-	Azure’s [list of customers](https://azure.microsoft.com/en-us/case-studies/) includes companies such as NBC Universal, 3M and Honeywell Inc.
-	Google Cloud’s [list of customers](https://cloud.google.com/customers/) is large as well, and includes a few mainstream sites, such as [Snapchat](http://www.businessinsider.com/snapchat-is-built-on-googles-cloud-2014-1), Best Buy, Domino’s, and Sony Music.

## Which AWS Services to Use

-	**Immature and unpopular services:** Just because AWS has a service that sounds promising, it doesn’t mean you should use it.
Some services are very narrow in use case, not mature, are overly opinionated, or have limitations, so building your own solution may be better. 
We try to give a sense for this by breaking products into categories.

-	**Must-know infrastructure:** Most typical small to medium-size users will focus on the following services first. If you manage use of AWS systems, you likely need to know at least a little about all of these. (Even if you don’t use them, you should learn enough to make that choice intelligently.)
## what is AWS `IAM`:

[IAM](#security-and-iam): User accounts and identities (you need to think about accounts early on!)

## what is EC2 and its components:

----	
-	[EC2](#ec2): Virtual servers and associated components, including:
    -	[AMIs](#amis): Machine Images
    -	[Load Balancers](#load-balancers): CLBs and ALBs
    -	[Autoscaling](#auto-scaling): Capacity scaling (adding and removing servers based on load)
    -	[EBS](#ebs): Network-attached disks
    -	[Elastic IPs](#elastic-ips): Assigned IP addresses
  
## What is AWS S3 ?
		
	-	[S3](#s3): Storage of files
## What is Route 53 ? 

-	[Route 53](#route-53): DNS and domain registration

## what is AWS VPC ? 	
	-	[VPC](#vpcs-network-security-and-security-groups): Virtual networking, network security, and co-location; you automatically use
## What is cloud front ?	
	-	[CloudFront](#cloudfront): CDN for hosting content
	
## what is CloudWatch ?
-	[CloudWatch](#cloudwatch): Alerts, paging, monitoring
## What is AWS managed service ? 

-	**Managed services:** Existing software solutions you could run on your own, but with managed deployment:
	-	[RDS](#rds): Managed relational databases (managed MySQL, Postgres, and Amazon’s own Aurora database)
	-	[EMR](#emr): Managed Hadoop
	-	[Elasticsearch](https://aws.amazon.com/elasticsearch-service/): Managed Elasticsearch
	-	[ElastiCache](https://aws.amazon.com/elasticache/): Managed Redis and Memcached
## List out some of important AWS infrastructure  ? 

-	**Optional but important infrastructure:** These are key and useful infrastructure components that are less widely known and used. You may have legitimate reasons to prefer alternatives, so evaluate with care to be sure they fit your needs:
	-	⛓[Lambda](#lambda): Running small, fully managed tasks “serverless”
	-	[CloudTrail](https://aws.amazon.com/cloudtrail/): AWS API logging and audit (often neglected but important)
	-	⛓🕍[CloudFormation](#cloudformation): Templatized configuration of collections of AWS resources
	-	🕍[Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/): Fully managed (PaaS) deployment of packaged Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker applications
	-	🐥[EFS](#efs): Network filesystem compatible with NFSv4.1
	-	⛓🕍[ECS](#ecs): Docker container/cluster management (note Docker can also be used directly, without ECS)
	-   🕍 [EKS](#eks): Kubernetes (K8) Docker Container/Cluster management
	-	⛓[ECR](https://aws.amazon.com/ecr/): Hosted private Docker registry
	-	🐥[Config](https://aws.amazon.com/config/): AWS configuration inventory, history, change notifications
	-	🐥[X-Ray](https://aws.amazon.com/xray/): Trace analysis and debugging for distributed applications such as microservices.

## List out some of Special-purpose infrastructure AWS: 
**Special-purpose infrastructure:** These services are focused on specific use cases and should be evaluated if they apply to your situation. Many also are proprietary architectures, so tend to tie you to AWS.

-	⛓[DynamoDB](#dynamodb): Low-latency NoSQL key-value store
-	⛓[Glacier](#glacier): Slow and cheap alternative to S3
-	⛓[Kinesis](https://aws.amazon.com/kinesis/): Streaming (distributed log) service
-	⛓[SQS](https://aws.amazon.com/sqs/): Message queueing service
-	⛓[Redshift](#redshift): Data warehouse
-	🐥[QuickSight](https://aws.amazon.com/quicksight/): Business intelligence service
-	[SES](https://aws.amazon.com/ses/): Send and receive e-mail for marketing or transactions
-	⛓[API Gateway](https://aws.amazon.com/api-gateway/): Proxy, manage, and secure API calls
-	⛓[IoT](#iot): Manage bidirectional communication over HTTP, WebSockets, and MQTT between AWS and clients (often but not necessarily “things” like appliances or sensors)
-	⛓[WAF](https://aws.amazon.com/waf/): Web firewall for CloudFront to deflect attacks
-	⛓[KMS](#kms): Store and manage encryption keys securely
-	[Inspector](https://aws.amazon.com/inspector/): Security audit
-	[Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/): Automated tips on reducing cost or making improvements
-	🐥[Certificate Manager](https://aws.amazon.com/certificate-manager/): Manage SSL/TLS certificates for AWS services
-	🐥⛓[Fargate](https://aws.amazon.com/fargate/): Docker containers management, backend for ECS and EKS

## AWS compound services:

**Compound services:** These are similarly specific, but are full-blown services that tackle complex problems and may tie you in. 
Usefulness depends on your requirements. If you have large or significant need, you may have these already managed by in-house systems and engineering teams.
-	[Machine Learning](https://aws.amazon.com/machine-learning/): Machine learning model training and classification
-	[Lex](https://aws.amazon.com/lex/): Automatic speech recognition (ASR) and natural language understanding (NLU)
-	[Polly](https://aws.amazon.com/polly/): Text-to-speech engine in the cloud
-	[Rekognition](https://aws.amazon.com/rekognition/): Service for image recognition
-	⛓🕍[Data Pipeline](https://aws.amazon.com/datapipeline/): Managed ETL service
-	⛓🕍[SWF](https://aws.amazon.com/swf/): Managed state tracker for distributed polyglot job workflow
-	⛓🕍[Lumberyard](https://aws.amazon.com/lumberyard/): 3D game engine

## Some of AWS	**Mobile/app development service:**

-	[SNS](https://aws.amazon.com/sns/): Manage app push notifications and other end-user notifications
-	⛓🕍[Cognito](https://aws.amazon.com/cognito/): User authentication via Facebook, Twitter, etc.
-	[Device Farm](https://aws.amazon.com/device-farm/): Cloud-based device testing
-	[Mobile Analytics](https://aws.amazon.com/mobileanalytics/): Analytics solution for app usage
-	🕍[Mobile Hub](https://aws.amazon.com/mobile/): Comprehensive, managed mobile app framework
	
## some of AWS	**Enterprise services:** 

These are relevant if you have significant corporate cloud-based or hybrid needs. Many smaller companies and startups use other solutions, like Google Apps or Box. Larger companies may also have their own non-AWS IT solutions.

-	[AppStream](https://aws.amazon.com/appstream/): Windows apps in the cloud, with access from many devices
-	[Workspaces](https://aws.amazon.com/workspaces/): Windows desktop in the cloud, with access from many devices
-	[WorkDocs](https://aws.amazon.com/workdocs/) (formerly Zocalo): Enterprise document sharing
-	[WorkMail](https://aws.amazon.com/workmail/): Enterprise managed e-mail and calendaring service
-	[Directory Service](https://aws.amazon.com/directoryservice/): Microsoft Active Directory in the cloud
-	[Direct Connect](https://aws.amazon.com/directconnect/): Dedicated network connection between office or data center and AWS
-	[Storage Gateway](https://aws.amazon.com/storagegateway/): Bridge between on-premises IT and cloud storage
-	[Service Catalog](https://aws.amazon.com/servicecatalog/): IT service approval and compliance


### Tools and Services Market Landscape

There are now enough cloud and “big data” enterprise companies and products that few can keep up with the market landscape.

We’ve assembled a landscape of a few of the services. 
This is far from complete, but tries to emphasize services that are popular with AWS practitioners — services that specifically help with AWS, or a complementary, or tools almost anyone using AWS must learn.

![Popular Tools and Services for AWS Practitioners](figures/aws-market-landscape.png)


## AWS Common Concepts

-	📒 The AWS [**General Reference**](https://docs.aws.amazon.com/general/latest/gr/Welcome.html) covers a bunch of common concepts that are relevant for multiple services.
-	AWS allows deployments in [**regions**](https://docs.aws.amazon.com/general/latest/gr/rande.html), which are isolated geographic locations that help you reduce latency or offer additional redundancy. 
Regions contain availability zones(AZs), which are typically the first tool of choice for [high availability](#high-availability)). AZs are [physically separate from one another](https://www.youtube.com/watch?v=JIQETrFC_SQ&feature=youtu.be&t=1428) even within the same region, and [may span multiple physical data centers](https://blog.rackspace.com/aws-101-regions-availability-zones). While they are connected via low latency links, natural disasters afflicting one should not affect others.
-	Each service has API **endpoints** for each region. Endpoints differ from service to service and not all services are available in each region, as listed in [these tables](https://docs.aws.amazon.com/general/latest/gr/rande.html).
-	[**Amazon Resource Names (ARNs)**](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) are specially formatted identifiers for identifying resources. They start with 'arn:' and are used in many services, and in particular for IAM policies.

## How to Getting Help and Support on AWS services:

-	**Forums:** For many problems, it’s worth searching or asking for help in the [discussion forums](https://forums.aws.amazon.com/index.jspa) to see if it’s a known issue.
-	**Premium support:** AWS offers several levels of [premium support](https://aws.amazon.com/premiumsupport/).
	-	The first tier, called "Developer support" lets you file support tickets with 12 to 24 hour turnaround time, it starts at $29 but once your monthly spend reaches around $1000 it changes to a 3% surcharge on your bill.
	-	The higher-level support services are quite expensive — and increase your bill by up to 10%. Many large and effective companies never pay for this level of support. They are usually more helpful for midsize or larger companies needing rapid turnaround on deeper or more perplexing problems.
	-	Keep in mind, a flexible architecture can reduce need for support. You shouldn’t be relying on AWS to solve your problems often. For example, if you can easily re-provision a new server, it may not be urgent to solve a rare kernel-level issue unique to one EC2 instance. If your EBS volumes have recent snapshots, you may be able to restore a volume before support can rectify the issue with the old volume. If your services have an issue in one availability zone, you should in any case be able to rely on a redundant zone or migrate services to another zone.
	-	Larger customers also get access to AWS Enterprise support, with dedicated technical account managers (TAMs) and shorter response time SLAs.
	-	There is definitely some controversy about how useful the paid support is. The support staff don’t always seem to have the information and authority to solve the problems that are brought to their attention. Often your ability to have a problem solved may depend on your relationship with your account rep.
-	**Account manager:** If you are at significant levels of spend (thousands of US dollars plus per month), you may be assigned (or may wish to ask for) a dedicated account manager.
	-	These are a great resource, even if you’re not paying for premium support. Build a good relationship with them and make use of them, for questions, problems, and guidance.
	-	Assign a single point of contact on your company’s side, to avoid confusing or overwhelming them.
-	**Contact:** The main web contact point for AWS is [here](https://aws.amazon.com/contact-us/). Many technical requests can be made via these channels.
-	**Consulting and managed services:** For more hands-on assistance, AWS has established relationships with many [consulting partners](https://aws.amazon.com/partners/consulting/) and [managed service partners](https://aws.amazon.com/partners/msp/). The big consultants won’t be cheap, but depending on your needs, may save you costs long term by helping you set up your architecture more effectively, or offering specific expertise, e.g. security. Managed service providers provide longer-term full-service management of cloud resources.
-	**AWS Professional Services:** AWS provides [consulting services](https://aws.amazon.com/professional-services/) alone or in combination with partners.

### AWS Restrictions and Other Notes

-	🔸Lots of resources in Amazon have [**limits**](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) on them. 
This is actually helpful, so you don’t incur large costs accidentally. You have to request that quotas be increased by opening support tickets. 
Some limits are easy to raise, and some are not. (Some of these are noted in sections below.) Additionally, not all service limits are published.
- **Obtaining Current Limits and Usage:** Limit information for a service may be available from the service API, Trusted Advisor, both or neither (in which case you'll need to contact Support). [This page](http://awslimitchecker.readthedocs.io/en/latest/limits.html) from the awslimitchecker tool's documentation provides a nice summary of available retrieval options for each limit. The [tool](https://github.com/jantman/awslimitchecker) itself is also valuable for automating limit checks.
-	🔸[**AWS terms of service**](https://aws.amazon.com/service-terms/) are extensive. Much is expected boilerplate, but it does contain important notes and restrictions on each service. In particular, there are restrictions against using many AWS services in **safety-critical systems**. (Those appreciative of legal humor may wish to review [clause 42.10](https://www.theguardian.com/technology/2016/feb/11/amazon-terms-of-service-zombie-apocalypse).)

### Related Topics

-	[OpenStack](https://www.openstack.org/) is a private cloud alternative to AWS used by large companies that wish to avoid public cloud offerings.

Learning and Career Development
-------------------------------

### AWS Certifications

**Certifications:** AWS offers [**certifications**](https://aws.amazon.com/certification/) for IT professionals who want to demonstrate their knowledge.

-	[Certified Cloud Practitioner](https://aws.amazon.com/certification/certified-cloud-practitioner/)
-	[Certified Solutions Architect Associate](https://aws.amazon.com/certification/certified-solutions-architect-associate/)
-	[Certified Developer Associate](https://aws.amazon.com/certification/certified-developer-associate/)
-	[Certified SysOps Administrator Associate](https://aws.amazon.com/certification/certified-sysops-admin-associate/)
-	[Certified Solutions Architect Professional](https://aws.amazon.com/certification/certified-solutions-architect-professional/)
-	[Certified DevOps Engineer Professional](https://aws.amazon.com/certification/certified-devops-engineer-professional/)
- [Certified Security – Specialty](https://aws.amazon.com/certification/certified-security-specialty/)
- [Certified Big Data – Specialty](https://aws.amazon.com/certification/certified-big-data-specialty/)
- [Certified Advanced Networking – Specialty](https://aws.amazon.com/certification/certified-advanced-networking-specialty/)
- [Certified Machine Learning – Specialty](https://aws.amazon.com/certification/certified-machine-learning-specialty/)
- [Certified Alexa Skill Builder – Specialty](https://aws.amazon.com/certification/certified-alexa-skill-builder-specialty/)
- [Certified Data Analytics – Specialty](https://aws.amazon.com/certification/certified-data-analytics-specialty/)
- [Certified Database – Specialty](https://aws.amazon.com/certification/certified-database-specialty/)

Associate level certifications were once required as pre-requisites to taking the Professional examinations - this is no longer the case.

-	**Getting certified:** If you’re interested in studying for and getting certifications, [this practical overview](https://gist.github.com/leonardofed/bbf6459ad154ad5215d354f3825435dc) tells you a lot of what you need to know. The official page is [here](https://aws.amazon.com/training/) and there is an [FAQ](https://aws.amazon.com/certification/faqs/).
-	**Training for certifications:** Training is offered by AWS themselves (mainly instructor-led and on-site) and various third-party companies (usually as video-based training) such as [A Cloud Guru](https://acloud.guru/aws-cloud-training), [CloudAcademy](https://cloudacademy.com/library/amazon-web-services/) and [Linux Academy](https://linuxacademy.com/library/topics/AWS/type/Course/).
-	**Do you need a certification?** Especially in consulting companies or when working in key tech roles in large non-tech companies, certifications are important credentials. In others, including in many tech companies and startups, certifications are not common or considered necessary. (In fact, fairly or not, some Silicon Valley hiring managers and engineers see them as a “negative” signal on a resume.)

Certifications are required to access certificate lounges at official AWS events such as [Summits](https://aws.amazon.com/events/summits/) and [re:Invent](https://reinvent.awsevents.com). Lounges typically provide power charging points, seats and relatively better coffee.






Further Reading
---------------

This section covers a few unusually useful or “must know about” resources or lists.

-	AWS
	-	[AWS In Plain English](https://www.expeditedssl.com/aws-in-plain-english): A readable overview of all the AWS services.
	-	[Awesome AWS](https://github.com/donnemartin/awesome-aws): A curated list of AWS tools and software.
	-	[AWS Tips I Wish I'd Known Before I Started](https://wblinks.com/notes/aws-tips-i-wish-id-known-before-i-started/): A list of tips from [Rich Adams](https://richadams.me/)
	-	[AWS Whitepapers](https://aws.amazon.com/whitepapers/): A list of technical AWS whitepapers, covering topics such as architecture, security and economics.
	-	[Last Week in AWS](https://lastweekinaws.com): A weekly email newsletter covering the latest happenings in the AWS ecosystem.
	-	[AWS Geek](https://www.awsgeek.com): A blog by AWS Community Hero Jerry Hargrove, with notes and hand-drawn diagrams about various AWS services.
-	Books
	-	[Amazon Web Services in Action](https://www.manning.com/books/amazon-web-services-in-action)
	-	[AWS Lambda in Action](https://www.manning.com/books/aws-lambda-in-action)
	-	[Serverless Architectures on AWS](https://www.manning.com/books/serverless-architectures-on-aws)
	-	[Serverless Single Page Apps](https://pragprog.com/book/brapps/serverless-single-page-apps)
	-	[The Terraform Book](https://terraformbook.com/)
	-	[AWS Scripted 2 book series](https://www.amazon.com/gp/product/B016QBB0GO?ref=series_rw_dp_labf)
	-	[Amazon Web Services For Dummies](https://www.amazon.com/dp/1118571835)
	-	[AWS System Administration](http://shop.oreilly.com/product/0636920027638.do)
	-	[Python and AWS Cookbook](http://shop.oreilly.com/product/0636920020202.do)
	-	[Resilience and Reliability on AWS](http://shop.oreilly.com/product/0636920026839.do)
	-	[AWS documentation as Kindle ebooks](https://www.amazon.com/Amazon-Web-Services/e/B007R6MVQ6)
-	General references
	-	[AWS Well Architected Framework Guide](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf): Amazon’s own 56 page guide to operational excellence - guidelines and checklists to validate baseline security, reliability, performance (including high availability) and cost optimization practices.
	-	[Awesome Microservices](https://github.com/mfornos/awesome-microservices): A curated list of tools and technologies for microservice architectures. Worth browsing to learn about popular open source projects.
	-	[Is it fast yet?](https://istlsfastyet.com/): Ilya Grigorik’s TLS performance overview
	-	[High Performance Browser Networking](https://hpbn.co/): A full, modern book on web network performance; a presentation on the HTTP/2 portion is [here](https://docs.google.com/presentation/d/1r7QXGYOLCh4fcUq0jDdDwKJWNqWK1o4xMtYpKZCJYjM/edit?usp=sharing).

Disclaimer
----------

The authors and contributors to this content cannot guarantee the validity of the information found here. Please make sure that you understand that the information provided here is being provided freely, and that no kind of agreement or contract is created between you and any persons associated with this content or project. The authors and contributors do not assume and hereby disclaim any liability to any party for any loss, damage, or disruption caused by errors or omissions in the information contained in, associated with, or linked from this content, whether such errors or omissions result from negligence, accident, or any other cause.

License
-------

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

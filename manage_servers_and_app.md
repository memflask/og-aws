Managing Servers and Applications
---------------------------------

### AWS vs Server Configuration

This guide is about AWS, not DevOps or server configuration management in general. But before getting into AWS in detail, it’s worth noting that in addition to the configuration management for your AWS resources, there is the long-standing problem of configuration management for servers themselves.

### Philosophy

-	Heroku’s [**Twelve-Factor App**](http://12factor.net/) principles list some established general best practices for deploying applications.
-	**Pets vs cattle:** Treat servers [like cattle, not pets](https://www.engineyard.com/blog/pets-vs-cattle). That is, design systems so infrastructure is disposable. It should be minimally worrisome if a server is unexpectedly destroyed.
-	The concept of [**immutable infrastructure**](http://radar.oreilly.com/2015/06/an-introduction-to-immutable-infrastructure.html) is an extension of this idea.
-	Minimize application state on EC2 instances. In general, instances should be able to be killed or die unexpectedly with minimal impact. State that is in your application should quickly move to RDS, S3, DynamoDB, EFS, or other data stores not on that instance. EBS is also an option, though it generally should not be the bootable volume, and EBS will require manual or automated re-mounting.

### Server Configuration Management

-	There is a [large set](https://en.wikipedia.org/wiki/Comparison_of_open-source_configuration_management_software) of open source tools for managing configuration of server instances.
-	These are generally not dependent on any particular cloud infrastructure, and work with any variety of Linux (or in many cases, a variety of operating systems).
-	Leading configuration management tools are [Puppet](https://github.com/puppetlabs/puppet), [Chef](https://github.com/chef/chef), [Ansible](https://github.com/ansible/ansible), and [Saltstack](https://github.com/saltstack/salt). These aren’t the focus of this guide, but we may mention them as they relate to AWS.

### Containers and AWS

-	[Docker](http://blog.scottlowe.org/2014/03/11/a-quick-introduction-to-docker/) and the containerization trend are changing the way many servers and services are deployed in general.
-	Containers are designed as a way to package up your application(s) and all of their dependencies in a known way. When you build a container, you are including every library or binary your application needs, outside of the kernel. A big advantage of this approach is that it’s easy to test and validate a container locally without worrying about some difference between your computer and the servers you deploy on.
-	A consequence of this is that you need fewer AMIs and boot scripts; for most deployments, the only boot script you need is a template that fetches an exported docker image and runs it.
-	Companies that are embracing [microservice architectures](http://martinfowler.com/articles/microservices.html) will often turn to container-based deployments.
-	AWS launched [ECS](https://aws.amazon.com/ecs/) as a service to manage clusters via Docker in late 2014, though many people still deploy Docker directly themselves. See the [ECS section](#ecs) for more details.
-	AWS launched [EKS](https://aws.amazon.com/eks/) as a service to manage Kubernetes Clusters mid 2018, though many people still deploy ECS or use Docker directly themselves. See the [EKS section](#eks) for more details.

### Visibility

-	Store and track instance metadata (such as instance id, availability zone, etc.) and deployment info (application build id, Git revision, etc.) in your logs or reports. The [**instance metadata service**](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) can help collect some of the AWS data you’ll need.
-	**Use log management services:** Be sure to set up a way to view and manage logs externally from servers.
	-	Cloud-based services such as [Sumo Logic](https://www.sumologic.com/), [Splunk Cloud](http://www.splunk.com/en_us/cloud.html), [Scalyr](https://www.scalyr.com/), [LogDNA](https://www.logdna.com/), and [Loggly](https://www.loggly.com/) are the easiest to set up and use (and also the most expensive, which may be a factor depending on how much log data you have).
	-	Major open source alternatives include [Elasticsearch](https://github.com/elastic/elasticsearch), [Logstash](https://github.com/elastic/logstash), and [Kibana](https://github.com/elastic/kibana) (the “[Elastic Stack](https://www.elastic.co/webinars/introduction-elk-stack)”) and [Graylog](https://www.graylog.org/).
	-	If you can afford it (you have little data or lots of money) and don’t have special needs, it makes sense to use hosted services whenever possible, since setting up your own scalable log processing systems is notoriously time consuming.
-	**Track and graph metrics:** The AWS Console can show you simple graphs from CloudWatch, you typically will want to track and graph many kinds of metrics, from CloudWatch and your applications. Collect and export helpful metrics everywhere you can (and as long as volume is manageable enough you can afford it).
	-	Services like [Librato](https://www.librato.com/), [KeenIO](https://keen.io/), and [Datadog](https://www.datadoghq.com/) have fancier features or better user interfaces that can save a lot of time. (A more detailed comparison is [here](http://blog.takipi.com/production-tools-guide/visualization-and-metrics/).)
	-	Use [Prometheus](https://prometheus.io) or [Graphite](https://github.com/graphite-project/graphite-web) as timeseries databases for your metrics (both are open source).
	-	[Grafana](https://github.com/grafana/grafana) can visualize with dashboards the stored metrics of both timeseries databases (also open source).

### Tips for Managing Servers

-	❗**Timezone settings on servers**: unless *absolutely necessary*, always **set the timezone on servers to [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)** (see instructions for your distribution, such as [Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-timezone-and-ntp-synchronization-on-ubuntu-14-04-quickstart), [CentOS](https://www.vultr.com/docs/setup-timezone-and-ntp-on-centos-6) or [Amazon](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html) Linux). Numerous distributed systems rely on time for synchronization and coordination and UTC [provides](https://blog.serverdensity.com/set-your-server-timezone-to-utc/) the universal reference plane: it is not subject to  daylight savings changes and adjustments in local time. It will also save you a lot of headache debugging [elusive timezone issues](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html) and provide coherent timeline of events in your logging and audit systems.
-	**NTP and accurate time:** If you are not using Amazon Linux (which comes preconfigured), you should confirm your servers [configure NTP correctly](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html#configure_ntp), to avoid insidious time drift (which can then cause all sorts of issues, from breaking API calls to misleading logs). This should be part of your automatic configuration for every server. If time has already drifted substantially (generally >1000 seconds), remember NTP won’t shift it back, so you may need to remediate manually (for example, [like this](http://askubuntu.com/questions/254826/how-to-force-a-clock-update-using-ntp) on Ubuntu).
-	**Testing immutable infrastructure:** If you want to be proactive about testing your service’s ability to cope with instance termination or failure, it can be helpful to introduce random instance termination during business hours, which will expose any such issues at a time when engineers are available to identify and fix them. Netflix’s [Simian Army](https://github.com/Netflix/SimianArmy) (specifically, [Chaos Monkey](https://github.com/Netflix/SimianArmy/wiki/Chaos-Monkey)) is a popular tool for this. Alternatively, [chaos-lambda](https://github.com/bbc/chaos-lambda) by the BBC is a lightweight option which runs on AWS [Lambda](#lambda).

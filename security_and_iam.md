+++
memflask = True
isdraft = False
+++

# Security and IAM

We cover security basics first, since configuring user accounts is something you usually have to do early on when setting up your system.

## Security and IAM Basics

-	📒 IAM [Homepage](https://aws.amazon.com/iam/) ∙ [User guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) ∙ [FAQ](https://aws.amazon.com/iam/faqs/)
-	The [AWS Security Blog](https://blogs.aws.amazon.com/security) is one of the best sources of news and information on AWS security.
-	**IAM** is the service you use to manage accounts and permissioning for AWS.
-	Managing security and access control with AWS is critical, so every AWS administrator needs to use and understand IAM, at least at a basic level.
-	[IAM identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) include users (people or services that are using AWS), groups (containers for sets of users and their permissions), and roles (containers for permissions assigned to AWS service instances). [Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions.html) for these identities are governed by [policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) You can use AWS pre-defined policies or custom policies that you create.
-	IAM manages various kinds of authentication, for both users and for software services that may need to authenticate with AWS, including:
	-	[**Passwords**](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords.html) to log into the console. These are a username and password for real users.
	-	[**Access keys**](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html), which you may use with command-line tools. These are two strings, one the “id”, which is an upper-case alphabetic string of the form 'AXXXXXXXXXXXXXXXXXXX', and the other is the secret, which is a 40-character mixed-case base64-style string. These are often set up for services, not just users.
		-	📜 Access keys that start with AKIA are normal keys. Access keys that start with ASIA are session/temporary keys from STS, and will require an additional "SessionToken" parameter to be sent along with the id and secret. See the documentation for [a complete list of access key prefixes](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-prefixes).
	-	[**Multi-factor authentication (MFA)**](https://aws.amazon.com/iam/details/mfa/), which is the highly recommended practice of using a keychain fob or smartphone app as a second layer of protection for user authentication.
-	IAM allows complex and fine-grained control of permissions, dividing users into groups, assigning permissions to roles, and so on. There is a [policy language](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) that can be used to customize security policies in a fine-grained way.
	-	An excellent high level overview of IAM policy concepts lives at [IAM Policies In A Nutshell](http://start.jcolemorrison.com/aws-iam-policies-in-a-nutshell/).
	-	🔸The policy language has a complex and error-prone JSON syntax that’s quite confusing, so unless you are an expert, it is wise to base yours off trusted examples or AWS’ own pre-defined [managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html).
-	At the beginning, IAM policy may be very simple, but for large systems, it will grow in complexity, and need to be managed with care.
	-	🔹Make sure one person (perhaps with a backup) in your organization is formally assigned ownership of managing IAM policies, make sure every administrator works with that person to have changes reviewed. This goes a long way to avoiding accidental and serious misconfigurations.
-	It is best to give each user or service the minimum privileges needed to perform their duties. This is the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege), one of the foundations of good security. Organize all IAM users and groups according to levels of access they need.
-	IAM has the [permission hierarchy](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) of:
	1. Explicit deny: The most restrictive policy wins.
	2. Explicit allow: Access permissions to any resource has to be explicitly given.
	3. Implicit deny: All permissions are implicitly denied by default.
-	You can test policy permissions via the AWS IAM [policy simulator tool tool](https://policysim.aws.amazon.com/home/index.jsp). This is particularly useful if you write custom policies.


## Security and IAM Tips

-	🔹Use IAM to create individual user accounts and **use IAM accounts for all users from the beginning**. This is slightly more work, but not that much.
	-	That way, you define different users, and groups with different levels of privilege (if you want, choose from Amazon’s default suggestions, of administrator, power user, etc.).
	-	This allows credential revocation, which is critical in some situations. If an employee leaves, or a key is compromised, you can revoke credentials with little effort.
	-	You can set up [Active Directory federation](https://blogs.aws.amazon.com/security/post/Tx71TWXXJ3UI14/Enabling-Federation-to-AWS-using-Windows-Active-Directory-ADFS-and-SAML-2-0) to use organizational accounts in AWS.
-	❗**Enable [MFA](https://aws.amazon.com/iam/details/mfa/)** on your account.
	-	You should always use MFA, and the sooner the better — enabling it when you already have many users is extra work.
	-	Unfortunately it can’t be enforced in software, so an administrative policy has to be established.
	-	Most users can use the Google Authenticator app (on [iOS](https://itunes.apple.com/us/app/google-authenticator/id388497605) or [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)) to support two-factor authentication. For the root account, consider a hardware fob.
- ❗Restrict use of significant IAM credentials as much as possible. Remember that in the cloud, loss of a highly capable IAM credential could essentially mean “game over,” for your deployment, your users, or your whole company.
  -	**Do NOT use the [Root User account](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)** other than when you initially create your account.  Create custom IAM users and/or roles and use those for your applications instead.
	- Lock up access and use of the root credentials as much as possible. Ideally they should be effectively “offline.” For critical deployments, this means attached to an actual MFA device, physically secured and rarely used.
-	❗**Turn on CloudTrail:** One of the first things you should do is [enable CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-a-trail-using-the-console-first-time.html). Even if you are not a security hawk, there is little reason not to do this from the beginning, so you have data on what has been happening in your AWS account should you need that information. You’ll likely also want to set up a [log management service](#visibility) to search and access these logs.
-	🔹**Use IAM roles for EC2:** Rather than assign IAM users to applications like services and then sharing the sensitive credentials, [define and assign roles to EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) and have applications retrieve credentials from the [instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html).
-	Assign IAM roles by realm — for example, to development, staging, and production. If you’re setting up a role, it should be tied to a specific realm so you have clean separation. This prevents, for example, a development instance from connecting to a production database.
-	**Best practices:** AWS’ [list of best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) is worth reading in full up front.
-	**IAM Reference:** [This interactive reference for all IAM actions, effects, and resources](https://iam.cloudonaut.io/) is great to have open while writing new or trying to understand existing IAM policies.
-	**Multiple accounts:** Decide on whether you want to use multiple AWS accounts and [research](https://dab35129f0361dca3159-2fe04d8054667ffada6c4002813eccf0.ssl.cf1.rackcdn.com/downloads/pdfs/Rackspace%20Best%20Practices%20for%20AWS%20-%20Identity%20Managment%20-%20Billing%20-%20Auditing.pdf) how to organize access across them. Factors to consider:
	-	Number of users
	-	Importance of isolation
		-	Resource Limits
		-	Permission granularity
		-	Security
		-	API Limits
	-	Regulatory issues
	-	Workload
	-	Size of infrastructure
	-	Cost of multi-account “overhead”: Internal AWS service management tools may need to be custom built or adapted.
	-	🔹It can help to use separate AWS accounts for independent parts of your infrastructure if you expect a high rate of AWS API calls, since AWS [throttles calls](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/query-api-troubleshooting.html#api-request-rate) at the AWS account level.
-	[**Inspector**](https://aws.amazon.com/inspector/) is an automated security assessment service from AWS that helps identify common security risks. This allows validation that you adhere to certain security practices and may help with compliance.
-	[**Trusted Advisor**](https://aws.amazon.com/blogs/aws/trusted-advisor-console-basic/) addresses a variety of best practices, but also offers some basic security checks around IAM usage, security group configurations, and MFA. At paid support tiers, Trusted Advisor exposes additional checks around other areas, such as reserved instance optimization.
-	**Use KMS for managing keys**: AWS offers [KMS](#kms) for securely managing encryption keys, which is usually a far better option than handling key security yourself. See [below](#kms).
-	[**AWS WAF**](https://aws.amazon.com/waf) is a web application firewall to help you protect your applications from common attack patterns.
-	**Security auditing:**
	-	[Security Monkey](https://github.com/Netflix/security_monkey) is an open source tool that is designed to assist with security audits.
	-	[Scout2](https://github.com/nccgroup/Scout2) is an open source tool that uses AWS APIs to assess an environment’s security posture. Scout2 is stable and actively maintained.
	-	🔹**Export and audit security settings:** You can audit security policies simply by exporting settings using AWS APIs, e.g. using a Boto script like [SecConfig.py](https://gist.github.com/jlevy/cce1b44fc24f94599d0a4b3e613cc15d) (from [this 2013 talk](http://www.slideshare.net/AmazonWebServices/intrusion-detection-in-the-cloud-sec402-aws-reinvent-2013)) and then reviewing and monitoring changes manually or automatically.

## Security and IAM Gotchas and Limitations

-	❗**Don’t share user credentials:** It’s remarkably common for first-time AWS users to create one account and one set of credentials (access key or password), and then use them for a while, sharing among engineers and others within a company. This is easy. But *don’t do this*. This is an insecure practice for many reasons, but in particular, if you do, you will have reduced ability to revoke credentials on a per-user or per-service basis (for example, if an employee leaves or a key is compromised), which can lead to serious complications.
-	❗**Instance metadata throttling:** The [instance metadata service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) has rate limiting on API calls. If you deploy IAM roles widely (as you should!) and have lots of services, you may hit global account limits easily.
	-	One solution is to have code or scripts cache and reuse the credentials locally for a short period (say 2 minutes). For example, they can be put into the ~/.aws/credentials file but must also be refreshed automatically.
	-	But be careful not to cache credentials for too long, as [they expire](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#instance-metadata-security-credentials). (Note the other [dynamic metadata](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html#dynamic-data-categories) also changes over time and should not be cached a long time, either.)
-	🔸Some IAM operations are slower than other API calls (many seconds), since AWS needs to propagate these globally across regions.
-	❗The uptime of IAM’s API has historically been lower than that of the instance metadata API. Be wary of incorporating a dependency on IAM’s API into critical paths or subsystems — for example, if you validate a user’s IAM group membership when they log into an instance and aren’t careful about precaching group membership or maintaining a back door, you might end up locking users out altogether when the API isn’t available.
-	❗**Don't check in AWS credentials or secrets to a git repository.** There are bots that scan GitHub looking for credentials. Use scripts or tools, such as [git-secrets](https://github.com/awslabs/git-secrets) to prevent anyone on your team from checking in sensitive information to your git repositories.

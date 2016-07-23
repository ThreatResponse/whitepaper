# Hardening AWS Environments and Automating Incident Response for AWS Compromises

## Abstract
Incident response in the cloud is performed differently than when performed in on-premise systems. Specifically, in a cloud environment a responder can not walk up to the physical asset, clone the drive with a write-blocker, or perform any action that requires hands on time with the system in question. Incident response best practices advise following predefined practiced procedures when dealing with a security incident, but organizations moving infrastructure to the cloud may fail to realize the procedural differences in obtaining forensic evidence. Furthermore, while cloud providers produce documents on handling incident response in the cloud, these documents may fail to address the newly released features or services that can aid incident response or help harden cloud infrastructure.

In this paper, we detail the evidence an incident responder should collect and the mitigations that should be preformed for an AWS instance (host) or key compromise. We introduce tools to automatically collect this evidence, mitigate the host or key compromise, and provide a web based workstation, tailored to managing AWS compromises. Finally, we introduce a tool that will help AWS users understand their current AWS environment, and offer suggestions to both decrease their security risk and increase valuable evidence they can collect.

## Authors & Contributors
**Andrew Krug** is a Senior Software Engineer at a large cyber security company. Krug has been Consultant, Network Architect, Systems Administrator, Operations Manager, Technical Trainer, and Software Engineer. Currently Krug works to develop gamified security education through security simulation scenarios.

**Alex McCormack** is a Principal Software Developer at a large cyber security company. Alex assists in the design and implementation of Capture the Flag competitions and training events. Alex has designed CTF challenges since 2013 and given training since 2012. Prior to developing CTFs, Alex worked in Incident Response and Malware Analysis.

TODO: expand Joel's and Jeff's bios

**Joel Ferrier** is the creator of Margarita Shotgun

**Jeff Parr** is a Front End Guru



## Nuances of Incident Response in AWS


## Mitigations to Preform
In March of 2016, Toni de la Fuente wrote a [blog post][blyx] detailing AWS steps to perform to collect evidence and mitigate a compromise. The mitigations included isolation, tagging, and shutting down an instance.

Isolation consists of creating a security group with exceptionally prohibitive access rules. Outbound traffic should be blocked completely, and inbound traffic should only be allowed by the specific IP address of the examiner.

The instance should then be tagged with a case number for record keeping and to alert other users that this instance should be treated with care. Evidence is then collected and the instance is shut down.

## Evidence to Collect
Two of the most important pieces of evidence to collect during an incident are disk and memory images. Disk image preservation is important because on the disk of a compromised host there may be host specific logs detailing what happened. There may also be copies of malware or other artifacts left by an attacker. Some attackers will alter the code of web applications to insert back doors, or collect sensitive data. Without forensic disk data, it may be impossible to determine what an attacker did after gaining access to the system.

Memory analysis is increasingly becoming a critical technique for forensic investigations. Memory analysis can be used to collect malware that may have been deleted from the disk, or never written to the disk in the first place. Memory analysis can be used to collect commands typed into a shell, discover programs hidden by rootkits, and much more.

In addition to disk and memory evidence, there are many AWS specific data points that may aid in an investigation. Metadata of an instance will reveal the public and private IP addresses of an instance, and the associated security groups. Console output and console screen shots may provide debugging messages from crashed services. VPC Flow logs may illustrate where an attack came from, and the destination of exfiltrated data. Finally, logs from CloudTrail may provide insights into actions performed by IAM users.


## Automating IR with ThreatResponse Tools

### AWS-IR: Automatic Evidence Collection and Mitigation

### ThreatResponse-Web: An Incident Response Workstation for AWS

### ThreatPrep: Preparing your environment for optimal evidence collection.

ThreatPrep is a tool to examine an AWS environment with two main objectives. Firstly, identify areas where the security posture could be increased and secondly, identify areas where the amount of forensic evidence could be increased.

The following items are currently checked by ThreatPrep:

 - S3 Buckets have versioning and logging enabled, and do not allow public reading or writing.
 - IAM users have MFA enabled, credentials have recently been rotated, and users are not attached to the AdministratorAccess policy.
 - VPCs have flow logs enabled.
 - Billing alerts are enabled.
 - Multi-regional CloudTrail is enabled.

ThreatPrep can be used either from a command line, or used in a python project. In fact, the ThreatResponse-Web project includes output from ThreatPrep in the advise section.

ThreatPrep offers similar advice as AWS Trusted Advisor. Trusted Advisor is a service that checks for both security and performance improvements that could be made in an AWS environment. Trusted Advisor is currently only available in the console and can not be accessed programatically. Trusted Advisor is provided through the support plan of an account. AWS Trusted Advisor provides a few checks for accounts with the basic support plan, but the full set of checks is only available to Business or Enterprise support plans.

ThreatPrep also shares similarities to AWS Config and Config Rules. Config deals with logging configuration details, called  a *configuration item* for supported AWS resources whenever that resource is created, deleted, or changed. AWS Config rules is a distinct offering from Config. Config Rules is responsible for evaluating the configuration item against a set of predefined criteria and then alerting AWS users if that criteria is not met. AWS provides a set of configurable rules users may use, as well as the ability to make custom rules. Many of the predefined rules cover similar areas as Trusted Advisor. The price to store a configuration item is about a third of a cent so the cost of enabling AWS Config is unlikely to cause significant price increases. AWS Config Rules cost two dollars per month per rule, and possibly more depending on how many times the rule is evaluated.





## References and Prior Work

 - [Forensics in AWS: an introduction][blyx]. Toni de la Fuente, 2016


[blyx]: http://blyx.com/2016/03/11/forensics-in-aws-an-introduction/

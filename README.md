# Amazon Kinesis Client Library for Java [![Build Status](https://travis-ci.org/awslabs/amazon-kinesis-client.svg?branch=master)](https://travis-ci.org/awslabs/amazon-kinesis-client)

The **Amazon Kinesis Client Library for Java** (Amazon KCL) enables Java developers to easily consume and process data from [Amazon Kinesis][kinesis].

* [Kinesis Product Page][kinesis]
* [Forum][kinesis-forum]
* [Issues][kinesis-client-library-issues]

## Features

* Provides an easy-to-use programming model for processing data using Amazon Kinesis
* Helps with scale-out and fault-tolerant processing

## Getting Started

1. **Sign up for AWS** &mdash; Before you begin, you need an AWS account. For more information about creating an AWS account and retrieving your AWS credentials, see [AWS Account and Credentials][docs-signup] in the AWS SDK for Java Developer Guide.
1. **Sign up for Amazon Kinesis** &mdash; Go to the Amazon Kinesis console to sign up for the service and create an Amazon Kinesis stream. For more information, see [Create an Amazon Kinesis Stream][kinesis-guide-create] in the Amazon Kinesis Developer Guide.
1. **Minimum requirements** &mdash; To use the Amazon Kinesis Client Library, you'll need **Java 1.7+**. For more information about Amazon Kinesis Client Library requirements, see [Before You Begin][kinesis-guide-begin] in the Amazon Kinesis Developer Guide.
1. **Using the Amazon Kinesis Client Library** &mdash; The best way to get familiar with the Amazon Kinesis Client Library is to read [Developing Record Consumer Applications][kinesis-guide-applications] in the Amazon Kinesis Developer Guide.

## Building from Source

After you've downloaded the code from GitHub, you can build it using Maven. To disable GPG signing in the build, use this command: `mvn clean install -Dgpg.skip=true`

## Integration with the Kinesis Producer Library
For producer-side developers using the **[Kinesis Producer Library (KPL)][kinesis-guide-kpl]**, the KCL integrates without additional effort.  When the KCL retrieves an aggregated Amazon Kinesis record consisting of multiple KPL user records, it will automatically invoke the KPL to extract the individual user records before returning them to the user.

## Amazon KCL support for other languages
To make it easier for developers to write record processors in other languages, we have implemented a Java based daemon, called MultiLangDaemon that does all the heavy lifting. Our approach has the daemon spawn a sub-process, which in turn runs the record processor, which can be written in any language. The MultiLangDaemon process and the record processor sub-process communicate with each other over [STDIN and STDOUT using a defined protocol][multi-lang-protocol]. There will be a one to one correspondence amongst record processors, child processes, and shards. For Python developers specifically, we have abstracted these implementation details away and [expose an interface][kclpy] that enables you to focus on writing record processing logic in Python. This approach enables KCL to be language agnostic, while providing identical features and similar parallel processing model across all languages.

## Release Notes
### Release 1.6.4 (July 6, 2016)
* Upgrade to AWS SDK for Java 1.11.14
  * [Issue #74](https://github.com/awslabs/amazon-kinesis-client/issues/74)
  * [Issue #73](https://github.com/awslabs/amazon-kinesis-client/issues/73)
* **Maven Artifact Signing Change** 
  * Artifacts are now signed by the identity `Amazon Kinesis Tools <amazon-kinesis-tools@amazon.com>`

### Release 1.6.3 (May 12, 2016)
* Fix format exception caused by DEBUG log in LeaseTaker [Issue # 68](https://github.com/awslabs/amazon-kinesis-client/issues/68)

### Release 1.6.2 (March 23, 2016)
* Support for specifying max leases per worker and max leases to steal at a time.
* Support for specifying initial DynamoDB table read and write capacity.
* Support for parallel lease renewal.
* Support for graceful worker shutdown.
* Change DefaultCWMetricsPublisher log level to debug. [PR # 49](https://github.com/awslabs/amazon-kinesis-client/pull/49)
* Avoid NPE in MLD record processor shutdown if record processor was not initialized. [Issue # 29](https://github.com/awslabs/amazon-kinesis-client/issues/29)

### Release 1.6.1 (September 23, 2015)
* Expose [approximateArrivalTimestamp](http://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetRecords.html) for Records in processRecords API call.

### Release 1.6.0 (July 31, 2015)
* Restores compatibility with [dynamodb-streams-kinesis-adapter](https://github.com/awslabs/dynamodb-streams-kinesis-adapter) (which was broken in 1.4.0).

### Release 1.5.1 (July 20, 2015)
* KCL maven artifact 1.5.0 does not work with JDK 7. This release addresses this issue.

### Release 1.5.0 (July 9, 2015)
* **[Metrics Enhancements][kinesis-guide-monitoring-with-kcl]**
	* Support metrics level and dimension configurations to control CloudWatch metrics emitted by the KCL.
	* Add new metrics that track time spent in record processor methods.
	* Disable WorkerIdentifier dimension by default.
* **Exception Reporting** &mdash; Do not silently ignore exceptions in ShardConsumer.
* **AWS SDK Component Dependencies** &mdash; Depend only on AWS SDK components that are used.

### Release 1.4.0 (June 2, 2015)
* Integration with the **[Kinesis Producer Library (KPL)][kinesis-guide-kpl]**
	* Automatically de-aggregate records put into the Kinesis stream using the KPL.
	* Support checkpointing at the individual user record level when multiple user records are aggregated into one Kinesis record using the KPL.

 See [Consumer De-aggregation with the KCL][kinesis-guide-consumer-deaggregation] for details.

### Release 1.3.0 (May 22, 2015)
* A new metric called "MillisBehindLatest", which tracks how far consumers are from real time, is now uploaded to CloudWatch.

### Release 1.2.1 (January 26, 2015)
* **MultiLangDaemon** &mdash; Changes to the MultiLangDaemon to make it easier to provide a custom worker.

### Release 1.2 (October 21, 2014)
* **Multi-Language Support** &mdash; Amazon KCL now supports implementing record processors in any language by communicating with the daemon over [STDIN and STDOUT][multi-lang-protocol]. Python developers can directly use the [Amazon Kinesis Client Library for Python][kclpy] to write their data processing applications.

### Release 1.1 (June 30, 2014)
* **Checkpointing at a specific sequence number** &mdash; The IRecordProcessorCheckpointer interface now supports checkpointing at a sequence number specified by the record processor.
* **Set region** &mdash; KinesisClientLibConfiguration now supports setting the region name to indicate the location of the Amazon Kinesis service. The Amazon DynamoDB table and Amazon CloudWatch metrics associated with your application will also use this region setting.

[kinesis]: http://aws.amazon.com/kinesis
[kinesis-forum]: http://developer.amazonwebservices.com/connect/forum.jspa?forumID=169
[kinesis-client-library-issues]: https://github.com/awslabs/amazon-kinesis-client/issues
[docs-signup]: http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-setup.html
[kinesis-guide]: http://docs.aws.amazon.com/kinesis/latest/dev/introduction.html
[kinesis-guide-begin]: http://docs.aws.amazon.com/kinesis/latest/dev/before-you-begin.html
[kinesis-guide-create]: http://docs.aws.amazon.com/kinesis/latest/dev/step-one-create-stream.html
[kinesis-guide-applications]: http://docs.aws.amazon.com/kinesis/latest/dev/kinesis-record-processor-app.html
[kinesis-guide-monitoring-with-kcl]: http://docs.aws.amazon.com//kinesis/latest/dev/monitoring-with-kcl.html
[kinesis-guide-kpl]: http://docs.aws.amazon.com//kinesis/latest/dev/developing-producers-with-kpl.html
[kinesis-guide-consumer-deaggregation]: http://docs.aws.amazon.com//kinesis/latest/dev/kinesis-kpl-consumer-deaggregation.html
[kclpy]: https://github.com/awslabs/amazon-kinesis-client-python
[multi-lang-protocol]: https://github.com/awslabs/amazon-kinesis-client/blob/master/src/main/java/com/amazonaws/services/kinesis/multilang/package-info.java


## Table of Contents  
[Update Your Configuration File](#update-your-configuration-file)

[Build the Application Jar](#build-the-application-jar-file)

[Run the Jar](#run-the-jar-supplying-your-configuration-as-an-option)  

## Update your configuration file

### Add Cluster Configuration

In your config.yml file, add your cluster configuration as shown here:

```
cluster:
  bootstrap.servers: pkc-l7p2j.us-west-2.aws.confluent.cloud:9092
  security.protocol: SASL_SSL
  sasl.jaas.config: org.apache.kafka.common.security.plain.PlainLoginModule required username="<API-KEY>" password="<API-SECRET>";
  sasl.mechanism: PLAIN
  ssl.endpoint.identification.algorithm: https
```

Any cluster connection configuration properties may be specified here, just make sure they align to the actual Kafka client properties exactly. 
For example, you can add "ssl.enabled.mechanisms". 
Any properties that are not specified or are left with a blank value will use the default values. 

### Add/Update Topics

In your `config.yml` file, update the section under "topics" with your desired topics as shown here:

```
topics:
  matt-topic-1:
    name: matt-topic-1
    replication.factor: 1
    partitions: 3
    cleanup.policy: 
    compression.type:
    retention.ms:
```

You can choose to leave the additional configurations for a topic blank (and defaults will be used), or you can enter them as well under the desired topic. 

If you are you using delete topics (default is disabled), you will need to make sure all topics that exist on the cluster are in your "topics" section or exist in the "default_topics" section, as shown here:

```
default_topics:
  - _confluent-metrics
  - __confluent.support.metrics
```

If you're not using delete topics, then you do not need to worry about using the "default_topics" section. 

### Add/Update/Remove ACLs

In your `config.yml` file, update the section under "acls" with your complete list of acls as shown here:

```
acls:
  project-xyz:
    resource-type: TOPIC
    resource-name: griz-test
    resource-pattern: LITERAL
    principal: User:12576
    host: '*'
    operation: DESCRIBE
    permission: ALLOW
```

Note that all fields are required for ACLs (unlike topics). Any ACLs that are not included in your list but that exist on the Kafka cluster will be removed when the application is run. 

## Build the application jar file

From the project root directly, run the following:

`mvn clean package`

## Run the Jar, supplying your configuration as an option

`java -jar <path-to-jar> <path-config.yml>`
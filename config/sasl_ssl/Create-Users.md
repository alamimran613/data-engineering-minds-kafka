# This docs help you to create users for SASL_SSL Authentication

## **First We Create broker-admin User/"User Principal" For Kafka Brokers**

bin/kafka-configs.sh --zookeeper ip-172-31-36-6.ap-south-1.compute.internal:2182 --zk-tls-config-file config/zookeeper-client.properties --entity-type users --entity-name broker-admin --alter --add-config 'SCRAM-SHA-512=[password=Dem123]'

## **List All Available Users**

bin/kafka-configs.sh --zookeeper ip-172-31-36-6.ap-south-1.compute.internal:2182 --zk-tls-config-file config/zookeeper-client.properties --entity-type users --describe

### **Describe SASL/SCRAM Specific User**

bin/kafka-configs.sh --zookeeper ip-172-31-36-6.ap-south-1.compute.internal:2182 --zk-tls-config-file config/zookeeper-client.properties --entity-type users --entity-name broker-admin --describe

## **Delete SASL/SCRAM User**

bin/kafka-configs.sh --zookeeper ip-172-31-36-6.ap-south-1.compute.internal:2182 --zk-tls-config-file config/zookeeper-client.properties --entity-type users --entity-name broker-admin --alter --delete-config 'SCRAM-SHA-512'

## For more info use this file

For more information about User Create, Delete, Describe use this file >> [](./README.md)

## Update server.properties

Update Broker-1 >> [](./server-0.properties)

Update Broker-2 >> [](./server-1.properties)

Update Broker-3 >> [](./server-2.properties)

## Restart all Brokers

sudo systemctl restart kafka

## **Create Producer User/User Principal For Producer Clients**

bin/kafka-configs.sh --zookeeper ip-172-31-36-6.ap-south-1.compute.internal:2182 --zk-tls-config-file zookeeper-client.properties --entity-type users --entity-name sasl-producer --alter --add-config 'SCRAM-SHA-512=[password=Dem1234]'

## **Create Consumer User/User Principal For Consumer Clients**

bin/kafka-configs.sh --zookeeper ip-172-31-36-6.ap-south-1.compute.internal:2182 --zk-tls-config-file zookeeper-client.properties --entity-type users --entity-name sasl-consumer --alter --add-config 'SCRAM-SHA-512=[password=Dem1234]'

## **Create ACLs For User/User Principal For Write and Read**

Use this file for create >> [](./create-acls.md)

## **After Create ACLs Configure Producer Properties**

Use This File >> [](./producer.properties)

## **After Create ACLs Configure Consumer Properties**

Use This File >> [](./consumer.properties)

## **Test Producer**

bin/kafka/kafka-console-producer.sh --bootstrap-server ip-172-31-45-22.ap-south-1.compute.internal:9092,ip-172-31-46-247.ap-south-1.compute.internal:9092,ip-172-31-38-217.ap-south-1.compute.internal:9092 --topic my-topic --producer.config config/producer.properties

## **Test Consumer**

bin/kafka/kafka-console-consumer.sh --bootstrap-server ip-172-31-45-22.ap-south-1.compute.internal:9092,ip-172-31-46-247.ap-south-1.compute.internal:9092,ip-172-31-38-217.ap-south-1.compute.internal:9092 --topic my-topic --consumer.config config/consumer.properties

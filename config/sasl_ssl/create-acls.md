# This docs help you to create ACLs for topic for User/User Principal

# **Create Access Control List for Topic for User Producer to Write, Describe and Describe Configs**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=ip-172-31-36-6.ap-south-1.compute.internal:2182 -zk-tls-config-file config/zookeeper-client.properties --add --allow-principal User:sasl-producer --topic my-topic --operation WRITE --operation DESCRIBE --operation DESCRIBECONFIGS

# **Create Access Control List for Topic for User Consumer to Read and Describe**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=ip-172-31-36-6.ap-south-1.compute.internal:2182 -zk-tls-config-file config/zookeeper-client.properties --add --allow-principal User:sasl-consumer --topic my-topic --operation READ --operation DESCRIBE

# **Create Access Control List for Topic for User Consumer For Consumer Group**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=ip-172-31-36-6.ap-south-1.compute.internal:2182 -zk-tls-config-file config/zookeeper-client.properties --add --allow-principal User:sasl-consumer --group sasl-consumer --operation READ

# **List all the ACLs associated with the user Producer**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=ip-172-31-36-6.ap-south-1.compute.internal:2182 -zk-tls-config-file config/zookeeper-client.properties --list --principal User:sasl-producer

# **List all the ACLs associated with the user Consumer**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=ip-172-31-36-6.ap-south-1.compute.internal:2182 -zk-tls-config-file config/zookeeper-client.properties --list --principal User:sasl-consumer


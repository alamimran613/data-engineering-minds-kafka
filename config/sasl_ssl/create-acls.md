# This docs help you to create ACLs for topic for User/User Principal

# **Create Access Control List for Topic for User Producer to Write, Describe and Describe Configs**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=localhost:2182 -zk-tls-config-file config/zookeeper-client.properties --add --allow-principal User:sasl-producer --topic my-topic --operation WRITE --operation DESCRIBE --operation DESCRIBECONFIGS

# **Create Access Control List for Topic for User Consumer to Read and Describe**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=localhost:2182 -zk-tls-config-file config/zookeeper-client.properties --add --allow-principal User:sasl-consumer --topic my-topic --operation READ --operation DESCRIBE

# **Create Access Control List for Topic for User Consumer For Consumer Group**

bin/kafka-acls.sh --authorizer-properties --zookeeper.connect=localhost:2182 -zk-tls-config-file config/zookeeper-client.properties --add --allow-principal User:sasl-consumer --group sasl-consumer --operation READ


**1. Create Truststore** <br />
keytool -keystore kafka.zookeeper-client.truststore.jks -alias ca-cert -import -file ca-cert

**2. Create Keystore** <br />
keytool -keystore kafka.zookeeper-client.keystore.jks -alias zookeeper-client -validity 3650 -genkey -keyalg RSA -ext SAN=dns:<enter your hostname/dns here>

### When prompt <What is your first and last name?> Then enter your <hostname/dns> here that you use above**

**3. Create certificate signing request (CSR)** <br />
keytool -keystore kafka.zookeeper-client.keystore.jks -alias zookeeper-client -certreq -file ca-request-zookeeper-client

**4. Sign the CSR** <br />
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-zookeeper-client -out ca-signed-zookeeper-client -days 3650 -CAcreateserial

**5. Import the CA into Keystore** <br />
keytool -keystore kafka.zookeeper-client.keystore.jks -alias ca-cert -import -file ca-cert

**6. Import the signed certificate from step 5 into Keystore** <br />
keytool -keystore kafka.zookeeper-client.keystore.jks -alias zookeeper-client -import -file ca-signed-zookeeper-client

**7. Create zookeeper-client.properties** <br />

nano zookeeper-client.properties

[](./zookeeper-client.properties)

### Check what inside Key store and Trust Store

keytool -list -v -keystore kafka.zookeeper-client.keystore.jks

keytool -list -v -keystore kafka.zookeeper-client.truststore.jks

### Check Your Certificate File Contents

keytool -printcert -v -file ca-cert

### Test the zookeeper shell using SSL Encryption with secure port 2182

zookeeper-shell.sh <enter your hostname/dns here>:2182 -zk-tls-config-file zookeeper-client.properties
ls /brokers/ids

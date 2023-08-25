## Configure Zookeeper To Broker Security SSL

## 1. Copy ca-cert to Broker 1

mkdir ~/ssl
cd ~/ssl

**1. Create Truststore** <br />
keytool -keystore kafka.broker1.truststore.jks -alias ca-cert -import -file ca-cert -storepass serversecret -keypass serversecret

**2. Create Keystore** <br />

keytool -genkey -keystore kafka.broker1.keystore.jks -alias broker1 -validity 3650 -storepass serversecret -keypass serversecret -dname "CN=ec2-13-235-231-26.ap-south-1.compute.amazonaws.com" -ext SAN=dns:ec2-13-235-231-26.ap-south-1.compute.amazonaws.com,dns:ip-172-31-45-22.ap-south-1.compute.internal,ip:13.235.231.26,ip:172.31.45.22 -storetype pkcs12

**3. Create certificate signing request (CSR)** <br />
keytool -keystore kafka.broker1.keystore.jks -alias broker1 -certreq -file ca-request-broker1 -storepass serversecret -keypass serversecret

**4. Sign the CSR** <br />
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-broker1 -out ca-signed-broker1 -days 3650 -CAcreateserial -passin pass:serversecret

**5. Import the CA into Keystore** <br />
keytool -keystore kafka.broker1.keystore.jks -alias ca-cert -import -file ca-cert -storepass serversecret -keypass serversecret

**6. Import the signed certificate from step 5 into Keystore** <br />
keytool -keystore kafka.broker1.keystore.jks -alias broker1 -import -file ca-signed-broker1 -storepass serversecret -keypass serversecret

**7. Modify server.properties file** <br />

nano ~/kafka/config/server.properties

[](./server-0.properties)

### Restart the Broker

sudo systemctl restart kafka

### Check what inside Key store and Trust Store

keytool -list -v -keystore kafka.broker1.keystore.jks

keytool -list -v -keystore kafka.broker2.truststore.jks

### Check Your Certificate File Contents

keytool -printcert -v -file ca-cert




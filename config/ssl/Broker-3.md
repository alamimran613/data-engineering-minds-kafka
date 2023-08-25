## Configure Zookeeper To Broker Security SSL

## 1. Copy ca-cert to Broker 3

mkdir ~/ssl
cd ~/ssl

**1. Create Truststore** <br />
keytool -keystore kafka.broker3.truststore.jks -alias ca-cert -import -file ca-cert

**2. Create Keystore** <br />
keytool -keystore kafka.broker3.keystore.jks -alias broker3 -validity 3650 -genkey -keyalg RSA -ext SAN=dns:<enter your hostname/dns here>
**or**

**For Private and Public DNS use below command**
keytool -keystore kafka.broker3.keystore.jks -alias broker3 -validity 3650 -genkey -keyalg RSA -ext SAN=dns:ec2-13-233-170-112.ap-south-1.compute.amazonaws.com,dns:ip-172-31-38-217.ap-south-1.compute.internal

### When prompt <What is your first and last name?> Then enter your <hostname/dns> here that provide below**

ec2-13-233-170-112.ap-south-1.compute.amazonaws.com


**3. Create certificate signing request (CSR)** <br />
keytool -keystore kafka.broker3.keystore.jks -alias broker3 -certreq -file ca-request-broker3

**4. Sign the CSR** <br />
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-broker3 -out ca-signed-broker3 -days 3650 -CAcreateserial

**5. Import the CA into Keystore** <br />
keytool -keystore kafka.broker3.keystore.jks -alias ca-cert -import -file ca-cert

**6. Import the signed certificate from step 5 into Keystore** <br />
keytool -keystore kafka.broker3.keystore.jks -alias broker3 -import -file ca-signed-broker3

**7. Modify server.properties file** <br />

nano ~/kafka/config/server.properties

[](./server-2.properties)

### Restart the Broker

sudo systemctl restart kafka

### Check what inside Key store and Trust Store

keytool -list -v -keystore kafka.broker1.keystore.jks -storepass serversecret

keytool -list -v -keystore kafka.broker2.truststore.jks -storepass serversecret

### Check Your Certificate File Contents

keytool -printcert -v -file ca-cert


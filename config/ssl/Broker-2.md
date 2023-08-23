## Configure Zookeeper To Broker Security SSL

## 1. Copy ca-cert to Broker 2

mkdir ~/ssl
cd ~/ssl

**1. Create Truststore** <br />
keytool -keystore kafka.broker2.truststore.jks -alias ca-cert -import -file ca-cert

**2. Create Keystore** <br />
keytool -keystore kafka.broker2.keystore.jks -alias broker2 -validity 3650 -genkey -keyalg RSA -ext SAN=dns:<enter your hostname/dns here>

### When prompt <What is your first and last name?> Then enter your <hostname/dns> here that you use above**

**3. Create certificate signing request (CSR)** <br />
keytool -keystore kafka.broker2.keystore.jks -alias broker2 -certreq -file ca-request-broker2

**4. Sign the CSR** <br />
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-broker2 -out ca-signed-broker2 -days 3650 -CAcreateserial

**5. Import the CA into Keystore** <br />
keytool -keystore kafka.broker2.keystore.jks -alias ca-cert -import -file ca-cert

**6. Import the signed certificate from step 5 into Keystore** <br />
keytool -keystore kafka.broker2.keystore.jks -alias broker2 -import -file ca-signed-broker2

**7. Modify server.properties file** <br />

nano ~/kafka/config/server.properties

[](./server-1.properties)

### Restart the Broker

sudo systemctl restart kafka

### Check what inside Key store and Trust Store

keytool -list -v -keystore kafka.broker1.keystore.jks

keytool -list -v -keystore kafka.broker2.truststore.jks

### Check Your Certificate File Contents

keytool -printcert -v -file ca-cert


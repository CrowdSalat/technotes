# Kafka security

## on-prem

### create trust- and keystores

```shell
#!/bin/bash

validity="354"
keystore-pass="testtest"
key-pass="test"
distinguished-name="CN=myhost.de,OU=,DC=de"
hostname="myhost"
ca-passwort="testtest"

# create keystore for a broker
keytool -keystore kafka.server.keystore.jks -alias localhost -keyalg RSA -validity {validity} -genkey -storepass {keystore-pass} -keypass {key-pass} -dname {distinguished-name} -ext SAN=DNS:{hostname}

# private ca key
openssl req -new -x509 -keyout ca-key -out ca-cert -days {validity}

# create a client truststore and  add ca cert to it
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert

# create a server truststore and add a ca cert to it
keytool -keystore kafka.server.truststore.jks -alias CARoot -importcert -file ca-cert

# export the certificate from the boker keystore
keytool -keystore kafka.server.keystore.jks -alias localhost -certreq -file cert-file

# sign the exportet certificate with the ca key
openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days {validity} -CAcreateserial -passin pass:{ca-password}

# import the signed certificate and the ca cert to the broker keystore
keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert
keytool -keystore kafka.server.keystore.jks -alias localhost -import -file cert-signed
```

### encyption (TLS) and authentification (via mTLS)

You can encrypt the traffic between brokers and the traffic between brokers and clients with TLS. If you want to use ACLs on topics you also can configure authentification via mTLS. Every participant needs to have a public/private keypair (keystore) and a list of trustet partners (truststore which holds root certificate). To configure a kafka broker follow the steps in [this manual](https://docs.confluent.io/current/kafka/encryption.html). By default hostname verification (`ssl.endpoint.identification.algorithm`) is enabled, so clients (brokers/producer/consumer) will check if the fully qualified domain name (FQDN) of the server (broker) equals the common name (CN) or the subject alternative name (SAN) of the server (broker) certificate.

```properties
# listenerName:endpoint;
listener=INTERNAL:localhost:9092
# the listeners which get send to zookeeper and the clients. if not set will equal to listener
# advertised.listener=

# The first is the listener name (e.g. security) the second is the protocol (PLAINTEXT, SSL, ...)
listener.security.protocol.map=
```

If SSL protocol is activated for one listener you must locate the private/public key pair and you should set a accepted TLS Version. This can be achieved for all listeners with the ssl.\* properties. If you want to adress a specific listener you need to prefix the properties with the listener.name.<listener_name> e.g. listener.name.internal.ssl.\*.

```properties
# set tls version
ssl.enabled.protocols=TLSv1.3
# locate private/public key for broker
ssl.keystore.location=
ssl.keystore.password=
ssl.key.password=
```

If you want to activate **encryption between the borkers** you need to activate it and trust the certificates/private keys of the other brokers:

```properties
# activate inter encryption
security.inter.broker.protocol=SSL
# add the certificate of the root ca to the truststore (or each broker certificate if there is no common ca) 
ssl.truststore.location=
ssl.truststore.password=

```

If a **client needs to encrypt the connection to kafka** via TLS a client need to activate encryption and trust the certificates/private keys of the brokers:

```properties
# activate encryption
security.protocol=SSL

# add brokers certificate to truststore
ssl.truststore.location=
ssl.truststore.password=

# to deactivate host name validation of the server certificate set it to blank
# ssl.endpoint.identification.algorithm=
```

If a **client should authentificate via mTLS** you need to set `ssl.client.auth=requested` or `ssl.client.auth=required` in the broker settings. Additionally to the encryption properties the client must configure a private/public key pair (which is trusted by the broker).

```properties
# acitvate encryption
security.protocol=SSL

# add brokers certificate to truststore
ssl.truststore.location=
ssl.truststore.password=

# add your own private/public keypair to the keystore (needs to be trusted by brokers)
ssl.keystore.location=
ssl.keystore.password=
ssl.key.password=

# to deactivate host name validation of the server certificate set it to blank
# ssl.endpoint.identification.algorithm=
```

After version 2.7 you can **use inline PEMs** for trusted certificates and private/public key/certificate:

```properties
# encryption
security.protocol=SSL

# truststore replacement
ssl.truststore.type=PEM
ssl.truststore.certificates=-----BEGIN CERTIFICATE----- \
<SECRET_HERE> \
-----END CERTIFICATE-----

# keystore replacement
ssl.keystore.type=PEM
ssl.keystore.key=
ssl.keystore.certificate.chain= 
```

### ACLs (autorization)

With ACLs you can permit [operations](https://docs.confluent.io/current/kafka/authorization.html#operations) on resources (topic, Cluster, ..) Follow the [instruction link](https://docs.confluent.io/current/kafka/authorization.html) to setup ACLs, but read the following keypoints and pitfalls before you do it:

- [Define a Authorizer for the broker: `authorizer.class.name=kafka.security.authorizer.AclAuthorizer`](https://docs.confluent.io/current/kafka/authorization.html#authorizer)
- When a client connects via mTLS the principal will be the certificate 'subject'/'distinguished name'. [If you want to use only the a part of it you need to configure the `ssl.principal.mapping.rules`](https://docs.confluent.io/current/kafka/authorization.html#tls-ssl-principal-user-names)
- A valid `ssl.principal.mapping.rules` to extract the CN from a cert is: `RULE: ^.*[Cc][Nn]=(([a-zA-Z0-9\.\-\-]*).*$/$1/L), DEFAULT`.*The leading `^.*` and the trailing `.*$` seems to be needed. Do not forget to append th DEFAULT rule after a comma at the end*
- If you want to debug if you Authorizer and your mapping rule work you should look into the `kafka-authorizer.log`. You may need to set the log level of the authorizer to DEBUG in the [log4j.xml](./config/log4j.xml) file.
- Use `allow.everyone.if.no.acl.found=true` when you want to dry run your setup.
- when defining a `super.user` you might want to set the whole 'distinguished name' as well as the CN in case your `ssl.principal.mapping.rules` does not work: `super.user=User:CN=bla,OU=bla;User:bla`. The `User` and the `;` enclose one entry.
- To add ACLs you can use the [kafka-acl](https://kafka.apache.org/documentation/#security_authz_cli) command. If an authorizer is active you will also need an certificate for the admin user or the use you want to use when creating ACLs
- If you setup an authorizer you need to allow the broker of your cluster to operate on reosurce 'Cluster' or it may not function as expected
- Information on ACLs is stored in zookeeper (so you might want to activate [ZooKeepers security](https://kafka.apache.org/documentation/#zk_authz) as well)

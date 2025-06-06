# Certificates and encryption

## certificate expiration

- validity period: when this period ends the certificate and all derived certs are expired 
- renewal period: period at the end of the validity period where the old certificate is replaced by a new one with a new validity period. The private key stays unchanged. 
- renewal (in contrast to replacing) keeps the private key of a certificate and only recreate the public certificate with a new validity period


## wildcard certificate 

- star works only for one subdomain.

## file types

- .pem: 
    - contains as first line: `-----BEGIN CERTIFICATE-----`
    - can be a private key or a public certificate
    - can be read with text editor but is base64 encoded
    - or with: `openssl x509 -in cert.pem -text`
- .der (.cer) binary encoded x509 certificate
- .key private key in PEM format
- .cert;.cer;.crt: certificate with private key (DER or PEM)
- .p12 - pkcs12 container which contains private key and public certificate
- .pfx - pkcs12
- .csr (certificate signing request): temporary 

### more on pem

A PEM file looks like this:

```
-----BEGIN [TYPE OF DATA]-----
[Base64 encoded binary data]
-----END [TYPE OF DATA]-----
```

The [TYPE OF DATA] part indicates the format of the binary data within the base64 block. This is where PKCS#1, PKCS#8, and OpenSSH formats come into play for private keys.

- PKCS#1 PEM Format (-----BEGIN RSA PRIVATE KEY-----) - : PKCS#1 (Public-Key Cryptography Standards #1) specifically defines the syntax for RSA public and private keys
- PKCS#8 PEM Format (-----BEGIN PRIVATE KEY----- or -----BEGIN ENCRYPTED PRIVATE KEY-----) -  PKCS#8 (Private-Key Information Syntax Specification) is a more generic standard for storing private keys for any public-key algorithm, not just RSA.
- OpenSSH Private Key Format (-----BEGIN OPENSSH PRIVATE KEY-----) - This is a custom, OpenSSH-specific format, introduced in OpenSSH 7.8 (released in 2017).


## default locations

### Linux

[Source: Where does Go lang search for certificates](https://golang.org/src/crypto/x509/root_linux.go)

- Debian/Ubuntu/Gentoo etc: "/etc/ssl/certs/ca-certificates.crt"
- Fedora/RHEL 6: "/etc/pki/tls/certs/ca-bundle.crt" 
- OpenSUSE: "/etc/ssl/ca-bundle.pem"
- OpenELEC: "/etc/pki/tls/cacert.pem"
- CentOS/RHEL 7: "/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem"
- Alpine Linux: "/etc/ssl/cert.pem"

### CentOS/RHEL 7

[Source](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-shared-system-certificates)

- to add a certficate as ca add the pem or der file to 
  - /etc/pki/ca-trust/source/anchors/ (higher precedence) or
  - /usr/share/pki/ca-trust-source/anchors/ (lower precedence)
  - and run `update-ca-trust` 
- *if you ever feel tempted to edit /etc/pki/ca-trust/extracted/ directly (instead of generating it) double check if you use the right format BEGIN/END CERTIFICATE vs. BEGIN/END TRUSTED CERTIFICATE and the file right name. See the following listing.*

```
# list certs
trust list 

# add pem or der certs to: /etc/pki/ca-trust/source/anchors/ or /usr/share/pki/ca-trust-source/anchors/
update-ca-trust

# see manpage: https://www.unix.com/man-page/centos/8/update-ca-trust/
# updates certs in the following folders: 
## /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
## /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
## /etc/pki/ca-trust/extracted/pem/email-ca-bundle.pem
## /etc/pki/ca-trust/extracted/pem/objsign-ca-bundle.pem
## /etc/pki/ca-trust/extracted/java/cacerts
## and then in /etc/pki/tls/certs/ca-bundle.crt
```

### (open)Suse

- add pem certs to /usr/share/pki/anchors
- run `sudo update-ca-certificates`

## create pem from p12

```shell
PASSWORD="test"

## deprecared: -nodes = "no DES" no password for private keys
## --d - private key will not be encrypted (no password for private keys neeed)

# ca.crt (PEM)
openssl pkcs12 -in keystore.p12 -nodes -out out/ca.crt -cacerts -nokeys -passin pass:$PASSWORD

# tls.crt (PEM)
openssl pkcs12 -in keystore.p12 -nodes -out out/tls.crt -nokeys -passin pass:$PASSWORD
  
# tls.key (PEM)
openssl pkcs12 -in keystore.p12 -nodes -out out/tls.key -nocerts -passin pass:$PASSWORD
```

## generate certificate and private key in pem format (self signed)

```shell
# create 
## with subject prompt
openssl req -x509 -newkey rsa:4096 -keyout ca.demo.key -out ca.demo.crt 
## without subject prompt
openssl req -x509 -newkey rsa:4096 -keyout ca.demo.key -out ca.demo.crt -subj "/CN=ca.demo.de"
## without encrypted private key and without subject prompt
openssl req --nodes -x509 -newkey rsa:4096 -keyout my.key -out my.crt -subj "/CN=ca.demo.de"

# read 
openssl x509 -in  my.crt -text -noout
openssl rsa -in my.crt -text -noout
```

## generate certificate and private key in pem format (ca signed)

```shell
# generate ca key and cert in pem format (or use existing CA key and cert)
openssl req -x509 -newkey rsa:4096 -keyout ca.demo.key -out ca.demo.crt -subj "/CN=ca.demo.de"

# create key for client
openssl genrsa -out client.demo.key 2048
# create signing requets for a client (unsigned certificate)
openssl req -new -key client.demo.key -subj "/CN=client.demo.de" -out client.demo.csr
# create signed client certificate
openssl x509 -req -in client.demo.csr -signkey client.demo.key -out client.demo.crt
```

## download leaf cert from url
 
HOSTNAME=
PORT=
openssl s_client -showcerts -connect ${HOSTNAME}:${PORT} </dev/null 2>/dev/null|openssl x509 -outform PEM >leafcert-${HOSTNAME}.pem

## pem headers examples

```shell
-----BEGIN CERTIFICATE-----
-----BEGIN CERTIFICATE REQUEST-----
-----BEGIN RSA PRIVATE KEY-----
-----BEGIN ENCRYPTED PRIVATE KEY-----
```

### ssh key

To generate ssh keys run `ssh-keygen` which generates ~/.ssh/id_rsa.pub and ~/.ssh/id_rsa. The private and public key do no use the pem format. The name of the owner is at the end of the pub file.

## Java truststore and keystore

- A Java keystore holds your private Keys + and your certificate
- A Java truststore holds the certificates you trust when you connect yourself to a server   
- You may need to assure that the truststore contains the whole trust-chain otherwise java may not accept a connection to the server  
- .fileending (Format): .jks (JSK), .keystore (JSK), .p12 (PKCS12)
- pkcs12 is the recommended format for keystores, not jks
- Javas bundled truststore resides in `$JAVA_HOME/jre/lib/security/jssecacerts` and `$JAVA_HOME/jre/lib/security/cacerts`
- java Properties 
  - `javax.net.ssl.keyStore*` 
  - `javax.net.ssl.trustStore*` 
  - `javax.net.debug` to activate debugging
  - `javax.net.ssl.*trust*StoreType` from java 9 onwards keyStoreType is pkcs12. Before it was jks.

```shell
# assumes that .crt and .key already exist

## 1. create pem with full chain of trust
cat  my.crt > full-chain.crt

## 2. create Keystore in .p12 format from .key and .crt. To pass prompt use -passin -passout
### you may want to set keystorepassword=privatekeypassword because some libraries assume that
openssl pkcs12 -export -in full-chain.crt -inkey my.key -out keystore.p12

## 3. create Truststore (add .crt) - which trust the cert and use changeit as password for the truststore
openssl pkcs12 -export -nokeys -in full-chain.crt -out truststore.p12

## (not recommended) keystore as jks. convert p12 to jks
keytool -importkeystore -srckeystore keystore.p12 -destkeystore keystore.jks -srcstoretype pkcs12 -deststoretype jks

## (not recommended) truststore as jks 
keytool -keystore truststore.jks -alias full-chain.crt -import -file full-chain.crt -noprompt -storepass changeit


# read p12 truststore
PASSWORD="bla"
openssl pkcs12 -info -in truststore.p12 -passin pass:$PASSWORD

# read p12 keystore 
# if -nodes is not set openssl will prompt "Enter PEM pass phrase" which will then used to encryp the private key
openssl pkcs12 -info -in keystore.p12 -passin pass:$PASSWORD -nodes
```

## create full chain pem public cert

```
cat cert.pem intermediate-ca.pem root-ca.pem cert-fullchain.pem
```

Or while exporting the cert with chrome (or other browser) choose as filetype certficate-chain instead of certificat

## tools

### mkcert

[mkcert](https://github.com/FiloSottile/mkcert)

### vscode opensslutils

[opensslutils](https://marketplace.visualstudio.com/items?itemName=ffaraone.opensslutils)

## trusted certficcate format

There is a difference between:

- BEGIN/END CERTIFICATE 
- BEGIN/END TRUSTED CERTIFICATE

see [manpage of x509](https://www.unix.com/man-page/centos/1/x509/) for further information.

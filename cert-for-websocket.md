# Overview

This article describe to generate certificate and use it web server/client.

## Create certificates and keys

Several files will be created during the certificate generation process, some with .crt and .key suffices. You may also see the suffix .pem when reading about certificates. It’s worth noting that .pem files are equivalent to .crt and .key files. PEM is a file format. .crt and .key are hints as to what the file contains (certificates and keys), but these files all use the PEM format.

## Install software to create the CA and certificate(s)

Install [certstrap](https://github.com/square/certstrap) to create certificate. There are several other alternative but I like to use this.

I keep all my certificates in a directory called **~/certs**. All the following commands will be run from that directory. When prompted for the passphrase just hit enter (i.e., no passphrase).

## Create the CA

```
manoj@ubuntu:~/certs$ certstrap init --common-name "LocalCA"
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Created out/LocalCA.key
Created out/LocalCA.crt
Created out/LocalCA.crl
```

```init``` directs ```certstrap``` to create a new CA. ```--common-name``` (CN) specifies the name for the our CA, which is named ```LocalCA```.
* LocalCA.key is the private key for LocalCA
* LocalCA.crt is the certificate for LocalCA
* LocalCA.crl is the certificate revocation list (CRL) for that CA. It contains a list of revoked certificates issued by the associated CA.

## Create the Server certificate

```
manoj@ubuntu:~/certs$ certstrap request-cert --domain  "manoj.local.net"
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Created out/manoj.local.net.key
Created out/manoj.local.net.csr
```

```manoj.local.net``` is used as the domain for the server since a valid FQDN of the host is required for servers.

## Sign the Server certificate

```
manoj@ubuntu:~/certs$ certstrap sign manoj.local.net --CA LocalCA
Created out/manoj.local.net.crt from out/manoj.local.net.csr signed by out/LocalCA.key
```

Certificates must be signed by a trusted authority, in this case the CA, in order to be valid. Signing is a guarantee by the CA that the owner of the certificate is who they say they are. The ```--CA``` flag above directs ```certstrap``` to have the certificates signed by our ```LocalCA```.

```
manoj@ubuntu:~# cd certs/
manoj@ubuntu:~# certstrap init --common-name "LocalCA"

manoj@ubuntu:~# certstrap request-cert --domain  "manoj.local.net"
manoj@ubuntu:~# certstrap sign manoj.local.net --CA LocalCA

scp out/LocalCA.crt manoj@192.168.1.7:/Users/manoj/certs
```

## View Certificates

use openssl command to view the generate certificates

```
openssl x509 -in manoj.local.net.crt -text -noout
openssl x509 -in manoj.local.net.key -text -noout
openssl x509 -in LocalCA.crt -text -noout
```

## Certificate Usage:

### Server
Start server by passing server certificate and key to server

```
sudo ./server -host "manoj@local.net" -cert /home/manoj/certs/out/manoj.local.net.crt -key /home/manoj/ep_certs/out/manoj.local.net.key
```

### Client
Use the CA certificate in client

```
./client  -cacert ../certs/out/LocalCA.crt
```

# References:
* [Create Secure Clients and Servers in Golang Using HTTPS](https://youngkin.github.io/post/gohttpsclientserver/)
* [Cheat Sheet - OpenSSL](https://megamorf.gitlab.io/cheat-sheets/openssl/)
* [What is Server Name Indication (SNI)](https://www.globalsign.com/en/blog/what-is-server-name-indication)
* [How SSL certificate works?](https://www.youtube.com/watch?v=33VYnE7Bzpk)

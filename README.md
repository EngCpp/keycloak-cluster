# Keycloak Cluster
The goal of this repo is to demonstrate how we can have fault tolerant, scalable Keycloak application
and how it operates in a cluster.


### What is Keycloak ?
Keycloak is an open source identity and access management tool,
it adds authentication to applications and secure services with minimum effort.
No need to deal with storing users or authenticating users.
Keycloak provides user federation, strong authentication, user management, fine-grained authorization, and more.

*For more visite:*
[website](https://www.keycloak.org)

### Configurations
**.env** file is where the most sensitive information is stored, here we keep environmental variables
that should be injected by a safe tool like vault or jenkins credentials.

### Generating a localhost (self signed certificate) for Keycloak nodes
To be able to expose a HTTPS (encrypting the data being transmitted over http protocol) a certificate is required.
Use the following line to create a keystore for the localhost domain.
The Keystore will later on be injected inside of the container for keycloak to use.
Please use the DNS Name for each node in case you are running it on your a real network.

```bash
keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore
```

### Creating a self signed certificate for NGINX ( proxy / loadbalancer )
```bash
openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out MyCert.crt -keyout MyKey.key
```
![Creating SSL Cert](/images/openssl.png "Creating SSL Certificate")

### For this example to work you may need to add a fake dns name to your hosts file (if you are using linux)
go to /etc/hosts and add the line below
```bash
127.0.0.1       keycloak.cluster.com
```
This will enable the hostname **keycloak.cluster.com** so your browser will convert it to 127.0.0.1 which is localhost.


### Docker Compose
Use ***docker compose*** to ***start*** and ***stop*** the servers

*to start up the servers use:*
```bash
docker-compose up
```

*To stop the servers use:*
```bash
docker-compose down
```

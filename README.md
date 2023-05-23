# fleetdm

## Usage

`docker-compose up -d`

`docker-compose down`

## Notes

Cert creation
```
# create environments
mkdir ssl
cd ssl
mkdir key csr crt

# create a key
openssl genrsa -des3 -out key/server.key 2048

# create a request
openssl req -new -key key/server.key -out csr/server.csr

# reset the passphrase form server.key
cp key/server.key key/server.key.org

openssl rsa -in key/server.key.org -out key/server.key

rm -rf key/server.key.org

# create config file to SAN
touch v3.ext

# config file v3.ext content
---
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer:always
basicConstraints       = CA:TRUE
keyUsage               = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment, keyAgreement, keyCertSign
subjectAltName         = DNS:example.com, DNS:*.example.com
issuerAltName          = issuer:copy
---

# create a certificate
openssl x509 -req -in csr/server.csr -signkey key/server.key -out crt/server.crt -days 3650 -sha256 -extfile v3.ext
```

Manually add vulnerability databases to FleetDM
```
# fall into a container
docker exec -ti fleetdm sh

# add databases
fleetctl vulnerability-data-stream --dir /tmp/vulndbs
```
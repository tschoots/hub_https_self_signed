# hub_https_self_signed




# how to run hub with self signed certificates

http://www.akadia.com/services/ssh_test_certificate.html


# login the hub server as blckdck
# create keystore direcory
mkdir /opt/blackduck/keystore

cd /opt/blackduck/keystore

# Generate a private key
openssl genrsa -des3 -out server.key 1024

# generate Certificate Signing Request
openssl req -new -key server.key -out server.csr

# since we do self signed no need to send the request to a CA
# so generate a self signed certificate
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

# now create a pkcs12 file
openssl  pkcs12 -keypbe PBE-SHA1-3DES -certpbe PBE-SHA1-3DES -export -in server.crt -inkey server.key -out server.pkcs12 -name hub_key

# now we are going to change the settings
cd /opt/blackduck/hub/appmgr/bin

./changeHubHttpSettings.sh

# the following answers to the questions if different then default there here below
Use HTTPS y
port 8443Redirect Port 443 to 8443?  [y, n]  y

Absolute path to Keystore File: /opt/blackduck/keystore/server.pkcs12

# server will stop and start



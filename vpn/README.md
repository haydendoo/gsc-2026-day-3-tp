# Generating self-signed certificate
```bash
# Create CA private key
openssl genrsa -out ca.key 4096

# Create CA self-signed certificate
openssl req -x509 -new -nodes \
  -key ca.key \
  -sha256 \
  -days 3650 \
  -out ca.crt \
  -subj "/CN=MyClientVPN-CA"

# Server private key
openssl genrsa -out server.key 2048

# CSR
openssl req -new \
  -key server.key \
  -out server.csr \
  -subj "/CN=clientvpn.server"

# Sign server cert using CA
openssl x509 -req \
  -in server.csr \
  -CA ca.crt \
  -CAkey ca.key \
  -CAcreateserial \
  -out server.crt \
  -days 365 \
  -sha256

aws acm import-certificate \
  --certificate fileb://server.crt \
  --private-key fileb://server.key \
  --certificate-chain fileb://ca.crt \
  --region ap-southeast-1

# Client private key
openssl genrsa -out client1.key 2048

# CSR
openssl req -new \
  -key client1.key \
  -out client1.csr \
  -subj "/CN=client1"

# Sign using CA
openssl x509 -req \
  -in client1.csr \
  -CA ca.crt \
  -CAkey ca.key \
  -CAcreateserial \
  -out client1.crt \
  -days 365 \
  -sha256
```

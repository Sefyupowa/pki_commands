# Generate a CSR with OpenSSL

This procedure explains how to generate a private key and CSR with OpenSSL.

## Prerequisites

- OpenSSL installed and available in your terminal.
- Server DNS names to include in CN and SAN.
- Permissions to store the private key securely.

## 1. Verify OpenSSL

```bash
openssl version
```

## 2. Create a working folder


```bash
mkdir -p ./csr_work
cd ./csr_work
```

## 3. Create an OpenSSL request config file

Create `server_cert.cnf` with your values:

```ini
[req]
default_bits       = 2048
distinguished_name = req_distinguished_name
req_extensions = req_ext
prompt = no

[req_distinguished_name]
C = FR
ST = Paris
L = Seine saint denis
O = AXA
0.OU = IAM
1.OU = PKI
CN = server.fqdn.example.com

[req_ext]
subjectAltName = @alt_names

[alt_names]
DNS.1 = server.fqdn.example.com
DNS.2 = servername
DNS.3 = server.alias.example.com
# IP.1 = 10.1.1.5
# email.1 = admin@example.com
```

## 4. Generate the private key and CSR in one command

```bash
openssl req -new  -nodes -keyout server.key -out server.csr -config server_cert.cnf
```

## 5. Validate the CSR

```bash
openssl req -in server.csr -noout -text
```

Verify:

- Subject contains the expected CN.
- X509v3 Subject Alternative Name contains all required DNS and IP entries.

## 6. Files produced

- `server.key`: private key. Keep this secret.
- `server.csr`: send this CSR to your Certificate Authority.
- `server_cert.cnf`: OpenSSL request configuration used for CSR generation.


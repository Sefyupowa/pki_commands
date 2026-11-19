# Extract Certificate and Private Key from PKCS#12 (Linux)

Use these commands with your PFX/P12 file (example: server.pfx).

## 1. Extract private key

```bash
openssl pkcs12 -in server.pfx -nocerts -nodes -out private.key
```

## 2. Extract certificate

```bash
openssl pkcs12 -in server.pfx -clcerts -nokeys -out server.crt
```

## 3. Extract CA bundle (chain)

```bash
openssl pkcs12 -in server.pfx -cacerts -nokeys -out ca_bundle.crt
```

## 4. Quick verification

```bash
openssl pkey -in private.key -check -noout
openssl x509 -in server.crt -noout -subject -issuer -dates
openssl crl2pkcs7 -nocrl -certfile ca_bundle.crt | openssl pkcs7 -print_certs -noout
```

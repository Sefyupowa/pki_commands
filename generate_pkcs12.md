# Generate CSR from Existing Certificate and Private Key (Linux)

Use this when the certificate is already generated and you still have the matching private key.

## 1. Verify key matches certificate

```bash
openssl x509 -noout -modulus -in server.crt | sha1sum
openssl rsa  -noout -modulus -in server.key | sha1sum
```

The two hashes must be identical.

## 2. Generate PFX (PKCS#12) from certificate and private key

Without CA chain:

```bash
openssl pkcs12 -export \
	-out server.pfx \
	-inkey server.key \
	-in server.crt
```

With CA chain (recommended if you have intermediate/root bundle):

```bash
openssl pkcs12 -export \
	-out server.pfx \
	-inkey server.key \
	-in server.crt \
	-certfile ca_bundle.crt
```

You will be prompted to set the PFX password.

## 3. Verify PFX content

```bash
openssl pkcs12 -info -in server.pfx -noout
```

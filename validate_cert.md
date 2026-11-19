# Validate a Certificate with CRL (Linux)

This procedure checks certificate details, finds CRL URLs, downloads the CRL, and validates the certificate with OpenSSL.

## 1. Show basic certificate details

```bash
openssl x509 -in server.crt -noout -subject -issuer -dates -serial
```

## 2. Check CRL Distribution Points (with grep)

```bash
openssl x509 -in server.crt -noout -text | grep -A3 "CRL Distribution Points"
```

If output is empty, the certificate does not publish a CRL URL.

## 3. Extract the CRL URL

```bash
CRL_URL=$(openssl x509 -in server.crt -noout -text \
	| grep -A3 "CRL Distribution Points" \
	| grep -Eo 'http://[^ ]+' \
	| head -n 1)

echo "$CRL_URL"
```

## 4. Download the CRL

```bash
curl -fsSL "$CRL_URL" -o issuer.crl
```

If the URL is LDAP, download from HTTP mirror or PKI portal instead.

## 5. Build a CA bundle for verification

```bash
cat intermediate_ca.crt root_ca.crt > ca_chain.crt
```

## 6. Verify certificate against chain + CRL

```bash
openssl verify -crl_check -CAfile ca_chain.crt -CRLfile issuer.crl server.crt
```

Expected result:

```text
server.crt: OK
```

If revoked, OpenSSL returns an error such as `certificate revoked`.

# L2 TLS Quick Checklist

Use this when users report HTTPS or certificate errors.



## 1. Check certificate from client side

Run:

```bash
openssl s_client -connect <host>:<port> -verify 10  -showcerts < /dev/null 

openssl s_client -connect <host>:<port> -verify 10  -showcerts < /dev/null | openssl x509 -text -noout

# Linux: quickly print SAN values from the leaf certificate
openssl s_client -connect celia-pp.corp.intraxa:443  < /dev/null 2>/dev/null | openssl x509 -noout -ext subjectAltName
```

Check these points:

- Certificate is not expired.
- Hostname is present in certificate names.
- More than one certificate is shown when a chain is expected.

## 2. If you have CA bundle, verify trust

```bash
openssl s_client -connect <host>:<port> -verify 10  -CAfile ca_bundle.crt -showcerts < /dev/null
```

Expected result: `Verify return code: 0`.

If you see code `20` or `21`, chain is likely incomplete or CA is missing.

## 3. Most common causes

- Expired certificate.
- Wrong hostname in certificate.
- Missing intermediate certificate.
- Certificate not trusted by client.
- Recent certificate change not fully deployed.

## 4. What to send to L3/PKI if still failing

- Hostname and port.
- Time of test.
- Full `openssl s_client` output.
- Error message from application/user.
- Note if issue is global or limited to some clients.
Title: Enable TLS
Description: How to enable DapperDox HTTPS
Keywords: TLS, HTTPS, Cipher, certificate, private key, SSL

# Enable TLS

DapperDox's implementation of TLS restricts itself to the strong TLS 1.2 suites, avoiding
RC4 and BEAST vulnerabilities. DapperDox is therefore compatible with current releases of
Google Chrome, Apple Safari, Mozilla Firefox and Internet Explorer.

DapperDox requires a certificate and a private key to be configured to enable TLS. The
certificate and key filenames are passed to DapperDox using the `-tls-certificate` and
`-tls-key` [configuration](/docs/configuration-guide) options.



## Supported cipher suites

DapperDox supports the following strong TLS 1.2 cipher suites:

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_256_CBC_SHA

## Generating self signed certificates

If you intend to use HTTPS, then for testing and internal use, you can generate your own
self-signed certificate:

### RSA 2048 bit

To generate a self-signed RSA certificate and 2048 bit key, use the following `openssl` command:

```
openssl req -x509 -nodes -newkey rsa:2048 -keyout dapperdox_rsa.key -out dapperdox_rsa.crt -days 3650
```

Configure DapperDox to read the certificate and key with:

```
-tls-cert=dapperdox_rsa.crt \
-tls-key=dapperdox_rsa.key
```


### ECDSA secp384r1

To generate a self-signed ECDSA certifiacte and secp384 key, use the following `openssl` command:

```
openssl req -x509 -nodes -newkey ec:secp384r1 -keyout dapperdox_ecdsa.key -out dapperdox_ecdsa.crt -days 3650
```

Configure DapperDox to read the certificate and key with:

```
-tls-cert=dapperdox_ecdsa.crt \
-tls-key=dapperdox_ecdsa.key
```


## Production certificates

For production use the certificate must be a chain, so you will likely need to 
concatenate your certificate authority issued certificate, the issuers intermediate
certificate and their root authority certificate together into one combined certificate file:

```
cat your_cert.pem intermediate.ca.pem ca.pem > dapperdox_production.crt
```

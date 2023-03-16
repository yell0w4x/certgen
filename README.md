# Generates Self Signed Certificate with Custom Root CA 

```
./certgen --help
Generates self-signed root CA certificate and certificate for the specified domain.

Usage:
    ./certgen [OPTIONS] DOMAIN

Arguments:
    DOMAIN Domain name to generate certificate for. 
           Override the CN parameter in --subj if specified.
           Use *.example.com format for wildcard certificate.

Options:
    -a, --ip        Host ip address (default: 127.0.0.1)
    -s, --subj      OpenSSL subj parameter in format "/C=US/ST=CA/O=MyOrg, Inc./CN=example.com"
                    (default: "/C=US/ST=CA/O=MyOrg, Inc./CN=${DOMAIN}")
    -d, --out-dir   Output directory. If doesn't exist it's created (default: /home/q/work/certgen)
    --days          Number of days generated certificates are valid (default: 3650)
    -h, --help      Show this message and exit
```

The following creates certificate files in `example.com` subdirectory.

```
./certgen -d example.com example.com && ls example.com
Certificate request self-signature ok
subject=C = US, ST = CA, O = "MyOrg, Inc.", CN = My CA
Certificate request self-signature ok
subject=C = US, ST = CA, O = "MyOrg, Inc.", CN = example.com
example.com.crt  example.com.key          root-ca-example.com.csr  root-ca-example.com.srl
example.com.csr  root-ca-example.com.crt  root-ca-example.com.key
```

Then depending on your system put the root ca certificate to local store. 
E.g. for Arch based distros the line is as follows.

    sudo trust anchor --store root-ca-example.com.crt

For Ubuntu copy the certificate to the `/usr/local/share/ca-certificates` directory (mkdir if needed).
Run `update-ca-certificates` as root.

Use `example.com.crt` and `example.com.key` in nginx ssl config. 
Now browsers stop yelling on certificate.




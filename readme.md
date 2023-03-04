# Make a Wildcard Certificate:
[Vice Documentation](https://api-ipv4.us-or.viasat.io/PKI.html)

## Create a x509 Cert
```
# Creating a Private Key
openssl genrsa -out private.key 4096

# Make a config File
server.config

# Put the following Contents into the config file
[req]
default_bits        = 2048
req_extensions      = v3_req
distinguished_name  = dn
prompt              = no
 
 
[v3_req]
subjectAltName      = @alt_dns
 
[alt_dns]
DNS.1=*.baps.viasat.io  ## <- YOUR DNS HERE
 
[dn]
CN=*.baps.viasat.io ## <- YOUR CN HERE
O=VIASAT, INC
OU=BAPS
L=Carlsbad
C=US
ST=CA


# Generate the CSR
openssl req -new -out cert_name.csr -key private.key -sha256 -config server.config
```

## Upload and Signing the Cert
1. Visit the "PKI" Tab and select the stripe
    - [BAPS LINK](https://portal.viasat.io/?stripe=baps&tab=pki&key=baps-intr)
2. Create an Intermediate Key if you don't have one
3. Click the check mark next to you intermediate key then click sign certificate
4. Paste the contents from the .csr file you obtained above (From the x509 cert section)
    - Cert name can be anything you want
5. Click the check mark next to your signed cert and click More actions
6. Click download keyfiles and download your cert file (This is your signed cert)
7. Click "More Actions" under the intermediate key and download both files (Intermediate Cert File and Intermediate Bundle File)
8. Open up your signed cert and copy the content from both the Intermediate cert and Intermediate Bundle files so that the file is formatted in the following order: **Sign Cert Contents | Intermediate Cert Contents | Intermediate Bundle Contents**

**Congrats you now have your cert_chain**

## Using the new cert
1. Navigate to the Certificate section of your application on kubernetes
2. Copy the contents of the chain cert into the cert variable
3. Use the original private key you got from when you created the x509 cert
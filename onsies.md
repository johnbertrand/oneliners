
### grep
ignore empty lines or comments
`grep -v '^\s*\#'`


## openssl
taken from https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs

#### x509 subject to avoid answeing questions
`-subj "/C=US/ST=CA/L=San Francisco/O=SFEMA.net/OU=Operations/CN=www.sfema.net"`

C=countryName
L=locality
O=organization
CN=commonName
OU=OrganizationalUnitName
ST=StateOrProvinceName
DN=DistinguishedNameQualifier

#### Generating a KEY and CSR at the same time
`openssl req   
       -newkey rsa:2048 -nodes -keyout domain.key  
       -out domain.csr`

#### Generate a CSR from an existing key.
`openssl req \
       -key domain.key \
       -new -out domain.csr`


#### Generate a CSR from an Existing Certificate and Private Key
`openssl x509 \
       -in domain.crt \
       -signkey domain.key \
       -x509toreq -out domain.csr`

#### Generate a Self Signed Certificate
`openssl req \
       -newkey rsa:2048 -nodes -keyout domain.key \
       -x509 -days 365 -out domain.crt`

#### Generate a Self-Signed Certificate from an Existing Private Key
`openssl req \
       -key domain.key \
       -new \
       -x509 -days 365 -out domain.crt`

#### Generate a Self-Signed Certificate from an Existing Private Key and CSR
`openssl x509 \
       -signkey domain.key \
       -in domain.csr \
       -req -days 365 -out domain.crt`


### View Certificates
Certificate and CSR files are encoded in PEM format, which is not readily human-readable.

#### View CSR Entries
`openssl req -text -noout -verify -in domain.csr`

#### View Certificate Entries
`openssl x509 -text -noout -in domain.crt`

#### Verify a Certificate was Signed by a CA
`openssl verify -verbose -CAFile ca.crt domain.crt`


### Private Keys
#### Create a Private Key
`openssl genrsa -des3 -out domain.key 2048`

#### Verify a Private Key
`openssl rsa -check -in domain.key`


#### Verify a Private Key Matches a Certificate and CSR
If the output of each command is identical there is an extremely high probability that the private key, certificate, and CSR are related. 

`openssl rsa -noout -modulus -in domain.key | openssl md5
openssl x509 -noout -modulus -in domain.crt | openssl md5
openssl req -noout -modulus -in domain.csr | openssl md5`


#### Encrypt a Private Key
`openssl rsa -des3 \
       -in unencrypted.key \
       -out encrypted.key`


#### Decrypt a Private Key
`openssl rsa \
       -in encrypted.key \
       -out decrypted.key`


### Convert Certificate Formats

#### Convert PEM to DER
`openssl x509 \
       -in domain.crt \
       -outform der -out domain.der`


### Get Version
`openssl version -a`
	


### PLOS
#This one pulls a random PLOS article and dumps the text good to use for the -program argument on your screen saver
curl -s "http://www.plosone.org/article/fetchObject.action?representation=PDF&uri=$(lynx -dump 
www.plosone.org | grep pone\\. | awk '{ print $2; }' | shuf | head -1 | sed -n -e s'/^.*article\///p')" | pdftotext - -

# creacion de CA local


```
mkdir ca
cd ca
mkdir ca.db.certs
touch ca.db.index
echo "1234" > ca.db.serial

´´´

crear el archivo de configracion de la CA - importante setear default_md = sha256

[ ca ]
default_ca = ca_default
[ ca_default ]
dir = ./ca
certs = $dir
new_certs_dir = $dir/ca.db.certs
database = $dir/ca.db.index
serial = $dir/ca.db.serial
RANDFILE = $dir/ca.db.rand
certificate = $dir/ca.crt
private_key = $dir/ca.key
default_days = 365
default_crl_days = 30
default_md = md5
preserve = no
policy = generic_policy
[ generic_policy ]
countryName = optional
stateOrProvinceName = optional
localityName = optional
organizationName = optional
organizationalUnitName = optional
commonName = optional
emailAddress = optional



Generar llave de 2048 bits

openssl genrsa -des3 -out ca/ca.key 2048

crear el certificado autofirmado X509

openssl req -new -x509 -days 10000 -key ca/ca.key -out ca/ca.crt

es importante que en el archivo openssl.cnf se encuentre habilitada el agloritmo de cifrado sha256

#openssl.cnf (/etc/pki/tls)
default_md      = sha256                # use SHA-256 by default


crear un archivo de configuracion para la creacion del certificado (esto es necesario para que quede el campo SubjectAltNames)

#server.cnf
[req]
default_bits = 2048
prompt = no
default_md = sha256
distinguished_name = dn

[dn]
C=CL
ST=stgo
L=Metro
O=orgnization
OU=Testing Domain
emailAddress=your-administrative-address@your-awesome-existing-domain.com
CN = *.domain.cl

creacion del request.

 openssl req -new -newkey rsa:2048 -nodes -days 8000 -sha256  -keyout mykey.pem -out myreq.pem  -config <( cat server.cnf )

luego es necesario firmar el request con las extenciones necesarias.

#extv3 

authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = *.domian.cl

firma del certificado

openssl ca -config ca.conf  -extfile v3file -days 8000 -out certificate.pem.crt -infiles myreq.pem










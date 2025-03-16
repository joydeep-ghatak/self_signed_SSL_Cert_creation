# 1. create ca.conf file and paste following
```ini
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
        default_md = sha256
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
```

# 2.  Create database and other files with following
```sh
        mkdir ca
        cd ca
        mkdir ca.db.certs
        touch ca.db.index
        echo "1234" > ca.db.serial
```
# 3.  Generate a 2048-bit RSA private key for the CA:
```sh
        openssl genrsa -des3 -out ca/ca.key 2048
```

# 4. Create a self-signed X509 certificate for the CA (the CSR will be signed with it):
```sh
       openssl req -new -x509 -days 10000 -key ca/ca.key -out ca/ca.crt
```
# 5.  Create CSR
```sh
        openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server-csr.pem 
```
# 6.  Sign CSR
```sh
        openssl ca -config ca.conf -out ssl-bundle.crt -infiles server-csr.pem
```


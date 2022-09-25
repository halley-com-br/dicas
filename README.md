# dicas
# Habilite o OpenSSL no "C:\xampp\php\php.ini"

extension=openssl
extension=php_openssl.dll

# Crie o arquivo "v3.ext" dentro do local "C:\xampp\apache", seu conteúdo deve ser:

authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost

# Edite o arquivo MakeCert.bat que está em "C:\xampp\apache\makecert.bat" para

@echo off
set OPENSSL_CONF=./conf/openssl.cnf

if not exist .\conf\ssl.crt mkdir .\conf\ssl.crt
if not exist .\conf\ssl.key mkdir .\conf\ssl.key

bin\openssl req -new -out server.csr
bin\openssl rsa -in privkey.pem -out server.key
bin\openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 365 -sha256 -extfile v3.ext

set OPENSSL_CONF=
del .rnd
del privkey.pem
del server.csr

move /y server.crt .\conf\ssl.crt
move /y server.key .\conf\ssl.key

echo.
echo -----
echo Das Zertifikat wurde erstellt.
echo The certificate was provided.
echo.
pause

# Execute o arquivo MakeCert.bat que está em "C:\xampp\apache\makecert.bat"

Enter PEM pass phrase: 12345
Verifying - Enter PEM pass phrase: 12345
Country Name: BR
Organization Name: Xampp Localhost
Common Name: localhost

# Instale o certificado que está em "C:\xampp\apache\conf\ssl.crt\server.crt"

# Reinicie o Apache do XAMPP

# Se não funcionar, reinicie o computador

# HTTP - HTTPS PROJECT 

This project gives you basic understanding glassfish server integration with self signed certifcate using keystore or openssl.

Either you can get certificates from CA or you can generate your own. Second option : Self signed certificate has been defined :


Please find the steps below:

Make sue you have installed glassfish in eclipse.

1. Dyanmic Web project
2. Glassfish Sever
3. openssl.exe
4. jdk 1.7

keytool you can find in jdk1.7/jre/lib folder. Make sure you add jre path in environment variables in order to execute keytool commands

HTTP - HTTPS PROJECT convert steps.(Using Command line in Windows)
-------------------------------------------------------------------

PORT : 443 for HTTPS
domain - name -: computer-name(use full computer name)

Make sure password will be "changeit" as same as glassfish master password.

Run all commands at location ../glassfish/domains/domain1/config

Generating RSA Private key:
-------------------------------------------

openssl genrsa -des3 -out computer-name.key 2048

Generate csr(Certificate signing request) (use first and last name or Common name as computer-name )
-------------------------

openssl req -new -key computer-name.key -out computer-name.csr

In order to access for multi domains

copy computer-name.key computer-name.key.org
openssl rsa -in computer-name.key.org -out computer-name.key


Generate self signed certificate (.crt) from private key and csr
-------------------------------
openssl x509 -req -days 365 -in computer-name.csr -signkey computer-name.key -out computer-name.crt


generate jks using crt file - (use first and last name or Common name as computer-name )
-----------------------------------------
keytool -genkeypair -alias computer-name -file computer-name.crt -storetype jks -keystore computer-name.jks -validity 366 -keyalg RSA -keysize 2048

copy your keystore to keystore.jks
-----------------------------------------
keytool -importkeystore -srckeystore computer-name.jks -destkeystore keystore.jks


add certificate to trustsdstore :- cacerts.jks
----------------------------------------------------
keytool -import -v -trustcacerts -alias computer-name -file computer-name.crt -keystore cacerts.jks -keypass changeit -storepass changeit


Make changes of port: 443 and server name : computer-name in glassfish admin console:
--------------------------------------------------
http://localhost:4848/common/index.jsf
  
  
  
Now just hit
-----------

https://localhost:8080/http-https-project

it will show output as Hello Glassfish server!!!

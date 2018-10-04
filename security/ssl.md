# SSL related

* KeyStore and TrustStore
  * When Client request Server, Server need to provide evidence to prove itself
  * The evidence server provide to client: `certificate`, `public key`, and also the the `private key` of server(not provide to client) is stored in `KeyStore` in Server side.
  * When client received server's `certificate` and `public key`, client will store them in to `TrustStore`.
  * When you want to store something belong to you, such as `password`, `private key`, `certificate`, it should be stored in `KeyStore`
  * When you want to store the thing received from third party, like `public key` and `certificate`, you should use `TrustStore`
  * The main difference between `KeyStore` and `TrustStore` is the content(file) stored inside it, the structure of `KeyStore` and `TrustStore` is same.

* Using `keytool` to generate `KeyStore` and `TrustStore`
  1. Generate Server Side `private key`, and saved in `Server Side KeyStore`
    * `keytool -genkey -alias server -keyalg RSA -keyStore server1.ks`
    * Will get `server1.ks` which saved the server private key
  2. Using Server Side Private Key export Server Certificate
    * `keytool -export -alias server -keyStore server1.ks -file server_cert`
    * Will get `server_cert`
    * Need to enter the password when we create `server private key`
  3. Import `Server Certificate` into `Client TrustStore`
    * `keytool -import -alias server -keystore client1.ts -file server_cert`
    * Will get `client1.ts`
    * Need to enter the password of `server private key`
  4. Generate Client Side `private key`, and saved in `Client Side KeyStore`
    * `keytool -genkey -alias client -keyalg RSA -keyStore client1.ks`
    * Will get `client1.ks`
  5. Using Client Side Private Key export Client Certificate
    * `keytool -export -alias client -keystore client1.ks -file client_cert`
    * Will get `client_cert`
  6. Import `Client Certificate` into `Server TrustStore`
    * `keytool -import -alias client -keystore server1.ts -file client_cert`
    * Will get `server1.ts`


 * Some Detials

```
>keytool -genkey -alias server -keyalg RSA -keyStore server1.ks
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Yang Wang
What is the name of your organizational unit?
  [Unknown]:  Hortonworks
What is the name of your organization?
  [Unknown]:  PS
What is the name of your City or Locality?
  [Unknown]:  Perth
What is the name of your State or Province?
  [Unknown]:  WA
What is the two-letter country code for this unit?
  [Unknown]:  AU
Is CN=Yang Wang, OU=Hortonworks, O=PS, L=Perth, ST=WA, C=AU correct?
  [no]:  y

Enter key password for <server>
	(RETURN if same as keystore password):
Re-enter new password:

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore server1.ks -destkeystore server1.ks -deststoretype pkcs12".
```


```
>keytool -export -alias server -keyStore server1.ks -file server_cert
Enter keystore password:
Certificate stored in file <server_cert>

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore server1.ks -destkeystore server1.ks -deststoretype pkcs12".
```

```
>keytool -import -alias server -keystore client1.ts -file server_cert
Enter keystore password:
Re-enter new password:
Owner: CN=Yang Wang, OU=Hortonworks, O=PS, L=Perth, ST=WA, C=AU
Issuer: CN=Yang Wang, OU=Hortonworks, O=PS, L=Perth, ST=WA, C=AU
Serial number: 690819ce
Valid from: Fri Oct 05 00:30:01 AWST 2018 until: Thu Jan 03 00:30:01 AWST 2019
Certificate fingerprints:
	 MD5:  A4:14:3A:C8:6F:6E:C6:4C:2F:BD:9F:42:95:C2:28:1A
	 SHA1: 8F:27:58:EB:25:ED:9B:44:6D:E4:16:0B:5E:B9:2F:81:17:A9:DD:36
	 SHA256: E4:3A:A3:FE:BA:6E:63:EE:4E:56:9E:13:B9:DF:FD:4A:AC:1F:11:51:C7:0F:94:A4:13:53:17:A3:44:87:08:BC
Signature algorithm name: SHA256withRSA
Subject Public Key Algorithm: 2048-bit RSA key
Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 4A 72 44 BE 76 31 4E 77   EA 5D 7A A7 B3 EB 70 5C  JrD.v1Nw.]z...p\
0010: 90 EE 5D 2A                                        ..]*
]
]

Trust this certificate? [no]:  y
Certificate was added to keystore
```

```
>keytool -genkey -alias client -keyalg RSA -keyStore client1.ks
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Yang Wang
What is the name of your organizational unit?
  [Unknown]:  Hortonworks
What is the name of your organization?
  [Unknown]:  PS
What is the name of your City or Locality?
  [Unknown]:  Perth
What is the name of your State or Province?
  [Unknown]:  WA
What is the two-letter country code for this unit?
  [Unknown]:  AU
Is CN=Yang Wang, OU=Hortonworks, O=PS, L=Perth, ST=WA, C=AU correct?
  [no]:  y

Enter key password for <client>
	(RETURN if same as keystore password):
Re-enter new password:

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore client1.ks -destkeystore client1.ks -deststoretype pkcs12".
```


```
>keytool -export -alias client -keystore client1.ks -file client_cert
Enter keystore password:
Certificate stored in file <client_cert>

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore client1.ks -destkeystore client1.ks -deststoretype pkcs12".
```


```
>keytool -import -alias client -keystore server1.ts -file client_cert
Enter keystore password:
Re-enter new password:
Owner: CN=Yang Wang, OU=Hortonworks, O=PS, L=Perth, ST=WA, C=AU
Issuer: CN=Yang Wang, OU=Hortonworks, O=PS, L=Perth, ST=WA, C=AU
Serial number: 5ecdda88
Valid from: Fri Oct 05 00:35:13 AWST 2018 until: Thu Jan 03 00:35:13 AWST 2019
Certificate fingerprints:
	 MD5:  AC:54:F3:5F:A7:CC:B4:6B:83:6F:E2:A7:E6:64:D6:98
	 SHA1: A8:E6:28:3E:7F:B3:10:27:56:AE:2A:8D:E5:A8:9C:94:40:8E:C9:E8
	 SHA256: C8:E0:45:2A:79:77:66:58:2F:76:A4:8A:7B:55:7E:5C:B1:E3:45:7E:7D:D1:6B:10:8B:22:0E:1C:4D:96:61:DE
Signature algorithm name: SHA256withRSA
Subject Public Key Algorithm: 2048-bit RSA key
Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 9D 4D 4B 1D C8 1D A0 2F   EB 57 F4 C8 5E 87 C9 2E  .MK..../.W..^...
0010: B1 AE 65 32                                        ..e2
]
]

Trust this certificate? [no]:  y
Certificate was added to keystore
```



* Summary
  * You will have 6 files
    * server1.ks
    * server_cert
    * client1.ts
    * client1.ks
    * client_cert
    * server1.ts
  * Procedure
    1. Generate Server Private Key in Server KeyStore
    2. Generate Server Certificate
    3. Import Server Certificate into Client TrustStore
    4. Generate Client Private Key in Client KeyStore
    5. Generate Client Certificate
    6. Import Client Certificate into Server TrustStore
  * At last, client and server only need `TrustStore` and `KeyStore`


* Good to read and ref:
  * One Way and Two Way SSL authentication
    * http://alvinhu.com/blog/2013/06/20/one-way-and-two-way-ssl-authentication/
  * Everything You Ever Wanted to Know About SSL (but Were Afraid to Ask)
    * https://www.oschina.net/translate/everything-you-ever-wanted-to-know-about-ssl?cmp
  * SSL/TLS protocol and OpenSSL Tool
    * https://www.jianshu.com/p/da65e5cd552e

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

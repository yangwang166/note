# Call NiFI API with client certificate

When NiFi secured with LDAP, and can already use username password access NiFi api:

```
token=$(curl 'https://host:port/nifi-api/access/token' -H 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8' --data 'username=your_username&password=your_password' --compressed --insecure)
curl -k -X GET "https://host:port/nifi-api/flow/cluster/summary" -H "Authorization: Bearer $token"
```

But if you don't want to expose your username & password, you can invoke nifi api with client certificate

```
keytool -importkeystore -srckeystore host.keystore.jks -destkeystore host.keystore.pkcs12 -deststoretype pkcs12
openssl pkcs12 -in host.keystore.pkcs12 -nocerts -out newkey.pem
openssl rsa -in newkey.pem -out newserver.key
openssl pkcs12 -in host.keystore.pkcs12 -clcerts -nokeys -out newcert.pem
```

Then call

```
curl -k -X GET "https://host:port/nifi-api/flow/cluster/summary" --cert newcert.pem --key newserver.key
```

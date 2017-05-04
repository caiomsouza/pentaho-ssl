# pentaho-ssl
Pentaho SSL

# Config

```
cd /home/pentaho
keytool -genkey -alias pentaho -keyalg RSA
```

# You can view the certificate with the following command:
```
cd /home/pentaho
keytool -list -keystore .keystore
```

# Test 
```
curl -k https://localhost:8443
```


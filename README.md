# Pentaho SSL

Env: Pentaho Server EE 7<BR>

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

# Execute the following command in the directory where the keystore file is located to export the certificate:
```
keytool -export -alias pentaho -file pentaho.cer -storepass pentaho -keypass pentaho -keystore .keystore
```

# Browse to \biserver-ee\tomcat\conf\ & uncomment the "SSL HTTP/1.1 Connector" entry in server.xml and tweak as necessary:

CHANGE FROM<BR>

```
<!-- Define a SSL HTTP/1.1 Connector on port 8443
This connector uses the JSSE configuration, when using APR, the
connector should be using the OpenSSL style configuration
described in the APR documentation -->
<!--
<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
maxThreads="150" scheme="https" secure="true"
clientAuth="false" sslProtocol="TLS" /> -->
<!-- Define an AJP 1.3 Connector on port 8023 -->
<Connector port="8023" protocol="AJP/1.3" redirectPort="8443" />
```
TO: <BR>

```
<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
maxThreads="150" scheme="https" 
keystoreFile="/app/.keystore" keystorePass="changeit" 
secure="true" connectionTimeout="240000" 
clientAuth="false" sslProtocol="TLS" />
<!-- Define an AJP 1.3 Connector on port 8023 -->
<Connector port="8023" protocol="AJP/1.3" redirectPort="8443" />
```

> port 8080 <BR>
CHANGE FROM: <BR> 

```
<!-- A "Connector" represents an endpoint by which requests are received
and responses are returned. Documentation at :
Java HTTP Connector: /docs/config/http.html (blocking & non-blocking)
Java AJP Connector: /docs/config/ajp.html
APR (HTTP/AJP) Connector: /docs/apr.html
Define a non-SSL HTTP/1.1 Connector on port 8080
-->
<Connector URIEncoding="UTF-8" port="8080" protocol="HTTP/1.1"
connectionTimeout="20000"
redirectPort="8443" />
```

TO (comment out this Connector): <BR>

```
<!--
<Connector URIEncoding="UTF-8" port="8080" protocol="HTTP/1.1"
connectionTimeout="20000"
redirectPort="8443" />
-->
```

- TrustedIpAddrs <BR>

```
<filter>
<filter-name>Proxy Trusting Filter</filter-name>
<filter-class>org.pentaho.platform.web.http.filters.ProxyTrustingFilter</filter-class>
<init-param>
<param-name>TrustedIpAddrs</param-name>
<param-value>127.0.0.1,<IP_ADDRESS_OF_PENTAHO_SERVER>,0:0:0:0:0:0:0:1(%.+)*$</param-value>
<description>Comma separated list of IP addresses of a trusted hosts.</description>
</init-param>
</filter>
```

# START BA SERVER and type the following in the web browser:
https://<IP_ADDRESS_OF_PENTAHO_SERVER>:8443


# Test 
```
curl -k https://localhost:8443
```


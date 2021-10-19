# How to Fix javax.net.ssl.SSLHandshakeException: unable to find valid certification path to requested target in Java

Read more: https://www.java67.com/2019/06/javaxnetsslsslhandshakeexception-PKIX-path-building-failed-ssl-certificate.html#ixzz79jSPwU4b

Some times a target the ssl server public key certificate may change and your applicationmay fail producing the following error:

```error
javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException:
 PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException:
 unable to find valid certification path to requested target
 ```
 This means the server does not have a public key that corresponds to the certificate produced by a server hosting a service to validate the cefrtificate. This regard its necessary that the appropriate public key is impirted into the JRE being used by the client/requesting application.
 
1. Make sure you use the JRE being used by application.
2. Download the public key from the host using the following commoand:

```ssh
openssl s_client -showcerts -connect apps.infinityworx.co:443
```
Use the certificate that has "i:/C=AT" making sure you include -----BEGIN CERTIFICATE----- and -----END CERTIFICATE-----

save the file eg root.cert and run the following command

```ssh
/Library/Java/JavaVirtualMachines/jdk-11.0.1.jdk/Contents/Home/bin/keytool -importcert -file ~/code/project/farm_app/farm_services/root.crt -keystore lib/security/cacerts -alias app.infinityworx.co

```
 


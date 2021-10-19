# How to Fix javax.net.ssl.SSLHandshakeException: unable to find valid certification path to requested target in Java

Read more: https://www.java67.com/2019/06/javaxnetsslsslhandshakeexception-PKIX-path-building-failed-ssl-certificate.html#ixzz79jSPwU4b

Some times a target the ssl server public key certificate may change and your applicationmay fail producing the following error:

```code```
javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException:
 PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException:
 unable to find valid certification path to requested target
 ```code```


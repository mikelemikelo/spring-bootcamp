Setup tomcat:
```
https://tomcat.apache.org/download-90.cgi
```

Tomcat users:
```
<role rolename="manager-gui"/>
<user username="admin" password="admin" roles="manager-gui"/>
```

Create local folder:
```
mkdir ~/.uaa
```

Define path to conf:
```
export UAA_CONFIG_PATH=~/.uaa
```

Download default uaa.yml
```
https://raw.githubusercontent.com/cloudfoundry/uaa/4.30.0/uaa/src/main/resources/required_configuration.yml
```

Define Spring Profile:
```
export SPRING_PROFILES="default,hsqldb"
```

Generate JWT keys:
```
openssl genrsa -out signingkey.pem 2048
openssl rsa -in signingkey.pem -pubout -out verificationkey.pem
```



Download War:
```
http://central.maven.org/maven2/org/cloudfoundry/identity/cloudfoundry-identity-uaa/4.30.0/
```

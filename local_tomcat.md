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

Load the JWT into memory:
```
export JWT_TOKEN_SIGNING_KEY=$(cat signingkey.pem)
export JWT_TOKEN_VERIFICATION_KEY=$(cat verificationkey.pem)
```

Download War:
```
http://central.maven.org/maven2/org/cloudfoundry/identity/cloudfoundry-identity-uaa/4.30.0/
```

Change name to uaa.war

Start tomcat:
```
catalina.sh
```
Download and install UAAC -> Uaa CLI tool:

```
uaac target http://localhost:8080/uaa
```

We need to authenticate as Admin, to start using UAA.

```
uaac token client get admin -s adminsecret
```



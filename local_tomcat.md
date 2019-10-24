Roles:

```
   OAuth defines four roles:

   resource owner
      An entity capable of granting access to a protected resource.
      When the resource owner is a person, it is referred to as an
      end-user.

   resource server
      The server hosting the protected resources, capable of accepting
      and responding to protected resource requests using access tokens.

   client
      An application making protected resource requests on behalf of the
      resource owner and with its authorization.  The term "client" does
      not imply any particular implementation characteristics (e.g.,
      whether the application executes on a server, a desktop, or other
      devices).

   authorization server
      The server issuing access tokens to the client after successfully
      authenticating the resource owner and obtaining authorization.

   The interaction between the authorization server and resource server
   is beyond the scope of this specification.  The authorization server
   may be the same server as the resource server or a separate entity.
   A single authorization server may issue access tokens accepted by
   multiple resource servers.
```




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
catalina.sh run
```
Download and install UAAC -> Uaa CLI tool:

```
uaac target http://localhost:8080/uaa
```

We need to authenticate as Admin, to start using UAA.

```
uaac token client get admin -s adminsecret
```


Now, operating as admin, we can register a client named webappclient with client add:

```
uaac client add webappclient -s webappclientsecret \ 
--name WebAppClient \ 
--scope resource.read,resource.write,openid,profile,email,address,phone \ 
--authorized_grant_types authorization_code,refresh_token,client_credentials,password \ 
--authorities uaa.resource \ 
--redirect_uri http://localhost:8081/login/oauth2/code/uaa
```

Adding a user, defined as appuser with appusersecret.
```
uaac user add appuser -p appusersecret --emails appuser@acme.com

```

Adding two groups:
```
uaac group add resource.read
uaac group add resource.write
```

And finally, we'll assign these groups to appuser with member add:
```
uaac member add resource.read appuser
uaac member add resource.write appuser
```

Create the Spring Boot Client application: (run it on 8081)

Application.properties
```
#registration
spring.security.oauth2.client.registration.uaa.client-id=webappclient
spring.security.oauth2.client.registration.uaa.client-secret=webappclientsecret
spring.security.oauth2.client.registration.uaa.scope=resource.read,resource.write,openid,profile

#provider
spring.security.oauth2.client.provider.uaa.issuer-uri=http://localhost:8080/uaa/oauth/token


#Updating port to 8081 , since 8080 its being used by out Identity Server
server.port=8081

```

Spring boot app:
```
package com.modelop.bootcamp;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```


POM file:
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.modelop.bootcamp</groupId>
    <artifactId>oath2-client</artifactId>
    <version>1.0-SNAPSHOT</version>


    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.6.RELEASE</version>
    </parent>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Finchley.SR2</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-oauth2-client</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```




## For notification Server:

```
uaac client add notification-service -s notification-service-secret --name notification-service --scope resource.read,resource.write,openid,profile,email,address,phone --authorized_grant_types authorization_code,refresh_token,client_credentials,password --authorities uaa.resource --redirect_uri http://localhost:8081/login/oauth2/code/uaa


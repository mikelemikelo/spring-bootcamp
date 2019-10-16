# spring-bootcamp

UAA.

The Data source configuration where the UAA is going to store information about clients, will be using HSQLDB.

HSQLDB - Hyper SQL DB -  Database engine which offers both in-memory and disk-based tables. Both embedded and server modes are available.

Set up the UAA.

Build the Docker image.
```
docker build --tag uaa .
```

Port forward 8090 to local 8888.
```
docker run -p 8888:8090 --name=uaa uaa
```

Download the UUA Server.

By default, UAA reads configuration from uaa.yml on its classpath.

http://localhost:8888/uaa/login

username: user1
password: password

Verify that the uaa is up and running:
```
http://localhost:8888/uaa/info
```

Need to make sure that Ruby is up and running.

```
Ruby -v
```

We need to install uaac:

```
sudo gem install cf-uaac
```

Once, has been installed, we need to run the next command, to point it to our UAA :

```
uaac target http://localhost:8080/uaa
```


Before creating our first client, we need to authenticate a valid user against UAA.

```
```


1 - We need to register a new Client (App with UAA).




JSon Web Token (JWT) - https://jwt.io/


Generating JWT Keys to be used:

```
openssl genrsa -out signingkey.pem 2048
openssl rsa -in signingkey.pem -pubout -out verificationkey.pem

```

The authorization server will sign the JWT with the private key, and our client and resource server will verify that signature with the public key.

we could specify these in uaa.yml via the jwt.token.signing-key and jwt.token.verification-key properties.



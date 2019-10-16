# spring-bootcamp

UAA.

The Data source configuration where the UAA is going to store information about clients, will be using HSQLDB.

HSQLDB - Hyper SQL DB -  Database engine which offers both in-memory and disk-based tables. Both embedded and server modes are available.



Set up the UAA.

Download the UUA Server.


By default, UAA reads configuration from uaa.yml on its classpath.



Build the Docker image.

```
docker build --tag uaa .
```


Port forward 8090 to local 8888.
```
docker run -p 8888:8090 --name=uaa uaa
```


JSon Web Token (JWT) - https://jwt.io/

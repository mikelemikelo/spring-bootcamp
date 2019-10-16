# spring-bootcamp

Set up the UAA.

Download the UUA Server.

```
docker build --tag uaa .
```


Port forward 8090 to local 8888.
```
docker run -p 8888:8090 --name=uaa uaa
```


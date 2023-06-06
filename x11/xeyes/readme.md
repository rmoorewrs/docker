Build the image in this directory with:

```
$ docker build -t xeyes .
```

Run with
```
export my_ip=10.10.11.91
docker run --rm -it -e DISPLAY=$my_ip:0.0 xeyes
```

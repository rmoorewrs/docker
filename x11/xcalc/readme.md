Build the image in this directory with:

```
$ docker build -t xcalc .
```

Run with
```
export MY_IP=10.10.11.91
docker run --rm -it -e DISPLAY=$MY_IP:0.0 xcalc
```

Build the image in this directory with:

```
$ docker build -t xcalc .
```

To run on WSL2 on Windows 10/11 with an X11 server running
```
export MY_IP=<Windows Host IP address>
docker run --rm -it -e DISPLAY=$MY_IP:0.0 xcalc
```

To run on a Linux machine
```
docker run --rm -it --net=host -e DISPLAY xcalc

Note: it may be necessary to run xhost + first
$ xhost +
```

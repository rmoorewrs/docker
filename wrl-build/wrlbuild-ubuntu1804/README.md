# wrlbuild-ubuntu1804

This dockerfile builds an image capable of building older versions of Wind River Linux LTS on newer Linux hosts, based on Ubuntu 18.04

## Requirements
You must have docker installed and be able to run it without root privelege. i.e. you should be a member of the 'docker' group and be able to run the following without `sudo`
```
$ docker run --rm hello-world
```
---

## Build instructions:

Enter the directory with the Dockerfile and run:
```
$ . ./build.sh
```
---

## Recommended use:

### Option 1: Create an alias in `~/.bash_aliases`

Examples:
```
alias wrlbuild='export WRL_MIRROR=/path/to/mirror; docker run --rm -it --workdir $(pwd) -u wrlbuild -e WRL_MIRROR=$WRL_MIRROR -e UID=$(id -u) -e GID=$(id -g) -e LANG=en_US.UTF-8 -v $WRL_MIRROR:$WRL_MIRROR -v $(pwd):$(pwd) wrlbuild-ubuntu1804'

alias lts18shell='export WRL_MIRROR=/path/to/LTS18_mirror; docker run --rm -it --workdir $(pwd) -u wrlbuild -e WRL_MIRROR=$WRL_MIRROR -e UID=$(id -u) -e GID=$(id -g) -e LANG=en_US.UTF-8 -v $WRL_MIRROR:$WRL_MIRROR -v $(pwd):$(pwd) wrlbuild-ubuntu1804'
alias lts19shell='export WRL_MIRROR=/path/to/LTS19_mirror; docker run --rm -it --workdir $(pwd) -u wrlbuild -e WRL_MIRROR=$WRL_MIRROR -e UID=$(id -u) -e GID=$(id -g) -e LANG=en_US.UTF-8 -v $WRL_MIRROR:$WRL_MIRROR -v $(pwd):$(pwd) wrlbuild-ubuntu1804'
```
> Note: set WRL_MIRROR to the location of the local mirror for your version of WR Linux. Remember to source `~/.bash_aliases` the first time after adding alias.

### Option 2: Create a bash function in `~/.bashrc`

Example:
```
wrlbuild() {
    WRL_MIRROR=/path/to/mirror   
    docker run --rm -it \
    -w $(pwd) -u wrlbuild \
    -e WRL_MIRROR=${WRL_MIRROR} \
    -e UID=1000 -e GID=1000 \
    -e LANG=en_US.UTF-8 \
    -v ${WRL_MIRROR}:${WRL_MIRROR} \
    -v $(pwd):$(pwd) \
    wrlbuild-ubuntu1804
}
```
> Note: set WRL_MIRROR to the location of the local mirror for your version of WR Linux. Remember to source `~/.bashrc` the first time after adding function.

---

## How to use
- run `wrbuild` (after sourcing your alias or bashrc file)
- proceed to build your WRL LTS/Yocto platform per normal use, for example:
```
$ git clone --branch WRLINUX_10_19_LTS $WRL_MIRROR/wrlinux-x
$ ./wrlinux-x/setup.sh --machines=qemux86-64 --distros=wrlinux --accept-eula=yes
```
- exit the shell when you're done
- invoke the shell whenever you need to build WR Linux



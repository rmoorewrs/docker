# wrlbuild-ubuntu1604

This dockerfile is used to build an image capable of building Wind River Linux LTS, based on Ubuntu 16.04

## Requirements
- you must have docker installed and be able to run it without root privelege. i.e. you should be a member of the 'docker' group


Build instructions:

Enter the directory with the Dockerfile and run:
```
$ . ./build.sh
```

Recommended use:

- Create an alias in ~/.bash_aliases like this and source ~/.bash_aliases

```
alias wrbuild-ubuntu1604='docker run --rm -it --workdir $(pwd) -u wrlbuild -e UID=$(id -u) -e GID=$(id -g)  -e LANG=en_US.UTF-8 -v $(pwd):$(pwd) wrlbuild-ubuntu1604'
```

- Enter a directory above the level of your LTS mirror and your workspace
- run `wrbuild-ubuntu1604` (after sourcing your alias file, e.g. `~/.bash_aliases`)
- proceed to build your WRL LTS/Yocto platform per normal use
- exit the shell when you're done

### Caveat:
This Dockerfile takes the  UID and GID of the builder, so each user needs to build it themselves or override the UID/GID in the alias/command line. i.e. by adding "-e UID=$(id -u) -e GID=$(id -g)"

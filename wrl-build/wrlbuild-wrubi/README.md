# wrlbuild-wrubi

This dockerfile is used to build an image capable of building Wind River Linux LTS, based on Wind River Linux UBI, LTS22

## Requirements
- you must have docker installed and be able to run it without root privelege. i.e. you should be a member of the 'docker' group


Build instructions:

Enter the directory with the Dockerfile and run:
```
docker build . -t wrlbuild-wrubi
```

Recommended use:

- Create an alias in ~/.bash_aliases like this and source ~/.bash_aliases

```
alias wrbuild-wrubi='docker run --rm -it --workdir $(pwd) -u wrlbuild -v $(pwd):$(pwd) wrlbuild-wrubi'
```

- Enter a directory above the level of your LTS mirror and your workspace
- run `wrlbuild-wrubi`
- proceed to build your WRL LTS/Yocto platform per normal use
- exit the shell when you're done

### Caveat:
This Dockerfile assumes your UID is 1000. If this is not the case, then change the UID in the Dockerfile to match your UID.
# Creating a new Mamba Environment



## Installation. 

Information on how to install Mamba can be be found <a href='https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html'>here </a>. 

Please ensure that you have the latest `curl` and `tar` versions installed and then download the relevant files:

```bash 
# Linux Intel (x86_64):
curl -Ls https://micro.mamba.pm/api/micromamba/linux-64/latest | tar -xvj bin/micromamba
# Linux ARM64:
curl -Ls https://micro.mamba.pm/api/micromamba/linux-aarch64/latest | tar -xvj bin/micromamba
# Linux Power:
curl -Ls https://micro.mamba.pm/api/micromamba/linux-ppc64le/latest | tar -xvj bin/micromamba
# macOS Intel (x86_64):
curl -Ls https://micro.mamba.pm/api/micromamba/osx-64/latest | tar -xvj bin/micromamba
# macOS Silicon/M1 (ARM64):
curl -Ls https://micro.mamba.pm/api/micromamba/osx-arm64/latest | tar -xvj bin/micromamba```
```

### Setting aliases and activating
Start by activating your micromamba installation
```
./micromamba shell init
```
This adds it to our .rc file and allows us to choose a custom mamba environment 

```
./bin/micromamba shell init -s bash -p ~/micromamba
or
./micromamba shell init -s zsh -p ~/micromamba
```

Don't forget to `source` the respective rc file. 


## Creating a new environment

``` bash
# create a new env
mamba create --name <envname>

#initiate mamba
mamba init

# activate our environment
mamba activate <envname>
```











ca mipcvs

# pip install fastapi-sso

mamba insall uvicorn fastapi itsdangrous requests

pip install fastapi-login
# Setup and Compilation

### Mamba

#### Install 
To install mkdocs, we can use mamba to create a enw environment. 
```bash
mamba create --name docs

mamba activate docs

mamba init

mamba install mkdocs mkdocs-material pymdown-extensions
```

#### Build
```bash 
mkdocs build --clean
```

### Docker

#### Install
```docker pull squidfunk/mkdocs-material```

#### Creating a site. (Dont run)
```docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material new .```

#### Preview 
```docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material```

#### Build 
```docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material build```


## Resources
### Demo files and structure: 
https://github.com/selfhostedshow/wiki/tree/master
### Permissable items
https://squidfunk.github.io/mkdocs-material/reference/admonitions/



### Developer notes
The following are Notes and will be tidied away in due course. 

https://squidfunk.github.io/mkdocs-material/getting-started/
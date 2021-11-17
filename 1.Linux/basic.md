## Basic git scripts

### Show all configurations
```
$ git config --list
```

### Config global account
```bash
// setter
git config --global user.name gloolo
git config --global user.email gloolo@example.com

// getter
$ git config --global user.name
$ git config --global user.email
``` 

### Config local account
```bash
// setter
git config --local user.name gloolo
git config --local user.email gloolo@example.com

// getter
$ git config --local user.name
$ git config --local user.email
```
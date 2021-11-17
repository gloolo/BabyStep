
When I generate a new SSH keychain, there's some error when I try to push local branch to remote main branch. Here's the details:
```
$ git push origin main
ERROR: Permission to gloolo/BabyStep.git denied to antwork.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

## Fix solution
Add custom config to github.com
If there's no `~/.ssh/config` file, you can create a new one. If there exist a old one, you can add contents like below: 
```shell
Host github.com
    User git
    IdentityFile /Users/xuq/.ssh/id_ed25519
    IdentitiesOnly yes
```

About the keynames
| keyword | comment |
| ------- | ------- |
| github.com | your remote git repository|
|User git | application name, here's git.
|IdentityFile | path for the private certification
|IdentitiesOnly | yes means use the specific private certification.

## Links
* [https://segmentfault.com/a/1190000005349818](https://segmentfault.com/a/1190000005349818)
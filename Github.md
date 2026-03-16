## Introduction

Here are some usefull infos about Github and how to use it.

## Configurations

``` bash
# Get started: Name, Email, Editor 
git config --global user.name "Example Name"
git config --global user.email "email@example.com"
git config --global core.editor "vim"
```

### Tokens

Go to Settings / Developer Settings / Personal access tokens

Create a Token with the needed permissions.

<https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens>

### Default text editor {#default_text_editor}

``` bash
git config --global core.editor "vim"
```

or

``` bash
vi ~/.gitconfig

-----------------------
[core]
        editor = vim
-----------------------
```

[Category:Git](Category:Git "Category:Git"){.wikilink}

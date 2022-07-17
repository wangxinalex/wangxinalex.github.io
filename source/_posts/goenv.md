---
title: goenv
date: 2022-07-17 22:54:49
tags:
---

# installation on Apple arm chip

- download

```bash
brew uninstall goenv
brew install --HEAD goenv # latest
```

- configure in `.zshrc`

```bash
export GOENV_ROOT="$HOME/.goenv"
export PATH="$GOENV_ROOT/bin:$PATH"
eval "$(goenv init -)"
export PATH="$GOROOT/bin:$PATH"
export PATH="$PATH:$GOPATH/bin"
```

- install and set golang version

```bash
goenv install --list # list all available golang versions
goenv install 1.18.3
goenv global 1.18.3 # set golang version globally
```

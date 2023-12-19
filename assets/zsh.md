# Zsh & Starship in Windows Git Bash

## Install Zsh

1. Download MSYS2 zsh package, it's named like [zsh-5.9-2-x86_64.pkg.tar.zst](https://mirror.msys2.org/msys/x86_64/zsh-5.9-2-x86_64.pkg.tar.zst). [MSYS2 package repository](https://packages.msys2.org/package/zsh?repo=msys&variant=x86_64) is here.
2. PeaZip or 7-Zip Beta to open ZST extension's file.
3. Extract zsh compression into Git Bash installation directory. `C:\Program Files\Git` is usually path. Windows may prompt if merge contents, select yes.
4. Open Git Bash, run `zsh` to confirm zsh has been installed correctly.
5. To set up Zsh as the default shell, edit `~/.bashrc` file:

   ```bash
   if [ -t 1 ]; then
     exec zsh
   fi

   ```

## Install oh-my-zsh

In the Zsh shell and run:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

```

## Install Plugins

In the Zsh shell and run:

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions
git clone https://github.com/zdharma-continuum/fast-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/fast-syntax-highlighting

```

Edit `~/.zshrc`

```diff
- plugins=(git)
+ plugins=(
+  git
+ zsh-completions
+ zsh-autosuggestions
+ fast-syntax-highlighting
+ )

```

## Install Starship

In the Zsh shell and run:

```bash
curl -sS https://starship.rs/install.sh | sh -s -- --bin-dir ~/.config/starship

```

Edit `~/.zshrc` file:

```plaintext
eval "$(starship init zsh)"
```

Restart zsh and enjoy it.

## User profile

`nvim ~/.profile`

```shell
home="~"

# alias
alias ~="cd ${home}"
alias cls="clear && history -c"
alias his="history"
alias n="nvim"
alias c="code"
alias t="touch"
alias m="mkdir"
alias h="history"
alias ws="winget search"
alias wl="winget list"
alias wi="winget install"
alias wu="winget upgrade"
alias wuiu="winget upgrade --include-unknown"
alias e="explorer.exe"
alias gac="git add -A && git commit"
alias galias="nvim ~/.oh-my-zsh/plugins/git/git.plugin.zsh"
alias gosrc="cd ${home}/go/src"

function sd() {
 shutdown -s -t ${1:-7000}
}

# start proxy
proxy () {
 export https_proxy=http://127.0.0.1:2080 http_proxy=http://127.0.0.1:2080 all_proxy=socks5://127.0.0.1:2080
 echo "HTTP Proxy on"
}
# close proxy
noproxy () {
 unset http_proxy
 unset https_proxy
 unset all_proxy
 echo "HTTP Proxy off"
}

```

`nvim .zshrc`

```diff
  # starship config
  eval "$(starship init zsh)"
  starship preset plain-text-symbols > ~/.config/starship.toml

  # fnm node env config
  eval "$(fnm env --use-on-cd)"

+ # Load user profile
+ source ~/.profile

+ # cursor keep solid display, put into end of .zshrc
+ echo -e -n "\e[2 q"


```

## Recommend Starship Presets

Select an [Presets](https://starship.rs/presets/) to custom Starship.

## Reference

[Installing Zsh (and oh-my-zsh) in Windows Git Bash](https://dominikrys.com/posts/zsh-in-git-bash-on-windows/)

[linux-like-windows-terminal](https://github.com/Kyza/linux-like-windows-terminal)

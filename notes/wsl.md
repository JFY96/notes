# Windows Subsystem for Linux

## Installing

`wsl --install`

Should install ubuntu too

### Check install

`wsl -l -v`

Should see something like
```
  NAME      STATE       VERSION
* Ubuntu    Running     2
```

### Git

- Open a WSL Terminal
- Check git is installed `git -v`, else `sudo apt-get install git` and [see git setup](./git.md)
- Add git user details (replace <>)
    - `git config --global user.name "<Your Name>"`
    - `git config --global user.email "<Your Email>"`
- Copy SSH Key setup from windows (Replace <>) else to set up from scratch [see git setup](./git.md) 
    - `cd`
    - `cp -r /mnt/c/Users/<Your Windows User>/.ssh ~/`
    - `chmod 600 ~/.ssh/id_rsa` or `chmod 600 ~/.ssh/id_ed25519` depending on key type
- Test with `ssh -T git@github.com`

### Zsh

https://github.com/ohmyzsh/ohmyzsh

If prefer to use `Zsh` over bash:
- `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
    - or if `raw.githubusercontent.com` is blocked, `sh -c "$(wget -O- https://install.ohmyz.sh/)"`
    - if there are errors with certificate, try
    ```
    openssl s_client -showcerts -servername github.com -connect github.com:443 </dev/null 2>/dev/null | sed -n -e '/BEGIN\ CERTIFICATE/,/END\ CERTIFICATE/ p'  > github-com.pem
    cat github-com.pem | sudo tee -a /etc/ssl/certs/ca-certificates.crt
    ```
- Then check installed `zsh --version`
- Make sure it is in your list of authorized shells `cat /etc/shells` and make it default shell if desired
- Restart system or WSL
- Test it worked with `echo $SHELL` and `$SHELL --version`

#### Theme

To install the pi theme:
- check `ls $ZSH_CUSTOM/themes` exists else `mkdir $ZSH_CUSTOM/themes`
- `wget -O $ZSH_CUSTOM/themes/pi.zsh-theme https://raw.githubusercontent.com/tobyjamesthomas/pi/master/pi.zsh-theme`
- `nano ~/.zshrc` and update `ZSH_THEME="pi"`

## docker

After installing docker desktop:
- go to settings and then "Resources" > "WSL integration"
- Turn on "Enable integration with additional distros: Ubuntu"`


## tips

- in bash, do `explorer.exe .` to open the current directory in windows file explorer
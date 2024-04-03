## Let Tim Cook!!

This is a simple setup env for a development env on mac OS with core essential tools installation and configurations

### Install Homebrew
Just do it, its great
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Setup docker (with colima)
```
brew install colima
```

#### Start up colima 
You may also add this to your `bashrc` or `zshrc` so that on start up colima is started
```
colima start --cpu 8 --memory 16
```
With this config colima will allocate
* 8 CPU cores
* 16 GB of memory

After this you may verify the installtion with `docker version`.

#### Notes
Some plugins in build tools will still look for the `docker` binary in the `/usr/local/bin` path

So its recommended to copy the binary from homebrew to the path for compatibility with those build tools
```
which docker
```

Once the path is found just use `sudo` and the `cp` command to copy the `docker` binary over to the `/usr/local/bin` path.

### Install docker-compose
```
brew install docker-compose
```

#### Notes
Some plugins in build tools will still look for the `docker-compose` binary in the `/usr/local/bin` path

So its recommended to copy the binary from homebrew to the path for compatibility with those build tools
```
which docker-compose
```

Once the path is found just use `sudo` and the `cp` command to copy the `docker-compose` binary over to the `/usr/local/bin` path.


#### Limitations
Some build tools once the system-test is ran and completed with docker-compose, the log may not work as the `docker-compose` binary used on the Apple Silicon which is of `aarch64` is not available on older `docker-compose` versions.

### Setup Tmux
```
brew install tmux
```
#### Update tmux config with pbcopy
```
bind-key -T copy-mode-vi y send -X copy-pipe-and-cancel 'pbcopy'
```
### Setup Neovim
Install neovim binary the same.

#### Install ripgrep
```
brew install ripgrep
```

### Oh-My-Zsh custom theme (Fixing execution time)
#### Ensure that coreutils is installed
```
brew install coreutils
```
#### Update script
Update the command that calculates the execution time with this function
```
precmd() {
    if [ -n "$_ZSH_CMD_DURATION_START" ]; then
        _ZSH_CMD_DURATION_END=$(gdate +%s.%3N)
        _ZSH_CMD_DURATION=$(awk "BEGIN {printf \"%.3f\", ${_ZSH_CMD_DURATION_END} - ${_ZSH_CMD_DURATION_START}}")
        unset _ZSH_CMD_DURATION_START
    fi
}
```

### Installing Kitty for the terminal
Why kitty? because it can remap mac's `option` key to `alt` easier.
```
brew install --cask kitty
```

#### Configuring kitty for alt key for neovim keybinds
In `~/.config/kitty/kitty.conf` find the config below and update it to `yes`
```
macos_option_as_alt yes
```

### Running old architecture stuff
Its possible to use older build tools without binaries for apple silicon. 

#### Install rosetta
Not the pokemon...
```
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
```

#### After this follow below steps
1* Go to finder
2* Go to `Utilities`
3* Right click `Terminal`
4* Select the option to open terminal with rosetta

#### Using it 
After this just open terminal, and it should be able to run any old cpu architecture binaries, at the cost of performance so should NOT be used for benchmarks

### To set docker buildx in macos
#### Install via homebrew
```
brew install docker-buildx
```
#### Set the link to the plugin
```
mkdir ~/.docker/cli-plugins
ln -sfn $(which docker-buildx) ~/.docker/cli-plugins/docker-buildx
```
#### Set the docker_host to colima if using colima
```
export DOCKER_HOST="unix://$HOME/.colima/docker.sock"
```

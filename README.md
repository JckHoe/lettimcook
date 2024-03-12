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

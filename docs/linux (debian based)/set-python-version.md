# Python

## install using pyenv

```shell
# install and configure path
brew install pyenv
echo "$(pyenv init --path)" >> ~/.zshrc

# list installable versions
pyenv install --list

# install
pyenv install 3.9.16 
pyenv install 2.7.18
# list installed versions
pyenv versions

# set path for python executables
pyenv global 3.9.16 2.7.18

# check versions
python2 --version
python3 --version
python --version
```

## create minimal webserver with python

```shell
# serve current dir on port 8000
python3 -m http.server

# serve current dir on custom port
python3 -m http.server 9999

# serve custom dir on custom port
python3 -m http.server 8001 --directory /opt/oh/my/content
```

## set python 3 as default

The last number is the priority. Greater number is higher priority. 

```shell
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 1
 
```

If you want to check the current config run `sudo update-alternatives --config python`

## install pip for python3

```shell
sudo apt install python3-pip
```

# Ports

## check open ports on local machine

```shell
## all ports
sudo ss -tulnp
# or
sudo netstat -tulnp
# or
sudo lsof -Pin

## specific port
sudo lsof -i :8080
```

windows:

```cmd
tnc google.com -port 80
# find and kill aplication which occupies port (windows)
netstat -ano | findstr :8080
taskkill /PID 13744 /F
```

## access non http traffic on a port

```shell
nc -vz IP PORT
```

## scan for open ports from remote

```shell
nc -zv -w 1 TARGET_PORT TARGET_PORT

# remote machine: check for open tcp ports (scans the first 1000 ports)
nmap <ip_address>

# remote machine: check for open tcp ports (scans all ports)
nmap -p- <ip_address>

# remote machine: check if tcp port 8080 is open 
nmap -p 8080 <ip_address>
```

## start webserver on port

```shell
# start webserver on port 
mkdir tmp && cd tmp && echo "Hello, you reached Jans webserver" > index.html
python3 -m http.server 8080 
cd -
```

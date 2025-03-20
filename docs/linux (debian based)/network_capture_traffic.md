# Capture network traffic

```
# list interfaces
sudo tcpdump -D

# capture traffic in asci format on port 80. -n disables name resolution
sudo tcpdump -i eth0 -A -n port 80
```
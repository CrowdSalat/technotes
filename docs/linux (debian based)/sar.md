# sar - Linux performance measurement 

```bash
# sar
 
## show cpu for whole day
sar
 
## cpu: every second for the next 5 intervals/seconds
sar -u 1 5
 
## ram
### %commit - memory usage (inclduing swap)
### %memusage memory including virtual memory
sar -r 1 5

 
## disk
### %util - approximate usage of disks. Not reliable for ssds or raid systems
sar -d 1 5

 
## network
sar -n ALL 1 5
 
## combined
sar -urb 1 5
sar -urbn ALL 1 5
 
# save in file
sar -urb 1 5 -o ./sarout.file
 
# save to file and run in backround
sar -urb 1 5 -o ./sarout.file >/dev/null 2>&1 &
 
# sadf
 
## csv
# sadf -dh -- <sar command>
sadf -dh -- -urb
 
## json
# sadf -j -- <sar command>
sadf -j -- -urb
 
## create sar file and read it into csv
 
sar -urb 1 5 -o ./sarout.file
sadf -dh sarout.file > sarout.csv
 
 
## create sar file and read it into csv in backround
 
sar -urb 1 5 -o ./sarout.file >/dev/null 2>&1 &
sadf -dh sarout.file > sarout.csv &
```
# scanning_IPs
collection of ways to scan IP addresses for sshing in bash:

When connecting a new rasperry pi system to a network, without hooking up a monitor it can be annoying to figure out which ip address it connected to so you can ssh into it. To aid in discovering, I collated a few ways to scan through a range of IP addresses for sshing (port 22). 

set your IP prefix (in my case 192.168.100):

`IPprefix=192.168.100`

## Option 1 (with nmap):

`nmap -sP $IPprefix.0/24`

## Option 2 (with nc):



## Option 3 (with ping):

`parallel -j0 ' if [[ $( ping -c 1 '$IPprefix'.{} | grep -c "0 received" )  -eq 0 ]]; then echo {}; fi ' ::: {0..254}`

This should spit out those octets that are open (255 isn't including as routers typically reserve it for broadcasting)

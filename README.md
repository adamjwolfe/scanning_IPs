# scanning_IPs - A collection of fast ways to scan IP addresses for open connections:

Motivation: When I connect a new rasperry pi system to a network, without hooking up a monitor it can be annoying to figure out which ip address it connected to so I can ssh into it. To aid in discovering, I collated a few ways to scan through a range of IP addresses for open connections:

set your IP prefix (in my case 192.168.100):

`IPprefix=192.168.100`

## Option 1 with nmap:

`nmap -sP $IPprefix.0/24`

## Option 2 with nc:
This will scan for open connections to port 22 (default ssh port). It will exclude those IPs that timed out after 2 seconds.

`parallel -j0 ' if [[ $( nc -zv -w 2 '$IPprefix'.{} 22 2>&1 | grep -E -c "failed|timed" )  -eq 0 ]]; then echo {}; fi ' ::: {0..254}`

This should spit out those octets that are open (255 isn't including as routers typically reserve it for broadcasting)

## Option 3 with ping:

`parallel -j0 ' if [[ $( ping -c 1 '$IPprefix'.{} | grep -c "0 received" )  -eq 0 ]]; then echo {}; fi ' ::: {0..254}`

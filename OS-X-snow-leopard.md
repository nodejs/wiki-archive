error message: (node)Hit maxfile limit. Increase 'ulimit -n'
ulimit does not function as required for os x 10.6

launchctl and launchd control jobs, 
vanilla flavoured instructions: http://blog.ghostinthemachines.com/2010/01/19/mac-os-x-fork-resource-temporarily-unavailable/

$ sudo sysctl -w kern.maxproc=1024   // but see above

and edit .launchd.conf or /etc/launchd.conf and reboot

ulimit may then be raised. if necessary?

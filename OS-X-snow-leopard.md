error message: (node)Hit maxfile limit. Increase 'ulimit -n'

ulimit does not function as required for os x 10.6

launchctl and launchd control jobs, 
vanilla flavoured instructions required...
notwithstanding the man page neither of the following wfm

$ sudo launchctl limit maxproc 512  1024

editing .launchd.conf and rebooting

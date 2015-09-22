# Node.js CI platform's administration guide

## UNIX machines

In order to be able to manage UNIX machines used by the CI platform, you will
need to [sign in to Joyent's Public Cloud with a specific
account](https://my.joyent.com/landing/signin/). Joyent Public Cloud supports
the ability to have users and sub-users. You will need to log as the user
"NodeCore" and the sub-user that was assigned to you, using the following form:

```
NodeCore/subuser
```

Usually, your subuser should follow the following format:

```
xlastname
```

where `x` is the first letter of your first name.

Once logged in, you will have access to all machines used by Node.js' CI platform.

To get access to a remote shell on these machines, you will first need to get
the username and the IP address of the machine you want to connect to. Simply
click on the entry corresponding to that machine and look for the line "Login"
in the "Summary" section (the first ection) in the instance details.

Then just use ssh with the username and IP address provided. If your SSH key is
denied access to one of these machines, please send an email to
team@nodejs.org.

## Windows machines

To access Windows machines, you will need to connect to them using the Remote
Desktop Protocol (RDP). There are RDP clients for most operating systems.

There are currently 3 different Windows machines hosted on Azure that are used
by the CI platform:

* nodejs-2k12, remote desktop available at nodejs-2k12.cloudapp.net:56188. This
is the first machine used to run tests. _However, it is currently disabled
for most jobs since we had performance issues (very long build times) with it
recently_.
* nodejs-2k8, remote desktop avalaible at nodejs-2k8.cloudapp.net:59310. This is
the second Windows machine used to run tests.
* node-build, remote desktop available at node-build.cloudapp.net:50708. This is
the machine that builds official releases. It's not used to run tests or any
other jobs.

For other administration tasks, such as resetting the password used to connect
to it, you should log in to [the Azure portal](ms.portal.azure.com).

## OSX machines

OSX machines are not hosted in a virtualized/cloud platform. Instead, they are actual
physical machines with a public IP. Currently, there is only one such machine
and it's hosted at Joyent's offices. To connect to this machine, you will need
to:

1. Connect to Joyent's VPN.
2. Use SSH to get a shell on that machine.

I'm in the process of providing all that is needed to members of the CI team
to connect to the VPN. In the meantime, you will need to contact me anytime
you need to fix a problem on this platform. Sorry for the inconvenience.
